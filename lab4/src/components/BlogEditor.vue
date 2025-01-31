<template>
    <div class="blog-editor">
        <h1>Blog Editor</h1>
        <div class="field">
            <label for="title">Title</label>
            <input type="text" id="title" v-model="title" />
        </div>
        <div class="field">
            <label for="slug">Blog ID</label>
            <input type="text" id="slug" v-model="id" />
        </div>
        <div class="field">
            <label for="author">Author</label>
            <input type="text" id="author" v-model="author" />
        </div>
        <div class="field">
            <label for="content">Content</label>
            <textarea id="content" v-model="content"></textarea>
        </div>
        <div class="field">
            <button class="save-button" @click="save" :disabled="!isFormValid">Save</button>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const title = ref('');
const author = ref('');
const content = ref('');
const id = ref('');

const emits = defineEmits(['addNewItem']);

const isFormValid = computed(() => {
    return title.value.trim() !== '' && author.value.trim() !== '' && content.value.trim() !== '' && id.value.trim() !== '';
});

const save = () => {
    if (isFormValid.value) {
        emits('addNewItem', ({
            title: title.value,
            author: author.value,
            content: content.value,
            id: id.value
        }));

        title.value = '';
        author.value = '';
        content.value = '';
        id.value = '';
    }
};
</script>

<style scoped>
.blog-editor {
    max-width: 600px;
    margin: 0 auto;
    padding: 2rem;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    text-align: left;
}

h1 {
    text-align: center;
    margin-bottom: 1.5rem;
}

.field {
    margin-bottom: 1.5rem;
}

.field label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: bold;
    text-align: left;
}

.field input,
.field textarea {
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    text-align: left;
}

.field textarea {
    resize: vertical;
    height: 150px;
}

.save-button {
    display: block;
    width: 50%;
    padding: 0.75rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
    text-align: center;
    margin: 0 auto;
}

.save-button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
}
</style>