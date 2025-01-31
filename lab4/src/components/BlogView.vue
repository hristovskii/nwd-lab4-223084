<template>
    <div class="blog-view">
        <div class="header">
            <button class="add-blog-button" @click="showEditor = !showEditor" v-if="!showEditor">Add new blog</button>
        </div>
        <div class="editor" v-if="showEditor">
            <BlogEditor @addNewItem="addItem"/>
        </div>
        <div class="blogs-container">
            <Blogs :blogs="blogs" :is-loading="isLoading" :error="error" @deleteBlog="deleteItem"/>
        </div>
    </div>
</template>

<script setup>
import BlogEditor from './BlogEditor.vue';
import Blogs from './Blogs.vue';
import { useBlogs } from '../composables/useBlogs';
import { ref } from 'vue';

const { blogs, isLoading, error, addItem, deleteItem } = useBlogs();
const showEditor = ref(false);
</script>

<style scoped>
.blog-view {
    padding: 20px;
    max-width: 800px;
    margin: 0 auto;
}

.header {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 20px;
}

.add-blog-button {
    background-color: #42b983;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
}

.add-blog-button:hover {
    background-color: #369f6e;
}

.editor {
    margin-bottom: 20px;
}

.blogs-container {
    margin-top: 20px;
}
</style>