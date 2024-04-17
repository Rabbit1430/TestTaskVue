<!-- @format -->

<template>
  <div class="mycontent">
    <div class="lift">
      <div class="etag">
        <div class="block"></div>
      </div>
      <div class="navigatelift">
        <div class="buttonetage" v-for="etg in numbersetag" :key="etg">
          <div>{{ etg }}</div>
          <button-lift><div class="circle"></div></button-lift>
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
    };
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
  height: 700px;
  display: flex;

  justify-content: space-around;
}

.etag {
  width: 200px;
  height: 100%;
  border: 2px solid #000;
  display: flex;
  align-items: end;
}

.block {
  width: 100%;
  height: 100px;
  background-color: aqua;
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
