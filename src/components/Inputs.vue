<template>
    <div class="flex flex-col">
        <label :class="labelClasses">{{ label }}</label>
       <div class="flex gap-2 justify-between">
            <input 
                :type="inputType" 
                :name="name"
                :placeholder="placeholder"
                class="px-3 rounded-md shadow-md focus:border-indigo-500 flex-1"
               v-model="inputValue"
            >
            <Button 
                @click="AddTask"
                button-title="Add"    
            />
       </div>
    </div>
</template>

<script setup>

    import {ref} from 'vue'
    import Button from './Button.vue';

    defineProps({
        inputType:{
            type: String,
            default: '',
        },
        name:{
            type: String,
            default: '',
        },
        placeholder: {
            type: String,
            default: '',
        },
        label:{
            type: String,
            default: '',
        },
        btnTitle:{
            type: String,
            default: '',
        },
        labelClasses: {
            type: String,
            default: '',
        },
    })

    const emit = defineEmits(['addNewTask'])

    const inputValue = ref('')

    const AddTask = () => {
        emit('addNewTask',  inputValue.value)
        inputValue.value = ''
    }
</script>