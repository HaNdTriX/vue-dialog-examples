<script setup lang="ts">
import { Button } from "@/components/ui/button";
import MyDialog from "@/components/MyDialog.vue";
import { ref } from "vue";

// Via this ref we can access the Dialog's imperative API.
const myDialogRef = ref<InstanceType<typeof MyDialog>>();

const handlerImperativeButtonClick = async () => {
  // Ensure the Dialog is mounted.
  if (!myDialogRef.value) throw new Error("Dialog not mounted");

  // Awesome! Here we have an imperative control flow.
  // We could even open multiple Dialogs in a row.
  const { data, isCanceled } = await myDialogRef.value.reveal({
    data: "Hello World",
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
    <Button @click="handlerImperativeButtonClick">
      Imperative Button Trigger
    </Button>

    <MyDialog ref="myDialogRef">
      <Button>Deklarative Button Trigger</Button>
    </MyDialog>
  </div>
</template>
