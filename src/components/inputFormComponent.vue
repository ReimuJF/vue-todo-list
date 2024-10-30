<script setup>
import { ref, computed, inject } from 'vue';
const itemText = ref('');
const entryNotEmpty = computed(() => itemText.value.trim().length > 0);
const todoList = inject('todoList');
const itemAdd = () => {
    if (entryNotEmpty.value) {
        todoList.value.push({
            "userId": 11,
            "id": todoList.value.length + 1,
            "title": itemText.value,
            completed: false
        });
    }
    itemText.value = '';
 }
</script>

<template>
    <div class="input-container">
        <label for="entry" style="font-size: 1.2rem;">Добавить запись: </label>
        <input type="text" id="entry" v-model="itemText" @keyup.enter="itemAdd" />
        <button class="button orange-background" :disabled="!entryNotEmpty"
            @click="itemAdd"><span>Добавить</span></button>
    </div>
</template>

<style scoped>
#entry {
    margin-left: 10px;
    font-size: 1.8rem;
    width: 240px;
    padding: 5px 8px;
    font-weight: 500;
}

#entry:focus-visible {
    outline: none;
}

.input-container {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    width: 40%;
    align-items: stretch;
}

.button {
    margin-left: 20px;
    font-family: 'Roboto-medium';
    font-size: 1.2rem;
    padding: 5px 10px 5px 10px;
    color: black;
    border-radius: 5px;
    border: 2px black solid;
    display: inline-block;
    cursor: pointer;
}

.button:disabled {
    background-color: gray;
    cursor: auto;
}

@media screen and (max-width: 500px) {
    .input-container {
        width: 100%;
    }

    #entry {
        width: 30vw;
    }
}
</style>