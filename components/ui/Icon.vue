<script setup lang="ts">
interface IconProps {
    class?: any;
    path: string;
    viewBox?: string;
    pathFill?: string;
    pathClass?: string;
    pathStrokeWidth?: string;
}

const props = withDefaults(defineProps<IconProps>(), {
    class: '',
    viewBox: '0 0 24 24',
    pathStrokeLinecap: 'square',
});
</script>

<template>
    <svg :class="props.class || 'w-5 h-5 fill-[currentColor]'" :viewBox="props.viewBox || '0 0 24 24'">
        <path :class="pathClass" :d="props.path" :fill="pathFill" :stroke-width="pathStrokeWidth" />
    </svg>
</template>
