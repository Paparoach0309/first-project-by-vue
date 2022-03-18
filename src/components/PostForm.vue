<template>
  <form @submit.prevent>
        <h4>Create post</h4>
        <my-input
            v-model="post.title"
            type="text"
            placeholder="Name of post"
        />
        <my-input
            v-model="post.body"
            type="text"
            placeholder="Decscription"
        />
        <my-button style="margin-top: 15px" class="btn" @click="createPost">Create</my-button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            post : {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        createPost () {
            this.post.id =  Date.now();
            this.$emit('create', this.post)
            this.post = {
                title: '',
                body: ''
            }
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: column;
    }
</style>