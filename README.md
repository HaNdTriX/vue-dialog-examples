# Vue Dialog Examples

Working with dialogs inside declarative UI frameworks like React.js or Vue.js can be quite complex.

This is because dialogs can be used in two different ways:

## 1. Declarative Way

```jsx
<DialogRoot>
  <DialogTrigger>Open Dialog</DialogTrigger>
  <DialogContent>...</DialogContent>
</DialogRoot>
```

## 2. Imperative Way

```jsx
function handleSomeKindOfEffect() {
  const resturnValues = await revealDialog()
}
```

From my point of view both ways a totally valid to use. There is nothing bad about the imperative way. Especially when working with dialog cascades the imperative way starts to shine:

```jsx
async function handleSomeKindOfEffect() {
  const { confirmed } = await revealDialog1();
  if (confirmed) {
    await revealDialog2();
  } else {
    await revealDialog3();
  }
}
```

Thats why this repo tries to show ways of implementing imperative dialogs in [Vue.js](https://vuejs.org/).

## Solution

This example tries to bring both ways together.
