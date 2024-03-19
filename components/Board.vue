<script setup lang="ts">
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column, Task } from "@/types/index";

const alt = useKeyModifier("Alt");

const columns = useLocalStorage<Column[]>(
  "taskBoard",
  [
    {
      id: nanoid(),
      title: "Backlog",
      tasks: [
        {
          id: nanoid(),
          title: "Create authentication",
          createdAt: new Date(),
        },
      ],
    },
    {
      id: nanoid(),
      title: "Developing",
      tasks: [
        {
          id: nanoid(),
          title: "Create registration form",
          createdAt: new Date(),
        },
      ],
    },
    {
      id: nanoid(),
      title: "Testing",
      tasks: [
        {
          id: nanoid(),
          title: "Update icons",
          createdAt: new Date(),
        },
      ],
    },
    {
      id: nanoid(),
      title: "Complete",
      tasks: [
        {
          id: nanoid(),
          title: "Create main page interface",
          createdAt: new Date(),
        },
      ],
    },
  ],
  {
    serializer: {
      read: (value) => {
        return JSON.parse(value).map((column: Column) => {
          column.tasks = column.tasks.map((task: Task) => {
            task.createdAt = new Date(task.createdAt);
            return task;
          });
          return column;
        });
      },
      write: (value) => JSON.stringify(value),
    },
  }
);

//backend integration
watch(columns, () => {}, {
  deep: true,
});

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    tasks: [],
  };
  columns.value.push(column);
  nextTick(() => {
    (
      document.querySelector(
        ".column:last-of-type .title-input"
      ) as HTMLInputElement
    ).focus();
  });
}
</script>
<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      :animation="200"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 items-start pb-6"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-100 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4 flex items-center gap-2">
            <DragHandle />
            <input
              type="text"
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              v-model="column.title"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="
                column.title === ''
                  ? (columns = columns.filter((col) => col.id !== column.id))
                  : null
              "
            />
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
    <button
      @click="createColumn"
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
    >
      + Add Another Column
    </button>
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
