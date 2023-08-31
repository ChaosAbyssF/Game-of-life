<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
    isAlive:Boolean
})
</script>

<template>
    <div :class="['block',props.isAlive?'alive':'']">
    </div>
</template>

<style scoped>
.block {
    height: 10px;
    width: 10px;
    border: white 1px solid;
    flex-shrink: 0;
}

.block.alive{
    background-color: white;
}
</style>
