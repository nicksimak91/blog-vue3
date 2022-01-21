<template>
    <div>
        <h3>Создание поста</h3>
            <form @submit.prevent>
                <my-input v-focus v-model="post.title" type="text" placeholder="Заголовок" />
                <my-input v-model="post.body" type="text" placeholder="Описание" />
                <my-button
                    @click="createPost"
                    class="btn"
                    style="align-self: flex-end; margin-top: 10px">
                        Опубликовать
                    </my-button>
            </form>
    </div>
</template>

<script>

    export default{
    
        data(){
            return {
                post: {
                    title: '',
                    body: '',
                }
            }
        },

        methods: {
            createPost(){
              this.post.id = Date.now();
              this.$emit('create', this.post);
              this.post = {
                  title: '',
                  body: '',
              }
            }
        },
    }
</script>

<style scoped>
    .input {
        width: 100%;
        border: 1px solid teal;
        padding: 10px;
        margin-top: 10px;
    }

    form {
        display: flex;
        flex-direction: column;
    }
</style>