<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
      v-model="searchQuery"
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
        v-model="selectedSort"
        :options="sortOptions"
      />

    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
        @create="createPost"
      />
    </my-dialog>

    <post-list
      :posts="sortedAndSearchPosts"
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
    <!--    <div class="page__wrapper">-->
    <!--      <div-->
    <!--        v-for="pageNumber in totalPages"-->
    <!--        :key="pageNumber"-->
    <!--        class="page"-->
    <!--        :class="{-->
    <!--          'current-page': page === pageNumber-->
    <!--        }"-->
    <!--        @click="changePage(pageNumber)"-->
    <!--      >-->
    <!--        {{ pageNumber }}-->
    <!--      </div>-->
    <!--    </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyButtons from "@/components/UI/MyButtons.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
export default {
  components: {
    MyInput,
    MySelect,
    MyButtons,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По содержимому'},
      ]
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    // },

    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert(`Ошибка в запросе: ${e}`)
      } finally {
        this.isPostsLoading = false;
      }
    },

    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert(`Ошибка в запросе: ${e}`)
      }
    }
  },
  mounted() {
    this.fetchPosts();

    console.log(this.$refs.observer);

    // const options = {
    //   rootMargin: "0px",
    //   threshold: 1.0,
    // };
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts()
    //   }
    // }
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer);
  },
  computed: {
    sortedPosts() {
      return[...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
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