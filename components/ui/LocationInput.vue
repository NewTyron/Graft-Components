<script setup lang="ts">
import { errorMessages } from '@vue/compiler-core';
import InputList from './InputList.vue';

interface Props {
    errorMessage?: string;
}

defineProps<Props>();
const emits = defineEmits(['blur', 'submit']);

const address = ref<string>('');
const coords = ref<{ lat?: number; lng?: number } | null>(null);
const error = ref<string | null>(null);
const locationOptions = ref<{ label: string; value: { lat: number; lng: number } }[]>([
    { label: 'Current Location', value: { lat: 0, lng: 0 } },
]);
const listInput = ref<string>('');
const loading = ref<boolean>(false);

const handleSearchLocation = async () => {
    try {
        error.value = null;
        coords.value = await geocodeAddress(address.value);
    } catch (err: any) {
        error.value = err.message;
    }
};

const handleGetCurrentLocation = async () => {
    try {
        error.value = null;
        coords.value = await getCurrentLocation();
        return coords.value;
    } catch (err: any) {
        error.value = err.message;
    }
};

const autoCompleteAddress = async () => {
    loading.value = true;

    locationOptions.value = [{ label: 'Current Location', value: { lat: 0, lng: 0 } }];
    let retrievedLocations: { label: string; value: { lat: number; lng: number } }[] = [];

    const locations = await searchLocations(address.value);

    retrievedLocations = await Promise.all(
        locations.map(async (loc: any) => ({
            label: (await reverseGeocode(loc.lat, loc.lng)).fullAddress,
            value: { lat: loc.lat, lng: loc.lng },
        }))
    );

    locationOptions.value = [...locationOptions.value, ...retrievedLocations];
    loading.value = false;
};

const handleSubmit = (val: { lat: number; lng: number }) => {
    if (val.lat === 0 && val.lng === 0) {
        handleGetCurrentLocation().then((res) => {
            coords.value = { lat: res?.lat, lng: res?.lng };
            emits('submit', coords.value);
        });
    } else {
        coords.value = val;
        emits('submit', coords.value);
    }
};

let debounceTimeout: ReturnType<typeof setTimeout> | null = null;

watch(listInput, (newVal) => {
    address.value = newVal;

    if (debounceTimeout) {
        clearTimeout(debounceTimeout);
    }

    debounceTimeout = setTimeout(async () => {
        await autoCompleteAddress();
        debounceTimeout = null;
    }, 1000);
});

watchEffect(() => {
    if (coords.value) {
        reverseGeocode(coords.value.lat, coords.value.lng).then((addr) => {
            listInput.value = addr.fullAddress;
        });
    }
});
</script>
<template>
    <div class="flex flex-col gap-5 overflow-visible z-20 w-full">
        <InputList
            :list-options="locationOptions"
            :label="'Where do you need the service?'"
            :model-value="listInput"
            @update:model-value="(val) => (listInput = val)"
            @submit="(val) => handleSubmit(val)"
            :forceClose="!!locationOptions.find((x) => x.label === listInput)"
            :loading="loading"
            @blur="emits('blur')"
            :error-message="errorMessage"
        />
    </div>
</template>
