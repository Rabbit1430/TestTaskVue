<!-- @format -->

<template>
  <div class="mycontent">
    <div class="lifttask" :style="{ height: numbersetag * 100 + 'px' }">
      <div class="lift">
        <div
          class="block"
          :style="{
            bottom: positionnow + 'px',
            transition: 'bottom ' + (numbersetag - 1) + 's ease',
          }"
          :class="{ block: true, goblock: isgoblock }"
        >
          <span v-if="isactiveup" class="material-symbols-outlined strelka">
            arrow_upward
          </span>
          <span v-if="isactivedown" class="material-symbols-outlined strelka">
            arrow_downward
          </span>
          <span>{{ nowetag }}</span>
        </div>
      </div>
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
export default {
  components: { ButtonLift },
  data() {
    return {
      numbersetag: data.etag,
      positionnow: 0,
      buttonclick: false,
      isgoblock: false,
      isogidanie: false,
      nowetag: 1,
      ochered: [],
      activelift: [],
      animationlift: [],
      isactiveup: false,
      isactivedown: false,
      lifts: data.lift,
    };
  },
  mounted() {
    this.loadstate();
    this.continuecode();
  },

  watch: {
    positionnow() {
      this.mystate();
    },
    nowetag() {
      this.mystate();
    },
    ochered() {
      this.mystate();
    },
    activelift() {
      this.mystate();
    },
    animationlift() {
      this.mystate();
    },
  },

  methods: {
    async GoLift(etg) {
      if (this.ochered.includes(etg)) {
        return;
      }

      this.activelift.push(etg);

      this.ochered.push(etg);
      await this.gotolift();
    },

    async gotolift() {
      if (this.isgoblock || this.ochered.length === 0) {
        return;
      }

      this.isgoblock = true;
      if (this.isogidanie) {
        if (this.ochered.includes(ocher)) {
          this.isgoblock = false;
          return;
        }
      }

      const ocher = this.ochered[0];

      const distance = Math.abs(ocher - this.nowetag);
      const distantion = distance * 1000;

      if (this.nowetag < ocher) {
        this.isactiveup = true;
      }

      if (this.nowetag > ocher) {
        this.isactivedown = true;
      }

      await new Promise((resolve) => {
        setTimeout(() => {
          this.nowetag = ocher;
          this.positionnow = (ocher - 1) * 100;
          resolve();
        }, 500);
      });

      await new Promise((resolve) => {
        setTimeout(() => {
          this.activelift.shift();
          this.isactiveup = false;
          this.isactivedown = false;
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

      this.ochered.shift();

      this.isgoblock = false;

      this.mystate();
      await this.gotolift(this.ochered[0]);
    },

    mystate() {
      localStorage.setItem(
        "liftstate",
        JSON.stringify({
          positionnow: this.positionnow,
          nowetag: this.nowetag,
          ochered: this.ochered,
          activelift: this.activelift,
          animationlift: this.animationlift,
        })
      );
    },
    loadstate() {
      const savestate = localStorage.getItem("liftstate");
      if (savestate) {
        const state = JSON.parse(savestate);
        this.positionnow = state.positionnow;
        this.nowetag = state.nowetag;
        this.ochered = state.ochered;
        this.activelift = state.activelift;
        this.animationlift = state.animationlift;
      }
    },

    continuecode() {
      if (this.ochered.length > 0) {
        this.gotolift(this.ochered[0]);
        this.isactive = false;
      }
    },
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
  height: 500px;
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
  height: 100px;
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
  width: 20px;
  height: 20px;
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
