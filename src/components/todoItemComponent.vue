<script setup>

import deleteButton from './deleteButtonComponent.vue';
const props = defineProps(["todo", "index"]);
const emit = defineEmits(["status", "delete"]);
const emitItemIndex = () => emit("status", props.index);
const emitDeleteItem = () => emit("delete", props.index);
</script>

<template>
  <div class="wrapper"> 
    <div class="item-container" :class="{completed: props.todo.completed}" @dblclick="emitItemIndex" @contextmenu.prevent="emitItemIndex">
      <deleteButton class="hidden-button" @click="emitDeleteItem"/>
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  position: relative;
}

.item-container {
  overflow-y: auto;
  background-color: white;
  min-width: 220px;
  max-width: 390px;
  margin: 5px auto 15px 5px;
  border-radius: 10px;
  padding: 10px;
  min-height: 60px;
  max-height: 200px;
  text-wrap: wrap;
  color: black;
  user-select: none;
  word-break: break-all;
}

.item-container::-webkit-scrollbar {
  display: none;
}

.completed {
  border-color: rgb(37, 212, 37);
  box-shadow: 0 0 20px rgb(37, 212, 37);
}

.hidden-button {
  position: absolute;
  top: 15%;
  left: 75%;
  display: none;
  z-index: 20;

}

.completed:hover .hidden-button, .completed:target  .hidden-button {
  display: block;
}

@media screen and (max-width: 500px) {
  .item-container {
    width: 100%;

  }
}
</style>