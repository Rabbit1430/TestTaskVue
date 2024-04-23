<!-- @format -->

<template>
  <button class="mybutton">
    <slot></slot>
  </button>
</template>

<script>
export default {};
</script>

<style scoped>
.mybutton {
  width: 20px;
  height: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
</style>
