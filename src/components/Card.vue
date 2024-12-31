<script setup>
import { ByTrash , FaRegEye, AkEdit} from '@kalimahapps/vue-icons';

const   props = defineProps({
    computer : Object,
})
const emit = defineEmits(['action', 'close'])
const delete_pc = () => {
    const newlist = JSON.parse(localStorage.getItem("computers"));
    delete newlist[props.computer.id]; 
    localStorage.setItem('computers',JSON.stringify(newlist))
    emit('close');
}

</script>
<template>
    <div class="w-80  h-96 border-t p-3 rounded-xl shadow-xl" >
        <img :src="computer.picture" class="w-full h-60 object-contain  shadow-inner rounded-xl border" />
        <div class="flex flex-col space-y-1 pt-1.5" >
            <p><span class="font-semibold">Nom: </span> {{ computer.name }} </p>
            <p><span class="font-semibold">Disque dur : </span>{{ computer.storage }}</p>
            <p><span class="font-semibold"> Ram : </span>{{ computer.ram }}</p>
            <div class="flex items-end  justify-between ">
                <p class="text-lg font-bold ">{{ computer.price }} fcfa</p>
                <div class="flex space-x-2">
                    <button @click="$emit('action' , 'Read', computer.id)" class="bg-cyan-700 p-1 rounded-md "><FaRegEye  class="text-white  text-xl"/></button>
                    <button @click="$emit('action' , 'Update', computer.id)" class="bg-emerald-700 p-1 rounded-md "><AkEdit class="text-white  text-xl"/></button>
                    <button @click="delete_pc" class="bg-red-500 p-1 rounded-md "><ByTrash  class="text-white  text-xl"/></button>
                </div>
            </div>
        </div>
    </div>
</template>