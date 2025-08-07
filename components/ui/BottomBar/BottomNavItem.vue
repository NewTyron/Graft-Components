<script setup lang="ts">
import Icon from '../Icon.vue';

interface Props {
    label: string;
    iconPath: string;
    to: string | undefined;
    active?: boolean;
}

const props = defineProps<Props>();
</script>

<template>
    <NuxtLink :to="to" class="flex flex-col items-center justify-center w-20 cursor-pointer">
        <Icon :path="props.iconPath" :class="['w-6 h-6 cursor-pointer', active ? 'fill-white' : 'fill-primary-400']" />
        <label :class="['text-xs w-full text-center leading-4 line-clamp-2 cursor-pointer', active ? 'text-white' : 'text-primary-400']">{{ props.label }}</label>
    </NuxtLink>
</template>
