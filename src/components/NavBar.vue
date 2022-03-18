<template>
  <div class="navbar">
      <div @click="$router.push('/')">Vue posts</div>
      <div class="nav-btns">
          <my-button @click="$router.push('/posts')">Posts</my-button>
          <my-button @click="$router.push('/about')">About</my-button>
      </div>
  </div>
</template>

<script>
// import MyButton from './UI/MyButton.vue'
// export default {
//     components: {MyButton}
// }
</script>

<style scoped>
.navbar {
    height: 50px;
    background-color: rgb(168, 170, 170);
    display: flex;
    align-items: center;
    padding: 0 15px;
}
.nav-btns {    
    margin-left: auto;
}
</style>