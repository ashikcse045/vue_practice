<script setup>
import { defineProps } from 'vue';
import NavItem from './NavItem.vue';

const { user, currentComp } = defineProps([
    'user',
    'currentComp'
]);
const emit = defineEmits(['changeComponent']);
const changeComponent = str => {
    emit('changeComponent', str);
}

</script>

<template>
    <NavItem :user="user" @changeComponent="changeComponent" :currentComp="currentComp" />
    <slot />
</template>