<script setup>
import todoItem from './todoItemComponent.vue'
import { inject } from 'vue';

let todoList = inject('todoList');
let error = inject("error");

function changeItemStatus(index) {

    todoList.value[index].completed = !todoList.value[index].completed;
    console.table(todoList.value[index]);
}
function deleteItem(index) {
    todoList.value.splice(index, 1);
    console.log(todoList.value.length);
}
//  function addItem(title){
//     function body
//  }
</script>

<template>
    <span class="hint">Кликните дважды по записи чтобы поменять ее статус.</span>
    <h2 v-if="error" style="color: red;">{{ error }}</h2>
    <div class="items-container" v-else>
        <transition-group name="fade">
            <todoItem v-for="(todo, index) in todoList" :todo="todo" :index="index" :key="todo.id"
                @status="() => changeItemStatus(index)" 
                @delete="() => deleteItem(index)">
                {{ todo.title }}
            </todoItem>
        </transition-group>
    </div>
</template>

<style scoped>
.hint {
    display: block;
    margin: 15px 0;
    color: #cac6c6;
    font-size: 1rem;
}

.fade-move,
.fade-enter-active,
.fade-leave-active {
    transition: all 0.5s;

}

.fade-enter-from,
.fade-leave-to {
    transform: translate(30px, 0);
    opacity: 0;

}

.fade-leave-active {
    position: absolute;
}

.items-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    align-items: start;
    text-align: center;
}

@media screen and (max-width: 500px) {
    .items-container {
        grid-template-columns: none;
    }
}
</style>