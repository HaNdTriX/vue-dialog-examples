<script setup lang="ts">
import { Button } from "@/components/ui/button";
import MyDialogContent from "@/components/MyDialogContent.vue";
import UnsiversalDialog from "@/components/UnsiversalDialog.vue";
import { ref } from "vue";

// Via this ref we can access the Dialog's imperative API.
const myDialogRef = ref<InstanceType<typeof UnsiversalDialog>>();

const handlerImperativeButtonClick = async () => {
  // Ensure the Dialog is mounted.
  if (!myDialogRef.value) throw new Error("Dialog not mounted");

  // Awesome! Here we have an imperative control flow.
  // We could even open multiple Dialogs in a row.
  const { data, isCanceled } = await myDialogRef.value.reveal({
    foo: "Hello World",
  });

  if (!isCanceled) {
    console.log("confirmed", data);
  } else {
    console.log("canceled", data);
  }
};
</script>

<template>
  <div class="mx-auto max-w-80 my-8 flex flex-col space-y-4">
    <UnsiversalDialog :content="MyDialogContent">
      <Button>Deklarative Button Trigger</Button>
    </UnsiversalDialog>

    <Button @click="handlerImperativeButtonClick">
      Imperative Button Trigger
    </Button>

    <UnsiversalDialog ref="myDialogRef" :content="MyDialogContent" />
  </div>
</template>
