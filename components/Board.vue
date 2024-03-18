<script setup lang="ts">
import { nanoid } from "nanoid";
import type { Column } from "@/types/index";
const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: "Backlog",
    tasks: [
      {
        id: nanoid(),
        title: "Define priority",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Project architecture",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "Selected for dev",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "Selected for dev",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "QA",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "Production",
    tasks: [],
  },
]);
</script>
<template>
  <div class="flex gap-4 overflow-x-auto items-start">
    <div
      v-for="column in columns"
      :key="column.id"
      class="column bg-gray-100 p-5 rounded min-w-[250px]"
    >
      <header>{{ column.title }}</header>
      <p v-for="task in column.tasks">
        {{ task.title }}
      </p>
    </div>
  </div>
</template>
