<script setup lang="ts">
import Button from './Button.vue';

const fileInput = ref<HTMLInputElement | null>(null);
const fileName = ref('');

const handleFileChange = () => {
    if (fileInput.value && fileInput.value.files && fileInput.value.files.length > 0) {
        fileName.value = fileInput.value.files[0].name;
    } else {
        fileName.value = '';
    }
};

const handleUploadClick = () => {
    fileInput.value?.click();
};
</script>

<template>
    <div class="flex flex-col items-center justify-center relative mt-3 w-full z-10">
        <div
            class="border border-primary-950 rounded w-full h-[42px] flex items-center justify-between overflow-hidden pr-2"
        >
            <Button label="Click to upload" class="whitespace-nowrap shrink-0 mr-2 -translate-x-0.5" @submit="handleUploadClick" />

            <div class="flex-1 text-ellipsis whitespace-nowrap overflow-hidden text-sm text-gray-700" title="File name">
                {{ fileName || 'No file selected' }}
            </div>

            <input ref="fileInput" id="myFileInput" type="file" class="hidden" @change="handleFileChange" />
        </div>
    </div>
</template>
