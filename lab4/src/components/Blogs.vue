<template>
  <div class="blogs-container">
    <h1>Blogs</h1>
    <div v-if="isLoading" class="loading">
      <p>Loading...</p>
    </div>
    <div v-else-if="error" class="error">
      <p>{{ error }}</p>
    </div>
    <section v-else>
      <div class="search">
        <label for="search">Search:</label>
        <input type="text" id="search" v-model="searchTerm" placeholder="Search blogs..." />
      </div>
      <fieldset>
        <div>Filter by: </div>
        <input type="checkbox" id="title" value="title" v-model="filters" />
        <label for="title">By Title</label>
        <input
          type="checkbox"
          id="content"
          value="description"
          v-model="filters"
        />
        <label for="content">By Content</label>
      </fieldset>
      <ul class="articles-list">
        <li v-for="(blog, i) in filteredBlogs" :key="i">
          <article class="article-item">
            <div class="article-item-info">
              <img
                v-if="blog.heroImage"
                class="thumbnail"
                :src="`${blog.heroImage.fields.file.url}?fit=scale&w=350&h=196`"
              />
              <div class="article-text">
                <div class="date">
                  {{ new Date(blog.publishDate).toDateString() }}
                </div>
                <h4>{{ blog.title }}</h4>
                <p>{{ blog.description }}</p>
              </div>
            </div>
            <button class="delete-button" @click="emits('deleteBlog', blog.id)">Delete</button>
          </article>
        </li>
      </ul>
    </section>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from "vue";
import { useSearch } from "../composables/useSearch";

const props = defineProps({
  blogs: {
    type: Array,
    required: true,
  },
  isLoading: {
    type: Boolean,
    required: true,
  },
  error: {
    type: String,
    required: true,
  },
});

const emits = defineEmits(["deleteBlog"]);
const blogs = ref(props.blogs);

watch(
  () => props.blogs,
  (newBlogs) => {
    blogs.value = newBlogs;
  }
);

const {
  searchTerm,
  filters,
  filteredItems: filteredBlogs,
} = useSearch(blogs);
</script>

<style scoped>
.blogs-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.loading, .error {
  text-align: center;
  margin-top: 20px;
}

.search {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
}

.search label {
  margin-right: 10px;
  font-weight: bold;
}

.search input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.search input[type="text"]::placeholder {
  color: #aaa;
}

.articles-list {
  list-style: none;
  padding: 0;
}

.article-item {
  display: flex;
  flex-direction: column;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-bottom: 20px;
  background-color: #f9f9f9;
}

.article-item-info {
  display: flex;
  align-items: center;
}

.article-text {
  padding-left: 20px;
}

.thumbnail {
  width: 150px;
  height: auto;
  border-radius: 8px;
}

.date {
  font-size: 12px;
  font-weight: bold;
  text-transform: uppercase;
  color: #888;
}

.delete-button {
  align-self: flex-end;
  padding: 10px 20px;
  background-color: #ff4d4d;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #ff1a1a;
}

fieldset {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  border: none;
}
</style>