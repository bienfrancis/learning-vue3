<template>
    <h1 class="text-2xl">{{ title }}</h1>
</template>

<script setup>

defineProps({
    title: {
        type: String,
        default: '',
    },    
})

</script>