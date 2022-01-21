<template>
    <div >
        <h1>Страница с постами</h1>
        <my-input 
            v-focus
            v-model="searchQuery"
            placeholder="Поиск..."
        />

        <div class="app__btns">
            <my-select 
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible"></my-dialog>

        <post-list  
            v-bind:posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading"    
        />

        <div v-else>Идет загрузка...</div>

    </div>
</template>

<script>

    import PostList from '@/components/PostList';
    import PostForm from '@/components/PostForm';
    import { usePosts } from '@/hooks/usePosts';
    import useSortedPosts from '@/hooks/useSortedPosts';
    import useSortedAndSearchedPosts from '@/hooks/useSortedAndSearchedPosts';

    export default{
    components: {
    PostList,
    PostForm,
},

     data(){
        return {
            dialogVisible: false,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'},
            ],

            
        }
      },

      setup(props){
          const { posts, totalPages, isPostsLoading } = usePosts(10);
          const { sortedPosts, selectedSort } = useSortedPosts(posts);
          const { searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)
            
            return {
            posts,
            totalPages,
            isPostsLoading,
            selectedSort,
            sortedPosts,
            searchQuery,
            sortedAndSearchedPosts
        }
      }



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