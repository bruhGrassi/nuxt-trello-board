<script setup lang="ts">
useSeoMeta({
  title: "Tasks Board",
  ogTitle: "Tasks Board",
  description: "Organize tasks and see the progress of your projects.",
  ogDescription: "Organize tasks and see the progress of your projects.",
});

useHead({
  htmlAttrs: {
    lang: "en",
  },
});
</script>

<template>
  <div
    class="p-10 bg-teal-400 bg-gradient-to-b from-teal-400 to-teal-700 h-[100vh] overflow-auto"
  >
    <h1 class="text-4xl text-white mb-10 font-bold flex items-center gap-4">
      <img src="/public/checklist.png" alt="checklist" class="w-20" />
      Tasks Board
    </h1>
    <Board />
  </div>
</template>
