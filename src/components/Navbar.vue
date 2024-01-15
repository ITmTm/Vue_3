<template>
  <div class="navbar">
    <div>Vue 3</div>
    <div class="navbar__btns">
      <my-buttons @click="$router.push('/')" style="background: darkkhaki">На главную</my-buttons>
      <my-buttons @click="$router.push('/post')" style="margin-left: 40px">Посты</my-buttons>
      <my-buttons @click="$router.push('/about')" style="margin-left: 20px">О сайте</my-buttons>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  background-color: lightblue;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
  &__btns {
    margin-left: auto;
  }
}

</style>