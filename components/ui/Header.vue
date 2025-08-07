<script setup lang="ts">
interface Props {
    text: string;
    mainHeader?: boolean;
    class?: string;
    size?: 'small' | 'medium' | 'large';
}

const props = withDefaults(defineProps<Props>(), {
    size: 'small',
});

const sizeClass = computed(() => {
    switch (props.size) {
        case 'small':
            return 'text-md';
        case 'medium':
            return 'text-2xl';
        case 'large':
            return 'text-4xl';
        default:
            return 'text-base';
    }
});
</script>

<template>
    <h1 v-if="mainHeader" :class="['text-5xl w-full mb-5 mt-3', props.class]">{{ text }}</h1>
    <h2 v-else :class="['font-bold', sizeClass, props.class]">{{ text }}</h2>
</template>
