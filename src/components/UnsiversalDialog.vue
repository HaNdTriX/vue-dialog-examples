<script
  setup
  lang="ts"
  generic="T extends Component<{
    data: unknown;
    open: boolean;
    onConfirm: (data: unknown) => void;
    onCancel: (data: unknown) => void;
  }>"
>
import { Dialog, DialogContent, DialogTrigger } from "./ui/dialog";
import { useConfirmDialog } from "@vueuse/core";
import type { ComponentProps, ComponentEmit } from "vue-component-type-helpers";
import { type Component } from "vue";

/**
 * Reactive Dialog State that controls the Dialog state.
 **/
const open = defineModel<boolean>("open", {
  default: false,
});

/**
 * Reactive Dialog Data that is passed to the DialogContent.
 **/
const data = defineModel<RevealData>("data", {
  default: {},
});

/**
 * Define optional TriggerButton slot
 */
const slots = defineSlots<{
  default?: (props: unknown) => unknown;
}>();

const props = defineProps<{
  content: T;
}>();

type ComponentEvent<ComponentType, EventName> =
  ComponentEmit<ComponentType> extends (
    event: EventName,
    args_0: infer Args0
  ) => void
    ? Args0
    : never;

/**
 * Imperative Dialog Trigger
 * Use this pattern if you want to to open or close the Dialog
 * from within a method (e.g.: an event handler).
 **/
type RevealData = ComponentProps<T>["data"];
type ConfirmData = ComponentEvent<T, "confirm">;
type CancelData = ComponentEvent<T, "cancel">;

const imperativeHandle = useConfirmDialog<RevealData, ConfirmData, CancelData>(
  open
);

// Here we inject data into the component (await reveal({  })).
imperativeHandle.onReveal((revealData) => {
  data.value = revealData;
});

// Here we expose the imperative API to the parent component.
// Later this api can be accesed via the ref attribute.
defineExpose(imperativeHandle);

// TODO: Forward other props to DialogContent
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
      <component
        :is="props.content"
        :data="data"
        v-model:open="open"
        @cancel="imperativeHandle.cancel"
        @confirm="imperativeHandle.confirm"
      />
    </DialogContent>
  </Dialog>
</template>
