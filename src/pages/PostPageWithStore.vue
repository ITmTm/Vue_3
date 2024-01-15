<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
      :model-value="searchQuery"
      @update:model-value="setSearchQuery"
      placeholder="Поиск..."
      v-focus
    />
    <div class="app_btns">
      <my-buttons
        @click="showDialog"
      >
        Создать пост
      </my-buttons>
      <my-select
        :model-value="selectedSort"
        @update:model-value="setSelectedSort"
        :options="sortOptions"
      />

    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
        @create="createPost"
      />
    </my-dialog>

    <post-list
      :posts="sortedAndSearchedPosts"
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
    <div v-intersection="loadMorePosts" class="observer"></div>


        <div class="page__wrapper">
          <div
            v-for="pageNumber in totalPages"
            :key="pageNumber"
            class="page"
            :class="{
              'current-page': page === pageNumber
            }"
            @click="changePage(pageNumber)"
          >
            {{ pageNumber }}
          </div>
        </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyButtons from "@/components/UI/MyButtons.vue";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
import {mapState, mapGetters, mapActions, mapMutations} from "vuex";

export default {
  components: {
    MyInput,
    MySelect,
    MyButtons,
    PostList, PostForm
  },
  data() {
    return {
      dialogVisible: false,
    }
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
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
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPostsLoading: state => state.post.isPostsLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions
    }),
    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
    })
  },
  watch: {
    // page() {
    //   this.fetchPosts();
    // }
  }
}
</script>

<style lang="scss">
.app_btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page {
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
  &__wrapper {
    display: flex;
    column-gap: 2px;
    margin-top: 15px;
  }
}

.current-page {
  border: 2px solid #60dc14;
}

.observer {
  height: 5px;
  background: green;
}
</style>