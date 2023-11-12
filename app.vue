<script lang="ts" setup>
interface Comment {
  id: number;
  name: string;
  body: string;
}

const comments = ref<Array<Comment> | null>();

async function getData() {
  try {
    const { data } = await useFetch<Array<Comment>>(
      "https://jsonplaceholder.typicode.com/comments",
    );

    comments.value = data.value;
  } catch (err) {
    console.log(err);
  }
}

const scrollEl = ref<HTMLElement | null>(null);
useInfiniteScroll(scrollEl, async () => {});

onMounted(() => getData());
</script>

<template>
  <div
    v-if="comments && comments.length > 0"
    class="h-screen w-screen flex justify-content">
    <div ref="scrollEl" class="h-80 overflow-scroll">
      <div v-for="item in comments" :key="item.id" class="flex flex-col">
        <span>{{ item.name }}</span>
        <span>{{ item.body }}</span>
      </div>
    </div>
  </div>
</template>
