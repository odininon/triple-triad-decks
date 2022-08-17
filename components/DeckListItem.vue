<template>
  <div class="container">
    <div>{{ deck.id }} - {{ deck.name }}</div>
    <div class="button-group">
      <button @click="$emit('delete', deck.id)">Delete</button>
      <button @click="linkTo">View</button>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(["deck"]);

function linkTo() {
  return navigateTo({
    path: `/decks/${props.deck.id}`,
  });
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  padding: 5px 0;
  margin: 5px 0;
  min-height: 50px;
}
.button-group {
}
</style>