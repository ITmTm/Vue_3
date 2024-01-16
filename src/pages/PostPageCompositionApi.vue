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
      />
    </my-dialog>

    <!--suppress JSValidateTypes -->
    <post-list
      :posts="sortedAndSearchedPosts"
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
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from '@/hooks/useSortedAndSearchedPosts';

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
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По содержимому'},
      ]
    }
  },
  setup(props) {
    const {posts, totalPages, isPostsLoading} = usePosts(10);
    const {sortedPosts, selectedSort} = useSortedPosts(posts);
    const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts);

    return {
      posts,
      totalPages,
      isPostsLoading,
      sortedPosts,
      selectedSort,
      searchQuery,
      sortedAndSearchedPosts,
    }
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