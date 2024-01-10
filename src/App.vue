<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-buttons
      @click="showDialog"
      style="margin: 15px 0;"
    >
      Создать пост
    </my-buttons>
    <my-dialog v-model:show="dialogVisible">
      <post-form
        @create="createPost"
      />
    </my-dialog>

    <post-list
      :posts="posts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>
      <img
        class="spinner"
        src="https://i.pinimg.com/originals/91/91/85/919185a188c5cc25655fadfbc9a4a2b4.gif"
        alt="loading"
        style="display: block; margin: 0 auto; width: 350px"
      >
    </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyButtons from "@/components/UI/MyButtons.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import axios from "axios";
export default {
  components: {
    MyButtons,
    MyDialog,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      this.isPostsLoading = true;
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
        this.posts = response.data;
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false;
      }
    }
  },
  mounted() {
    this.fetchPosts();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

</style>