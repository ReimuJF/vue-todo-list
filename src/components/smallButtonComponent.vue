<script setup>
const props = defineProps(["title"]);
</script>

<template>

    <span :title="props.title" class="button">
        <slot></slot>
    </span>

</template>
<style scoped>
.button {
    border-radius: 100%;
    cursor: pointer;

}

.button:hover, button:active {
    filter: drop-shadow(3px 3px 2px #ffbc02);
    transform: scale(1.2);
}
</style>
