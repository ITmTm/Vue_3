<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-buttons'
}
</script>

<style scoped>
.btn {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
  transition: .6s all ease;
  &:hover {
    transition: .5s all ease;
    background: blue;
    color: aqua;
  }
}
</style>