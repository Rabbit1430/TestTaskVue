<!-- @format -->

<template>
  <div class="mycontent">
    <div
      class="lifttask"
      :style="{
        height: numbersetag * 50 + 'px',
        width: countlift * 250 + 'px',
      }"
    >
      <set-lift  v-for="(lift, index) in lifts"
        :key="index"
        :lift="lift"
        :numbersetag="numbersetag" />
      <div class="navigatelift">
        <div class="buttonetage" v-for="etg in numbersetag" :key="etg">
          <div class="chislo">{{ etg }}</div>
          <button-lift
            @click="
              {
                GoLift(etg);
              }
            "
            ><div
              class="circle"
              :class="{
                active: activelift.includes(etg),
                animationlift: animationlift.includes(etg),
              }"
            ></div
          ></button-lift>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import ButtonLift from "@/components/UI/ButtonLift.vue";
import data from "./etagandlift_config";
import SetLift from './SetLift.vue';
export default {
  components: { ButtonLift , SetLift},
  
  data() {
    return {
      numbersetag: data.etag,
      buttonclick: false,
      activelift: [],
      animationlift: [],
      countlift: data.lift,
      myetag: [],
      lifts: Array.from({ length: data.lift }).map(() => ({
        positionnow: 0,
        ochered: [],
        isgoblock: false,
        isactiveup: false,
        isactivedown: false,
        nowetag: 1,
        isogidanie: false,
        
      })),
      
    };
  },
 

  mounted() {
this.loadstate()
this.continuecode()
  
},

watch: {
    activelift() {
      this.mystate();
    },
    animationlift() {
      this.mystate();
    },
    lifts() {
      this.mystate()
    },
   
    myetag(){
      this.mystate()
    }
  },
  

  methods: {
    
    async GoLift(etg) {

      this.myetag.push(etg);
      // Вычисление ближнего лифта
      const distnclift = this.lifts.map((lift) => Math.abs(lift.nowetag - etg));

const indexdistlift = distnclift.indexOf(Math.min(...distnclift));

  const finallift = this.lifts[indexdistlift];
  

if (finallift.ochered.includes(etg)) {
  return;
}


this.activelift.push(etg);

finallift.ochered.push(etg);


// console.log(this.myetag);

this.mystate();
await this.gotolift(finallift);

    },


    //Движение лифта и анимация кнопок
    async gotolift(finallift) {
      if (finallift.isgoblock || finallift.ochered.length === 0) {
        return;
      }

      finallift.isgoblock = true;
      if (finallift.isogidanie) {
        if (finallift.ochered.includes(ocher)) {
          finallift.isgoblock = false;
          return;
        }
      }

      const ocher = finallift.ochered[0];

      const distance = Math.abs(ocher - finallift.nowetag);

      const distantion = distance * 1000;

      if (finallift.nowetag < ocher) {
        finallift.isactiveup = true;
      }

      if (finallift.nowetag > ocher) {
        finallift.isactivedown = true;
      }

      await new Promise((resolve) => {
        setTimeout(() => {
          finallift.nowetag = ocher;

          finallift.positionnow = (ocher - 1) * 50;
          resolve();
        }, 500);
      });

      await new Promise((resolve) => {
        setTimeout(() => {
          this.activelift.shift();
          finallift.isactiveup = false;
          finallift.isactivedown = false;
          resolve();
        }, distantion + 3000);
      });

      await new Promise((resolve) => {
        setTimeout(() => {
          this.animationlift.push(ocher);
          resolve();
        }, 2000);
      });

      await new Promise((resolve) => {
        setTimeout(() => {
          this.animationlift.shift();
          resolve();
        }, 3000);
      });

      finallift.ochered.shift();

      finallift.isgoblock = false;

      this.myetag.shift()
      this.mystate();
      
      await this.gotolift(finallift);
      
      
    },

 // localStorage
    mystate() {
      const liftstate = this.lifts.map(lift => ({
      positionnow: lift.positionnow,
      ochered: lift.ochered,
      nowetag: lift.nowetag
    }));
    localStorage.setItem('liftstate', JSON.stringify(liftstate));

    localStorage.setItem(
      "animebutton",
      JSON.stringify({
        activelift: this.activelift,
        continue: this.continue,
        myetag: this.myetag
     
      })
    );
  },
  loadstate() {
    const savelift = localStorage.getItem('liftstate');
    if (savelift) {
      const datalift = JSON.parse(savelift);
      datalift.forEach((dtlift, index) => {
        const lift = this.lifts[index];
        if (lift) {
          lift.positionnow = dtlift.positionnow;
          lift.ochered = dtlift.ochered;
          lift.nowetag = dtlift.nowetag;
        }
      });
    }

    const saveanime = localStorage.getItem('animebutton');
    if (saveanime) {
      const anime = JSON.parse(saveanime);
      this.activelift = anime.activelift;
      this.continue = anime.continue;
      this.myetag = anime.myetag;
     
    }

   
  
  },
  
  continuecode() {
 
if(this.myetag.length > 0) {
  this.lifts.forEach(finallift => {
  
      this.gotolift(finallift);
  
  });
}
  }
  },
};
</script>

<style scoped>
.mycontent {
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
}
.lifttask {
  width: 600px;
  height: 250px;
  display: flex;

  justify-content: space-around;
}

.lift {
  width: 200px;
  height: 100%;
  border: 2px solid #000;
  display: flex;

  position: relative;
}

.block {
  position: absolute;
  width: 100%;
  height: 50px;
  background-color: aqua;
  bottom: 0;
  transition: bottom 4s ease;
  display: flex;
  justify-content: center;
  align-items: center;
}

.goblock {
  transition: bottom 4s ease;
}

.circle {
  width: 10px;
  height: 10px;
  background-color: transparent;
  border-radius: 50%;
  border: 2px solid #000;
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.animationlift {
  background-color: #000;
  animation: blink 1s infinite;
}

.navigatelift {
  display: flex;
  flex-direction: column-reverse;
  justify-content: space-around;
}

.active {
  background-color: #000;
}
</style>