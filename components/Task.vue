<script setup lang="ts">
import type { Task } from "@/types";
defineProps<{
  task: Task;
}>();
</script>
<template>
  <div
    :title="task.createdAt.toLocaleDateString()"
    class="task bg-white shadow-sm rounded mb-2 p-2"
  >
    <span>{{ task.title }}</span>
  </div>
</template>
