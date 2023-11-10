<template>
  <main class="container" style="margin-top: 10%">
    <datatable 
      :items="items"
      :perPage=10
    />
  </main>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Datatable from './components/Datatable.vue';
import axios from 'axios';

const items = ref([]);

onMounted(async () => {
  await axios.get('../public/users.json')
      .then((res) => items.value = res.data);
});
</script>