<template>
    <div class="navbar">
        <div @click="$router.push('/')">Проект на Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/')" class="btn">Главная</my-button>
            <my-button @click="$router.push('/posts')" class="btn navbar__btns__item">Посты</my-button>
            <my-button @click="$router.push('/store')" class="btn navbar__btns__item">Store</my-button>
            <my-button @click="$router.push('/composition')" class="btn navbar__btns__item">Composition</my-button>
            <my-button @click="$router.push('/about')" class="btn navbar__btns__item">О приложении</my-button>
        </div>
    </div>
</template>

<script>

    export default {}

</script>

<style scoped>
    .navbar {
        height: 70px;
        background-color: lightgrey;
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 10px;
    }
    .navbar__btns {
        margin-left: auto;
    }

    .navbar__btns__item {
        margin-left: 10px;
    }
</style>