<script setup lang="ts">
import { mdiClose, mdiEye, mdiEyeOff, mdiLoading } from '@mdi/js';
import Icon from './Icon.vue';

interface Props {
    label: string;
    modelValue?: any;
    iconPath?: string;
    wrapperClass?: string;
    inputClass?: string;
    clearable?: boolean;
    loading?: boolean;
    type?: 'text' | 'password' | 'email' | 'number';
    errorMessage?: string | undefined;
}

const props = withDefaults(defineProps<Props>(), {
    type: 'text',
});

const emits = defineEmits(['update:modelValue', 'focusin', 'focusout', 'iconClick', 'blur']);

const typeRef = ref<'text' | 'password' | 'email' | 'number'>(props.type);

const value = computed({
    get() {
        return props.modelValue;
    },
    set(val) {
        emits('update:modelValue', val);
    },
});
</script>

<template>
    <div
        :class="[
            'flex flex-col items-center justify-center relative mt-3 w-full z-10',
            { 'mb-2': errorMessage },
            wrapperClass,
        ]"
    >
        <input
            :class="[
                'border rounded px-4 py-2 w-full focus:outline-none peer',
                errorMessage ? 'border-red-600' : 'border-primary-950',
            ]"
            v-model="value"
            @focusin="emits('focusin', $event)"
            @focusout="emits('focusout', $event)"
            :type="typeRef"
            @blur="emits('blur')"
        />
        <label
            :class="[
                'text-primary-950 mb-2 absolute -translate-y-1/2 whitespace-nowrap peer-focus:left-1 peer-focus:-top-0.5 bg-white peer-focus:text-xs pointer-events-none transition-all duration-300 px-1',
                value && value.length > 0 ? 'text-xs -top-0.5 left-1' : 'top-1/2 left-3',
            ]"
            >{{ props.label }}</label
        >
        <Icon
            v-if="props.iconPath && type !== 'password'"
            :path="loading ? mdiLoading : props.iconPath"
            :class="['absolute right-2 top-1/2 -translate-y-1/2 h-6 w-6 cursor-pointer', { 'animate-spin': loading }]"
            @click="emits('iconClick')"
        />
        <Icon
            v-else-if="value && clearable && type !== 'password'"
            :path="mdiClose"
            class="absolute right-2 top-1/2 -translate-y-1/2 h-6 w-6 fill-primary-700 cursor-pointer"
            @click="value = undefined"
        />
        <Icon
            v-else-if="type === 'password'"
            :path="typeRef === 'password' ? mdiEye : mdiEyeOff"
            class="absolute right-2 top-1/2 -translate-y-1/2 h-6 w-6 fill-primary-700 cursor-pointer"
            @click="typeRef = typeRef === 'password' ? 'text' : 'password'"
        />

        <label class="absolute left-1 text-red-600 bottom-0 translate-y-full text-xs">{{ errorMessage }}</label>
    </div>
</template>

<style>
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
input:-webkit-autofill:active {
    -webkit-background-clip: text;
    -webkit-text-fill-color: #000000;
    transition: background-color 5000s ease-in-out 0s;
    box-shadow: inset 0 0 20px 20px #ffffff00;
}
</style>
