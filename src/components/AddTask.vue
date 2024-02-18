<template>
<Banner>
    <div class="w-1/3 mx-auto h-full py-3">
        <Title 
            title="Todo" 
            class="text-white uppercase font-bold text-center" 
        />
        <div>
            <Inputs
                label="Add New Task"
                input-type="text"
                placeholder="Add Task"
                btn-title="Add"
                :class="'mt-5'"
                label-classes="text-white mb-2"
                @add-new-task="addNewTasks"
            />
        </div>
      <TasksList
        :datas="lists"
        @delete-data="deleteTask"
      />
    </div>
</Banner>
</template>

<script setup>

import Banner from './Banner.vue';
import Title from './Title.vue';
import Inputs from './Inputs.vue';
import { ref, onMounted, onUpdated, onBeforeMount, computed } from 'vue'
import TasksList from './TasksList.vue';
import axios, * as others from 'axios';
import Swal from 'sweetalert2'

const props = defineProps({
    inputTask: {
        type: String,
        default: '',
    },
})

const lists = ref([]);

const addNewTasks = async (taskName) => {
    try {
        const response = await axios.post('http://localhost:3000/tasks',{
            title: taskName
        })
        const newTask = response.data
        lists.value.push(newTask)
        console.log(lists.value)
    } catch (error) {
        console.error('Error creating task:', error)
    }
}

onMounted( async () => {
    try {
        const response = await axios.get('http://localhost:3000/task')
        lists.value = response.data
    } catch (error) {
        console.error('Error fetching task:', error)
    }
})

const deleteTask = (taskID) => {
    Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!"
    }).then( async (result) => {
        if (result.isConfirmed) {
            try {
                const delResponse = await axios.delete(`http://localhost:3000/task/${taskID}`);
                const deleteItem = delResponse.data;
                Swal.fire({
                title: "Deleted!",
                text: "Your file has been deleted.",
                icon: "success"
                });
                lists.value = lists.value.filter(item => item.id !== deleteItem.id);
          } catch (error) {
                console.error('Error deleting task:', error);
                Swal.fire({
                title: "Error!",
                text: "Failed to delete the task.",
                icon: "error"
                });
          }
        }
    });
}

</script>