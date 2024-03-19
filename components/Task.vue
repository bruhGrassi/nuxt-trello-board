<script setup lang="ts">
import type { Task, ID } from "@/types";
const props = defineProps<{
  task: Task;
}>();

const focused = ref(false);

const emit = defineEmits<{
  (e: "delete", payload: ID): void;
}>();

//Vueuse
onKeyStroke("Backspace", (e) => {
  if (focused.value) emit("delete", props.task.id);
});
</script>
<template>
  <div
    :title="task.createdAt.toLocaleDateString()"
    class="task bg-white shadow-sm rounded mb-2 p-2 flex"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0"
  >
    <DragHandle class="pr-2" />
    <span>{{ task.title }}</span>
  </div>
</template>
