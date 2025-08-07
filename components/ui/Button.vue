<script setup lang="ts">
import { mdiLoading } from '@mdi/js';
import Icon from './Icon.vue';

interface Props {
    label: string;
    size?: 'small' | 'medium' | 'large';
    variant?: 'primary' | 'secondary' | 'custom';
    buttonClass?: string;
    type?: 'button' | 'submit' | 'reset';
    loading?: boolean;
    iconPath?: string;
}

const props = withDefaults(defineProps<Props>(), {
    size: 'medium',
    variant: 'primary',
    type: 'button',
});

const emits = defineEmits(['submit']);

const buttonSizes = {
    small: 'text-xs px-2 py-1',
    medium: 'text-sm px-4 py-2',
    large: 'text-lg px-6 py-3',
};

const buttonVariants = {
    primary: 'bg-red-950 hover:bg-red-900 text-white',
    secondary: 'bg-primary-50 hover:bg-primary-50 text-primary-950',
    custom: 'bg-custom-950 hover:bg-custom-900 text-white',
};
</script>
<template>
    <button
        :class="[
            'rounded text-primary-50 m-1 text-center flex items-center cursor-pointer h-fit relative',
            buttonSizes[size],
            buttonVariants[variant],
            buttonClass,
        ]"
        @click="emits('submit')"
        :type="type"
        >
        <Icon v-if="loading" :path="mdiLoading" class="absolute top-1/2 left-1/2 -translate-1/2 animate-spin fill-primary-50 h-6 w-6" />
        <Icon v-else-if="iconPath" :path="iconPath" class="fill-primary-50 h-6 w-6 -translate-x-2" />
        <label :class="['cursor-pointer', {'invisible': loading}]">
            {{ props.label }}
        </label>
    </button>
</template>
