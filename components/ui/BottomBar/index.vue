<script setup lang="ts">
import { mdiAccount, mdiHammer, mdiHome, mdiTruck } from '@mdi/js';
import { useAuth } from '~/store/auth';

const route = useRoute();
const auth = useAuth();
</script>
<template>
    <div class="h-15 bg-primary-950 w-screen fixed bottom-0 left-0 flex items-center justify-between pb-2 md:px-10">
        <UiBottomBarBottomNavItem
            :active="route.name === 'home' || route.name === 'index'"
            :to="'/home'"
            :icon-path="mdiHome"
            :label="'Home'"
        />
        <UiBottomBarBottomNavItem
            :active="route.name === 'active-jobs'"
            :to="'/active-jobs'"
            :icon-path="mdiTruck"
            :label="'Active Jobs'"
        />
        <UiBottomBarBottomNavItem
            :active="route.name === 'activity'"
            :to="'/activity'"
            :icon-path="mdiHammer"
            :label="'Activity'"
        />
        <UiBottomBarBottomNavItem
            :active="route.name === 'account'"
            :to="auth.isLoggedIn ? '/account' : undefined"
            :icon-path="mdiAccount"
            :label="'Account'"
            @click="!auth.isLoggedIn ? auth.loginModal = true : ''"
        />
    </div>
</template>
