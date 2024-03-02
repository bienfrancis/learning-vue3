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
import axiosInstance from '../helpers/axios';

const props = defineProps({
    inputTask: {
        type: String,
        default: '',
    },
})

const lists = ref([]);

const addNewTasks = async (taskName) => {
    try {
        const response = await axiosInstance.post('/create',{
            title: taskName
        })
        const newTask = response.data
        lists.value.unshift(newTask)
        console.log(lists.value)
    } catch (error) {
        Swal.fire({
            position: "top-end",
            icon: "error",
            title: "Error creating task",
            showConfirmButton: false,
            timer: 3500
        });
    }
}

onMounted( async () => {
    try {
        const response = await axiosInstance.get()
        lists.value = response.data
    } catch (error) {
        Swal.fire({
            position: "top-end",
            icon: "error",
            title: "Error fetching task:",
            showConfirmButton: false,
            timer: 3500
        });
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
                const delResponse = await axiosInstance.delete(`/delete/${taskID}`);
                const deleteItem = delResponse.data;
                Swal.fire({
                    title: "Deleted!",
                    text: "Your task has been deleted.",
                    icon: "success"
                });
                lists.value = lists.value.filter(item => item.id !== deleteItem.id);
          } catch (error) {
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