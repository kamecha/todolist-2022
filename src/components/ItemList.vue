<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";

const items = ref([]);
const newItemName = ref("");
const url = "https://temma.trap.show/naro-todo-server/";
const traqId = "kamecha";
let counter = 0;

const addItem = () => {
  if (newItemName.value === "") {
    return;
  }
  let item = {
    id: counter++,
    name: newItemName.value,
    isDone: false,
  };
  axios.post(url + traqId + "/tasks", item).then((res) => {
    items.value = res.data;
  });
  newItemName.value = "";
};
const getTask = () => {
  axios.get(url + traqId + "/tasks").then((res) => {
    console.log(res);
    items.value = res.data;
  });
};
const deleteAllTask = () => {
  axios.delete(url + traqId + "/tasks").then((res) => {
    items.value = res.data;
  });
};
const deleteTask = (id) => {
  axios.delete(url + traqId + "/tasks" + "/" + id).then((res) => {
    console.log(res);
    items.value = res.data;
  });
};
const checkTask = (id) => {
  let putItem;
  items.value.forEach((item) => {
    if (item.id == id) {
      putItem = item;
    }
  });
  putItem.isDone = !putItem.isDone;
  axios.put(url + traqId + "/tasks" + "/" + id, putItem).then((res) => {
    console.log(res);
    items.value = res.data;
  });
};
onMounted(() => {
  getTask();
});
</script>

<template>
  <div>TodoList</div>
  <button @click="getTask">Get List</button>
  <button @click="deleteAllTask">All Delete</button>
  <div>
    <label>
      名前
      <input v-model="newItemName" type="text" />
    </label>
    <button @click="addItem">add</button>
  </div>

  <div>
    <label>未完了</label>
    <div v-for="item in items" :key="item.name">
      <div v-if="item.isDone == false">
        <div class="item">
          <div class="name">
            {{ item.name }}
            <button @click="checkTask(item.id)">check</button>
            <button @click="deleteTask(item.id)">delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div>
    <label>完了</label>
    <div v-for="item in items" :key="item.name">
      <div v-if="item.isDone == true">
        <div class="item">
          <div class="name">
            {{ item.name }}
            <button @click="checkTask(item.id)">check</button>
            <button @click="deleteTask(item.id)">delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
