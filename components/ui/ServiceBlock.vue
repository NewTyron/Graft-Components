<script setup lang="ts">
import Icon from './Icon.vue';

interface Props {
    label?: string;
    iconPath?: string;
    wrapperClass?: string;
    loading?: boolean;
}

defineProps<Props>();
const emits = defineEmits(['submit']);
</script>

<template>
    <div class="flex flex-col items-center h-full mb-auto justify-center w-20 cursor-pointer" @click="emits('submit')">
        <div :class="['rounded w-full h-20 flex items-center justify-center mb-1', loading ? 'animate-pulse bg-primary-500' : 'bg-primary-950']">
            <img v-if="!loading" :src="iconPath || ''" class="w-12 h-12 fill-primary-50" />
        </div>
        <label v-if="!loading" class="text-primary-800 text-xs w-full text-center h-8 line-clamp-2">{{ label }}</label>
        <div v-else class="bg-primary-500 h-4 mb-4 rounded animate-pulse w-[90%]" />
    </div>
</template>
