<template>
  <section class="h-80 flex flex-col">
    <h2 class="font-semibold">Fetching Data</h2>
    <div v-if="isPending" class="text-gray-700">Loading...</div>
    <div v-if="isFetching" class="text-gray-700">Fetching...</div>
    <div v-if="isError" class="text-red-700">{{ error!.message }}</div>
    <ul
      v-if="todos"
      class="py-3 rounded overflow-auto shadow-sm border space-y-2 p-2"
    >
      <li
        v-for="todo in todos"
        :key="todo.id"
        class="bg-gray-50 shadow-sm rounded-sm p-1"
      >
        {{ todo.title }}
      </li>
    </ul>
  </section>

  <section>
    <h2 class="font-semibold">Reveal Data</h2>
    <pre class="rounded border p-2">{{ JSON.stringify(data, null, 2) }}</pre>
  </section>

  <section class="flex flex-col space-y-2">
    <h2 class="font-semibold">Trigger Buttons:</h2>
    <!-- 
      Close modal directly by using v-model
    -->
    <Button @click="() => (open = false)">close modal via v-model</Button>

    <!-- 
      Close modal directly by using DialogClose
    -->
    <DialogClose asChild>
      <Button>close modal via DialogClose</Button>
    </DialogClose>

    <!-- Imperative way (useConfirmDialog) -->
    <Button
      @click="() => $emit('confirm', { data: 'SUCCESS DATA' })"
      variant="secondary"
    >
      confirm
    </Button>
    <Button
      @click="() => $emit('cancel', { data: 'CANCEL DATA' })"
      variant="destructive"
    >
      cancel
    </Button>
  </section>
</template>
<script lang="ts" setup>
import { defineEmits, defineModel } from "vue";
import { Button } from "@/components/ui/button";
import { DialogClose } from "@/components/ui/dialog";
import { useQuery } from "@tanstack/vue-query";

const open = defineModel("open", {
  type: Boolean,
  default: false,
});

type Props = {
  data: {
    foo: string;
  };
};

defineProps<Props>();

defineEmits<{
  // Declarative way
  close: [{ data: string }];

  // Imperative way
  cancel: [{ data: string }];
  confirm: [{ data: string }];
}>();

type Todo = {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
};

const {
  isPending,
  isFetching,
  isError,
  data: todos,
  error,
} = useQuery<Todo[]>({
  queryKey: ["todos"],
  queryFn: async () => {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    return response.json();
  },
});
</script>
