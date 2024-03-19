<script setup lang="ts">
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column, Task } from "@/types/index";

const alt = useKeyModifier("Alt");

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
      :animation="200"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start pb-6"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-100 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4 flex items-center gap-2">
            <DragHandle />
            {{ column.title }}
          </header>
          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
            :animation="200"
            handle=".drag-handle"
            item-key="id"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <Task
                  :task="task"
                  @delete="
                    column.tasks = column.tasks.filter(
                      (task) => task.id !== $event
                    )
                  "
                />
              </div>
            </template>
          </draggable>
          <footer class="mt-3">
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>

<style>
/*applied when start moving the item */
.sortable-drag .task {
  transform: rotate(5deg);
}

/*element underneath the item, moves around behind it */
.sortable-ghost .task {
  position: relative;
}

.sortable-ghost .task::after {
  content: "";
  @apply absolute top-0 bottom-0 left-0 top-0 rounded bg-purple-300;
}

.task:focus,
.task:focus-visible {
  @apply outline-gray-400 !important;
  outline: gray auto 1px;
}
</style>
