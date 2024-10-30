<script setup>
import { inject, ref, watch } from 'vue';
import TodoItem from './todoItemComponent.vue';
import SortingComponent from './sortingComponent.vue';



let todoList = inject('todoList');
if (!todoList.value) { todoList = ref([]) }
let todoListShown = ref([...todoList.value]);
let error = inject("error");
const activeFilter = ref(null); 

function updateDisplayedList() {
    todoListShown.value = [...todoList.value];
}

watch(todoList, updateDisplayedList, { deep: true });

function sortCompletedFirst() { todoListShown.value.sort((a,b) => b.completed - a.completed) };
const sortNotCompletedFirst = () => { todoListShown.value.sort((a,b) => a.completed - b.completed) };
const sortLengthSort = () => { todoListShown.value.sort((a,b) => a.title.length - b.title.length) };
function filterOnlyCompleted() {
    if (activeFilter.value === 'completed') {
        updateDisplayedList();
    } else {
        activeFilter.value = 'completed';
        todoListShown.value = todoList.value.filter(el => el.completed);
    }
}
function filterOnlyNotCompleted() {
    if (activeFilter.value === 'notCompleted') {
        updateDisplayedList();
    } else {
        activeFilter.value = 'notCompleted';
        todoListShown.value = todoList.value.filter(el => !el.completed);
    }
}
function resetList() {
    updateDisplayedList() 
    activeFilter.value = null;
}



function changeItemStatus(index) {
    todoList.value[index].completed = !todoList.value[index].completed;
    console.log(todoList.value[index])
}
function deleteItem(index) {
    todoList.value.splice(index, 1);
    console.log(todoList.value.length);
}

</script>

<template>
    <SortingComponent @only-completed="filterOnlyCompleted" @completed-first="sortCompletedFirst"
        @not-completed-first="sortNotCompletedFirst" @length-sort="sortLengthSort"
        @only-not-completed="filterOnlyNotCompleted" @reset="resetList" />
    <span class="hint">Кликните дважды по записи чтобы поменять ее статус.</span>
    <h2 v-if="error" style="color: red;">{{ error }}</h2>
    <div class="items-container" v-else>
        <transition-group name="fade">
            <TodoItem v-for="(todo, index) in todoListShown" :todo="todo" :index="index" :key="todo.id"
                @status="changeItemStatus(index)" @delete="deleteItem(index)">
                {{ todo.title }}
            </TodoItem>
        </transition-group>
    </div>
</template>

<style scoped>
.hint {
    font-family: 'Roboto';
    font-weight: 500;
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