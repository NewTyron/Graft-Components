<script setup lang="ts">
import { mdiClose, mdiLoading } from '@mdi/js';
import Icon from './Icon.vue';

interface Props {
    active: boolean;
    title: string;
    loading?: boolean;
    size?: 'small' | 'medium' | 'large' | 'full';
}

withDefaults(defineProps<Props>(), {
    size: 'small',
});

const emits = defineEmits(['close']);
</script>
<template>
    <div v-if="active" :class="'fixed bg-black/30 w-screen h-screen top-0 left-0 z-[99999]'" @click="$emit('close')" />
    <div
        :class="[
            ['fixed left-1/2 -translate-x-1/2 top-1/2 -translate-y-1/2 bg-white min-w-[300px] rounded-lg overflow-hidden transition-transform ease-in-out duration-300 z-[9999999]', {'h-[95%] aspect-square' :size === 'full'}],
            active ? 'scale-100' : 'scale-0',
        ]"
    >
        <!-- Loading Overlay -->
        <div v-if="loading" class="absolute top-0 left-0 w-full h-full bg-black/60 z-20 flex justify-center items-center">
            <Icon :path="mdiLoading" class="animate-spin w-10 h-10 fill-white" />
        </div>

        <div class="flex items-center justify-between bg-primary-950 text-white text-3xl p-3 px-4 relative z-[9999999]">
            {{ title }}
            <div class="cursor-pointer z-[9999999]">
                <Icon :path="mdiClose" :path-class="'font-bold'" :class="'w-6 h-6 fill-white hover:fill-crimson-500'" @click="emits('close', !active)" />
            </div>
        </div>

        <div :class="'relative p-5 z-[999999]'">
            <slot />
        </div>
    </div>
</template>
