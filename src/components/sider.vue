<template>
    <main>
        <div v-for="(item, index) in props.list" :key="index" class="item" :class="{
            active: index === props.now
        }" @click="emits('page', index)">
            {{ item }}
        </div>
    </main>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const emits = defineEmits(['page'])

const props = defineProps({
    list: Array,
    now: Number
})

</script>

<script>
export default {
    name: 'slider'
}
</script>

<style scoped lang="less">
main {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
}

.item {
    width: 120px;
    height: 40px;
    line-height: 40px;
    font-weight: 800;
    color: #ccc;
    text-align: center;
    cursor: pointer;

    &.active {
        color: #fff;
        border-bottom: 2px solid rgb(25, 155, 205);
    }
}
</style>