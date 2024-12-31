<script setup>
import { watchEffect , ref} from 'vue';
import { AnOutlinedCloseCircle } from '@kalimahapps/vue-icons';
const computer = ref({})
const emit = defineEmits(['close'])
const props = defineProps({
    id: String,
})
watchEffect(() => {
    const computers = JSON.parse(localStorage.getItem('computers'))
    computer.value = computers[props.id]
    console.log( props.id, computers[props.id] );
})
</script>
<template>
    <div class="flex flex-col space-y-4 p-3 md:p-6 rounded-lg shadow-md bg-white relative">
        <p class=" text-xl text-teal-700 font-bold">Détails</p>
        <button class="absolute top-1 right-5 "  @click="$emit('close')"><AnOutlinedCloseCircle class="text-teal-700 text-xl" /></button>
        <p><span class=" font-semibold">Nom :</span>  <span>{{ computer.name }}</span> </p>
        <p><span class=" font-semibold">Processeur :</span>  <span>{{ computer.processor }}</span> </p>
        <p><span class=" font-semibold">Mémoire Ram :</span>  <span>{{ computer.ram }}</span> </p>
        <p><span class=" font-semibold">Disque Dur :</span>  <span>{{ computer.storage }}</span> </p>
        <p><span class=" font-semibold">Etat de l'ordinateur :</span>  <span>{{ computer.state }}</span> </p>
        <p><span class=" font-semibold">Prix :</span>  <span>{{ computer.price }}</span> </p>
        <button @click="$emit('close')" 
        class="bg-teal-700 px-5 py-1.5 w-fit mx-auto text-white rounded-md font-semibold flex gap-2 items-center"
        >Ok</button>
    </div>
</template>