<script setup lang="ts">
import Input from './Input.vue';

interface Props {
    label: string;
    listOptions: { label: string; value: string | object | undefined | number }[];
    modelValue?: any;
    wrapperClass?: string;
    inputClass?: string;
    iconPath?: string;
    forceClose?: boolean;
    loading?: boolean;
    errorMessage?: string;
}

const props = defineProps<Props>();
const emits = defineEmits(['update:modelValue', 'submit', 'blur']);

const focussed = ref<boolean>(false);

const value = computed({
    get() {
        return props.modelValue;
    },
    set(val) {
        emits('update:modelValue', val);
        submitted.value = false;
    },
});

const submitted = ref<boolean>(props.forceClose);
</script>
<template>
    <div :class="['flex flex-col relative w-full', wrapperClass]">
        <Input
            :label="label"
            :model-value="modelValue"
            @update:model-value="(val) => (value = val)"
            class="z-10"
            @focusin="focussed = true"
            @focusout="focussed = true"
            :icon-path="iconPath"
            @icon-click="() => emits('submit', value)"
            @blur="emits('blur')"
            :error-message="errorMessage"
        />
        <!-- dropdown -->
        <div
            v-if="listOptions && value && !forceClose && !loading && !submitted"
            :class="[
                'flex-col px-3 border-x border-b border-primary-700 rounded-b peer-focus:hidden -translate-y-[2px] absolute w-full top-full bg-white shadow-2xl z-30',
                focussed ? 'flex' : 'hidden',
                inputClass,
            ]"
        >
            <div
                v-for="(option, index) in listOptions.slice(0, 5)"
                @click="
                    submitted = true;
                    emits('submit', option.value);
                "
                :class="[
                    'w-full border-b border-b-primary-700 my-2 text-sm pb-2 flex items-center cursor-pointer',
                    { 'pt-2': index === 0, 'border-b-0 mb-0': index === listOptions.length - 1 },
                ]"
            >
                {{ option.label }}
            </div>
        </div>
        <div
            v-else-if="!forceClose && value && loading"
            :class="[
                'flex-col px-3 border-x border-b border-primary-700 rounded-b peer-focus:hidden -translate-y-[2px] absolute w-full top-full bg-white shadow-2xl',
                focussed ? 'flex' : 'hidden',
                inputClass,
            ]"
        >
            <div
                v-for="i in Array.from({ length: 5 })"
                :class="[
                    'w-full border-b border-b-primary-700 my-2 text-sm pb-2 flex items-center cursor-pointer',
                    { 'pt-2': i === 0, 'border-b-0 mb-0': i === listOptions.length - 1 },
                ]"
            >
                <div class="w-[70%] bg-primary-500 animate-pulse h-3 rounded" />
            </div>
        </div>
    </div>
</template>
