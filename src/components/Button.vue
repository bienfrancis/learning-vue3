<template>
     <button 
        class="rounded-md shadow-md bg-indigo-500 text-white py-2 px-4 hover:bg-indigo-600"
    >
        {{ props.buttonTitle }}
    </button>
</template>

<script setup>

const props = defineProps({
    buttonTitle:{
        type: String,
        default: '',
    }
})

</script>