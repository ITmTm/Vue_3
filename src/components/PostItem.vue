<template>
  <div class="post">
    <div>
      <div>{{ post.id }}</div>
      <div><strong>Название:</strong> {{ post.title }}</div>
      <div><strong>Описание:</strong> {{ post.body }}</div>
    </div>
    <div class="post__btns">
      <my-buttons
        @click="$router.push(`/posts/${post.id}`)"
      >
        Открыть
      </my-buttons>

      <my-buttons
        @click="$emit('remove', post)"
      >
        Удалить
      </my-buttons>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style lang="scss" scoped>
.post {
  padding: 15px;
  margin-top: 15px;
  border: 2px solid teal;
  display: flex;
  align-items: center;
  justify-content: space-between;
  &__btns {
    display: flex;
  }
}
</style>