    
<script setup>
import { RouterLink } from 'vue-router'
defineProps({
    to: {
        type: String
    }
})

</script>
<template>
    <RouterLink
        class="block md:inline-block rounded bg-blue-600 py-2 px-3 text-white uppercase font-bold text-sm shadow hover:bg-blue-500"
        :to="{ name: to }">
        <slot></slot>
    </RouterLink>
</template>
