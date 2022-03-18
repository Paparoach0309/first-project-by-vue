<template>
    <div>
        <h1>Posts </h1>
        <my-input v-model="searchQuerry" placeholder="search..."/>
        <div class="app_btns">
            <my-button @click="showDialog">Create post</my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">            
            <post-form @create="createPost"/>
        </my-dialog>
        <post-list :posts="searchPost" @remove="removePost" v-if="!isPostLoading"/>
        <div v-else>Loading...</div>
        <div class="page_wrap">
            <div
                v-for="pageNumber in totalPage"
                :key="pageNumber"
                class="page"
                :class="{'current-page': page === pageNumber}"
                @click="changePage(pageNumber)"
            >{{pageNumber}}</div>
        </div>
    </div>
</template>

<script>
    import PostForm from '@/components/PostForm.vue';
    import PostList from '@/components/PostList.vue';
    import MyButton from '@/components/UI/MyButton.vue';
    import MySelect from '../components/UI/MySelect.vue';
    import axios from 'axios';
import MyInput from '../components/UI/MyInput.vue';
export default {
    components: {
        PostList, PostForm, MyButton, MySelect,
        MyInput
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostLoading: false,
            selectedSort: '',
            searchQuerry: '',
            page: 1,
            limit: 10,
            totalPage: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'},
                {value: 'id', name: 'По ID'},
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
            this.dialogVisible = true
        },
        changePage(pageNumber) {
            this.page = pageNumber;
            // this.fetchPosts();
        },
        async fetchPosts() {
            try {
                this.isPostLoading = true;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                });
                this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = await response.data;
            } catch (e) {
                alert('error')
            } finally {
                this.isPostLoading = false;
            }
        }
    },
    mounted() {
        this.fetchPosts()
    },
    // watch: {
    //     selectedSort(newValue) {
    //         console.log(newValue);
    //         this.posts.sort((post1, post2) => {
    //             return post1[newValue]?.localeCompare(post2[newValue])
    //         })
    //     }
    // }
    watch: {
        page() {
            this.fetchPosts()
        }
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localCompare(post2[this.selectedSort]))
        },
        searchPost() {
                return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuerry.toLowerCase()))
        }
    }
}
</script>

<style>
    .app_btns {
        margin: 15px 0;
        display: flex;
        justify-content: space-between;
    }

    .page_wrap {
        display: flex;
        margin-top: 15px;
    }

    .page {
        border: 1px solid black;
        padding: 10px;
    }

    .current-page {
        border: 2px solid teal;
    }
    
</style>