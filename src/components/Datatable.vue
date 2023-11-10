<template>
<table role="grid">
  <thead>
    <tr
      v-if="items.length"
    >
      <th
        v-for="key in Object.keys(items[0])"
      >
        {{ key }}
            <a
                @click.prevent="sortBy(key, 'asc')"
                href="#"
            >
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" style="width: 16px;">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 19.5v-15m0 0l-6.75 6.75M12 4.5l6.75 6.75" />
                </svg>
            </a>

            <a
                @click.prevent="sortBy(key, 'desc')"
                href="#"
            >
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" style="width: 16px;">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m0 0l6.75-6.75M12 19.5l-6.75-6.75" />
                </svg>
            </a>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr
        v-for="(item, index) in items"
    >
      <template
        v-if="inRange(index)"
      >
        <td v-text="item.name" />
        <td v-text="item.age" />
        <td v-text="item.isAdmin" />
      </template>
    </tr>
  </tbody>
</table>

<nav>
  <ul>
    <li
      v-for="page in pages"
    >
      <a
          @click="setPage(page)"
          href="#"
      >{{ page }}</a>
    </li>
  </ul>
</nav>
</template>

<script setup lang="ts">

import {computed, ref, watch} from "vue";

type itemType = {name: string, age: number, isAdmin: boolean};
type directionType = 'asc'|'desc';

const props = defineProps<{
    items: Array<itemType>,
    perPage: number,
}>();

const currentPage = ref<number>(1);
const startRange = ref<number>();
const endRange = ref<number>();

const setPage = (page: number) => currentPage.value = page;

const inRange = (index: number): boolean => index >= startRange.value! && index < endRange.value!;

const pages = computed((): number => Math.ceil(props.items.length / props.perPage));

const sortBy = (
    key: string,
    direction: directionType,
) => {
  props.items.sort((a: itemType, b: itemType): number => {
    if (typeof a[key] === 'number') {
      return (direction === 'asc' ? a : b)[key] - (direction === 'asc' ? b : a)[key];
    }

    if (typeof a[key] === 'string') {
      return (direction === 'asc' ? a : b)[key].localeCompare((direction === 'asc' ? b : a)[key]);
    }

    return (direction === 'asc' ? a : b)[key] - (direction === 'asc' ? b : a)[key];
  });
}

watch(currentPage, (currentPage) => {
  startRange.value = (currentPage - 1) * props.perPage;
  endRange.value = currentPage * props.perPage;
}, { immediate: true });
</script>