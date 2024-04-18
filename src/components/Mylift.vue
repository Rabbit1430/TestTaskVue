<!-- @format -->

<template>
  <div class="mycontent">
    <div class="lift">
      <div class="etag">
        <div
          class="block"
          :style="{ bottom: positionnow + 'px' }"
          :class="{ block: true, goblock: isgoblock }"
        ></div>
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
            ><div class="circle"></div
          ></button-lift>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ButtonLift from "@/components/UI/ButtonLift.vue";
export default {
  components: { ButtonLift },
  data() {
    return {
      numbersetag: 5,
      positionnow: 0,
      buttonclick: false,
      isgoblock: false,
      isogidanie: false,
      nowetag: 1,
      ochered: [],
    };
  },

  methods: {
    async GoLift(etg) {
      if (this.ochered.includes(etg)) {
        return;
      }

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

      await new Promise((resolve) => {
        setTimeout(() => {
          this.nowetag = ocher;
          this.positionnow = (ocher - 1) * 100;
          resolve();
        }, distantion);
      });

      await new Promise((resolve) => {
        setTimeout(resolve, distantion + 3000);
      });

      this.ochered.shift();

      this.isgoblock = false;
      await this.gotolift();
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
.lift {
  width: 600px;
  height: 500px;
  display: flex;

  justify-content: space-around;
}

.etag {
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
  transition: bottom 4s ease-in-out;
}

.goblock {
  transition: bottom 4s ease-in-out;
}

.circle {
  width: 20px;
  height: 20px;
  background-color: transparent;
  border-radius: 50%;
  border: 2px solid #000;
}

.navigatelift {
  display: flex;
  flex-direction: column-reverse;
  justify-content: space-around;
}
</style>
