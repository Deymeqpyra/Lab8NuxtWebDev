<template>
  <div class="container">
    <div class="flex justify-center">
      <div class="w-full">
        <div class="card">
          <div v-if="post" class="card-body">
            <p><strong>ID:</strong> {{ post.id }}</p>
            <h1>{{ post.title }}</h1>
            <p><strong>Автор:</strong> {{ post.user.name }}</p>
            <p><strong>Категорія:</strong> {{ post.category.title }}</p>
            <p><strong>Дата публікації:</strong> {{ post.published_at }}</p>
            <div v-html="post.content"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';
const post = ref(null);
const route = useRoute();
const getPostDetails = async (id) => {
  try {
    const response = await fetch(`http://127.0.0.1:8000/api/blog/posts/${id}`);
    if (response.ok) {
      post.value = await response.json();
    } else {
      throw new Error('Невдалося вивести данні');
    }
  } catch (error) {
    console.error('Помилка: ', error);
  }
};
onMounted(() => {
  if (route.params.id) {
    getPostDetails(route.params.id);
  }
});
watch(() => route.params.id, (newId) => {
  if (newId) {
    getPostDetails(newId);
  }
});
</script>

<style scoped>
</style>
