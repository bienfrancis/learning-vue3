<template>
      <ul
            class="shadow-md mt-4 p-5 bg-white rounded-md"
            v-if="datas.length > 0"
        >
            <li
                v-for="(data, index) in datas"
                :key="data.id"
                class="border-b-[1px] mb-3 flex justify-between items-center"
            >
                <div>
                    <h5 class="text-xl">{{ data.title }}</h5>
                    <small> added {{ moment(data.createdAt).fromNow() }}</small>
                </div>
                <Button
                    button-title="Del"
                    :class="`bg-red-600 text-white hover:bg-red-800`"
                    @click="deleteItem(data.id)"
                />
            </li>
        </ul>
</template>
<script setup>
import { computed, onMounted  } from 'vue';
import moment from 'moment';
import Button from './Button.vue';
import { PlusIcon } from '@heroicons/vue/24/outline'

const props = defineProps({
    datas: {
        type: Object,
        default: () => [],
    },
    relativeDate: {
        type: String,
        default: '',
    }
})

const emit = defineEmits(['deleteData'])

const deleteItem = (id) => {
    emit('deleteData', id)
}


</script>