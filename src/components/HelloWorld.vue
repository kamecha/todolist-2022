<script setup>
import ItemList from "./ItemList.vue";

defineProps({
  msg: { type: String, default: "" },
});
</script>

<template>
  <div>
    <h1>{{ msg }}</h1>
    <ItemList />
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
