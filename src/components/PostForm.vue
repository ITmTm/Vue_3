<template>
  <form @submit.prevent>
    <h4>Создание поста</h4>
    <my-input
      v-focus
      v-model="post.title"
      type="text"
      placeholder="Название"
    />
    <my-input
      v-model="post.body"
      type="text"
      placeholder="Описание"
    />
    <my-buttons
      style="align-self: flex-end; margin-top: 15px"
      @click.prevent="createPost"
    >
      Создать
    </my-buttons>
  </form>
</template>

<script>
import MyInput from "@/components/UI/MyInput.vue";
import MyButtons from "@/components/UI/MyButtons.vue";

export default {
  components: {
    MyButtons, MyInput
  },
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>