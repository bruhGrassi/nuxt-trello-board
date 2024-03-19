<script setup lang="ts">
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column } from "@/types/index";
import Task from "@/components/Task.vue";

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
  <div>
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start pb-6"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-100 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">{{ column.title }}</header>
          <Task v-for="task in column.tasks" :key="task.id" :task="task" />
          <footer class="mt-3">
            <button class="text-gray-400">+ Add a new card</button>
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>
