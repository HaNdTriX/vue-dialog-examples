<script setup lang="ts">
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTrigger,
  DialogTitle,
} from "./ui/dialog";
import MyDialogContent from "./MyDialogContent.vue";
import { defineModel, useSlots } from "vue";
import { useConfirmDialog } from "@vueuse/core";

/**
 * Reactive Dialog State that controls the Dialog state.
 **/
const open = defineModel<boolean>("open", {
  default: false,
});

/**
 * Imperative Dialog Trigger
 * Use this pattern if you want to to open or close the Dialog
 * from within a method (e.g.: an event handler).
 **/
type RevealData = {};
type ConfirmData = { data: string };
type CancelData = { data: string };
const imperativeHandle = useConfirmDialog<RevealData, ConfirmData, CancelData>(
  open
);

// Here we expose the imperative API to the parent component.
// Later this api can be accesed via the ref attribute.
defineExpose(imperativeHandle);

const slots = useSlots();
</script>

<template>
  <Dialog v-model:open="open">
    <!-- 
      The DialogTrigger is the best way to open the Dialog.
      This is because
      - it is accessible
      - it is declarative
      - it is easy to use
    -->

    <DialogTrigger v-if="slots.default" asChild>
      <slot />
    </DialogTrigger>

    <DialogContent>
      <!-- 
        The DialogHeader is Required.
        - You can use radix.VisuallyHidden to hide
        - You can move it down the jsx e.g. into MyDialogContent
          if it depends on data that has been fetched in MyDialogContent
      -->
      <DialogHeader>
        <DialogTitle>My Dialog</DialogTitle>
      </DialogHeader>

      <!-- 
        If MyDialogContent has any effects (e.g. fetching data)
        then it must not be inline (be in it's own file)
        otherwise it will be mounted immediately 
      -->
      <MyDialogContent
        v-model:open="open"
        @cancel="imperativeHandle.cancel"
        @confirm="imperativeHandle.confirm"
      />
    </DialogContent>
  </Dialog>
</template>
