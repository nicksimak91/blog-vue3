<template>
    <div >
        <h1>Страница с постами</h1>
        <my-input 
            v-focus
            v-model="searchQuery"
            placeholder="Поиск..."
        />
        <div class="app__btns">
            <my-button 
                @click="showDialog" 
                class="btn" 
                style="width: 200px"
            >
            Добавить пост
            </my-button>

            <my-select 
                v-model="selectedSort"
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
        <div v-intersection="loadMorePosts" class="odserves"></div>
    </div>
</template>

<script>

    import PostList from '@/components/PostList';
    import PostForm from '@/components/PostForm';
    import axios from 'axios';

    export default{

     components: {
    PostList,
    PostForm,
},

     data(){
        return {
            posts: [],
            dialogVisible: false,
            modificatorValue: '',
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'},
            ],

            
        }
      },
      methods: {
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

          async fetchPosts(){
            try {
                this.isPostsLoading = true;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                    _page: this.page,
                    _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts = response.data;
                  
            } catch (e) {
                alert('Ошибка')
            } finally {
                this.isPostsLoading = false;
            }
          },

          async loadMorePosts(){
            try {
                this.page += 1;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                    _page: this.page,
                    _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts = [...this.posts, ...response.data];
                  
            } catch (e) {
                alert('Ошибка')
            } finally {
             
            }
          }
      } ,

      mounted: function(){
          this.fetchPosts();
        },

      computed: {
          sortedPosts(){
              return [...this.posts].sort((post1, post2) => {
                  return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]);
              } )
          },
          sortedAndSearchedPosts(){
              return this.sortedPosts.filter( post => {
                  return post.title.toLowerCase().includes(this.searchQuery.toLowerCase());
              });
          }
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