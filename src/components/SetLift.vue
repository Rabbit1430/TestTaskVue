<template>
    <div class="lift">
      <div
        class="block"
        :style="{
          bottom: lift.positionnow + 'px',
          transition: 'bottom ' + (numbersetag - 1) + 's ease',
        }"
        :class="{ block: true, goblock: lift.isgoblock }"
      >
        <span v-if="lift.isactiveup" class="material-symbols-outlined strelka">
          arrow_upward
        </span>
        <span v-if="lift.isactivedown" class="material-symbols-outlined strelka">
          arrow_downward
        </span>
        <span>{{ lift.nowetag }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      lift: {
        type: Object,
        required: true
      },
      numbersetag: {
        type: Number,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
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
  
 
  </style>
  