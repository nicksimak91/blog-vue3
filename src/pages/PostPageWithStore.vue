<template>
    <div>
        <h1>Страница с постами</h1>
        <my-input 
            v-focus
            :model-value="searchQuery"
            @update:model-value="setSearchQuery"
            placeholder="Поиск..."
        />
        <div class="app__btns">
            <my-button @click="showDialog" class="btn" style="width: 200px">Добавить пост</my-button>
            <my-select 
                :model-value="selectedSort"
                @update:model-value="setSelectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">
            <post-form @create="addPost" />
        </my-dialog>
        <post-list  
            @remove="removePost" 
            v-bind:posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading"
            
        />
        <div v-else>Идет загрузка...</div>
        <div class="page__wrapper">
            <div 
                v-for="pageNumber in totalPages" 
                :key="pageNumber"
                class="page"
                :class="{
                    'current-page': page === pageNumber,
                }"
                @click="setPage(pageNumber)"
                >{{pageNumber}}
                
            </div>
        </div>
        <div v-intersection="loadMorePosts" class="odserves"></div> 
    </div>
</template>

<script>

    import PostList from '@/components/PostList';
    import PostForm from '@/components/PostForm';
    import {mapState, mapGetters, mapActions, mapMutations} from 'vuex';

    export default{

    components: {
    PostList,
    PostForm,
},

     data(){
        return {
            dialogVisible: false,
        }
      },
      methods: {
          ...mapMutations({
              setPage: 'post/setPage',
              setSearchQuery: 'post/setSearchQuery',
              setSelectedSort: 'post/setSelectedSort',
              setPage: 'post/setPage',
          }),
          ...mapActions({
              loadMorePosts: 'post/loadMorePosts',
              fetchPosts: 'post/fetchPosts',
          }),

          addPost(post){
              this.posts.push(post);
              this.dialogVisible = false;
          },
          removePost(post){
              this.posts = this.posts.filter( p => p.id !== post.id );
          },
          showDialog(){
              this.dialogVisible = true;
          },

          changePage(pageNumber){
              this.page =  pageNumber;  
          }
      } ,

      mounted: function(){
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
            sortOptions: state => state.post.sortOptions,
         }),

         ...mapGetters({
             sortedPosts: 'post/sortedPosts',
             sortedAndSearchedPosts: 'post/sortedAndSearchedPosts',
         }),
      },

    }
</script>

<style>
    .app__btns {
        display: flex;
        justify-content: space-between;
        margin: 10px 0;
    }

    .page__wrapper {
        display: flex;
        margin-top: 15px;
    }

    .page {
        border: 1px solid teal;
        padding: 10px;
        margin-left: 5px;
    }

    .current-page {
        border: 2px solid rgb(7, 105, 105);
    }

    .odserves {
        height: 30px;
        background: rgb(175, 174, 174);
    }
</style>