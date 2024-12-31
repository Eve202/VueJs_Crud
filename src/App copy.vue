<script setup>
import HelloWorld from './components/HelloWorld.vue'
import CardPc from './components/Card.vue'
import CreatePc from './components/Create.vue'
import ReadPc from './components/Read.vue';
import UpdatePc from './components/Update.vue';
import { AkCirclePlus } from '@kalimahapps/vue-icons';
import { ref , computed, watchEffect } from 'vue'

const computers = ref([])
const count_pc = computed(() => computers.length)
const show = ref(true)
const type = ref('')
const idSelected = ref('')
const setShow = () => {
  show.value = !show;
}
const listComputers = ref([]);
const action = (newtype, pc) => {
  type.value = newtype;
  idSelected.value  = pc.id;
  show.value = true

  if (newtype == "Delete") {
    listComputers.value = listComputers.value.splice(listComputers.value.indexOf(pc),1 )
   localStorage.setItem('computers',JSON.stringify(listComputers.value))
    // console.log(filterPc,listComputers.value.splice(listComputers.value.indexOf(pc),1 ))
  }
}
   listComputers.value = localStorage.getItem('computers') ? JSON.parse(localStorage.getItem('computers')) : [] 

// watchEffect(() => {
//   localStorage.setItem('computers',JSON.stringify(
//     [
//   { id: 1, name: "Lenovo ThinkPad X1 Carbon", brand: "Lenovo", processor: "Intel Core i7-1260P", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11 Pro", price: 1800 },
//   { id: 2, name: "Asus ROG Zephyrus G14", brand: "Asus", processor: "AMD Ryzen 9 5900HS", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 2200 },
//   { id: 3, name: "Dell Inspiron 15 3000", brand: "Dell", processor: "Intel Core i5-1135G7", ram: "8 Go", storage: "256 Go SSD", os: "Windows 11", price: 750 },
//   { id: 4, name: "HP Spectre x360 14", brand: "HP", processor: "Intel Core i7-1165G7", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11", price: 1500 },
//   { id: 5, name: "Lenovo IdeaPad 3", brand: "Lenovo", processor: "AMD Ryzen 5 5500U", ram: "8 Go", storage: "512 Go SSD", os: "Windows 11", price: 600 },
//   { id: 6, name: "Asus TUF Gaming F15", brand: "Asus", processor: "Intel Core i7-12700H", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 1400 },
//   { id: 7, name: "Dell XPS 13 Plus", brand: "Dell", processor: "Intel Core i7-1280P", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 2000 },
//   { id: 8, name: "HP Pavilion 15", brand: "HP", processor: "AMD Ryzen 7 5700U", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11", price: 850 },
//   { id: 9, name: "Lenovo Legion 5 Pro", brand: "Lenovo", processor: "AMD Ryzen 7 5800H", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11", price: 1300 },
//   { id: 10, name: "Asus VivoBook 15", brand: "Asus", processor: "Intel Core i3-1115G4", ram: "8 Go", storage: "256 Go SSD", os: "Windows 11", price: 500 },
//   { id: 11, name: "Dell Latitude 7420", brand: "Dell", processor: "Intel Core i7-1165G7", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11 Pro", price: 1800 },
//   { id: 12, name: "HP EliteBook 840 G8", brand: "HP", processor: "Intel Core i7-1165G7", ram: "16 Go", storage: "1 To SSD", os: "Windows 11 Pro", price: 1900 },
//   { id: 13, name: "Lenovo Yoga 9i", brand: "Lenovo", processor: "Intel Core i7-1185G7", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 1700 },
//   { id: 14, name: "Asus ZenBook 14 OLED", brand: "Asus", processor: "AMD Ryzen 7 5800U", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11", price: 1300 },
//   { id: 15, name: "Dell G15 Ryzen Edition", brand: "Dell", processor: "AMD Ryzen 5 5600H", ram: "8 Go", storage: "512 Go SSD", os: "Windows 11", price: 1200 },
//   { id: 16, name: "HP Omen 16", brand: "HP", processor: "Intel Core i7-12700H", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 1700 },
//   { id: 17, name: "Lenovo ThinkBook 14s Yoga", brand: "Lenovo", processor: "Intel Core i5-1135G7", ram: "8 Go", storage: "512 Go SSD", os: "Windows 11", price: 900 },
//   { id: 18, name: "Asus ProArt Studiobook", brand: "Asus", processor: "Intel Core i9-12900H", ram: "32 Go", storage: "2 To SSD", os: "Windows 11 Pro", price: 3000 },
//   { id: 19, name: "Dell Precision 5560", brand: "Dell", processor: "Intel Core i7-11850H", ram: "32 Go", storage: "1 To SSD", os: "Windows 11 Pro", price: 2400 },
//   { id: 20, name: "HP ZBook Firefly G8", brand: "HP", processor: "Intel Core i7-1165G7", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11 Pro", price: 2000 },
//   { id: 21, name: "Lenovo Legion 7i", brand: "Lenovo", processor: "Intel Core i7-11800H", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 2200 },
//   { id: 22, name: "Asus Chromebook Flip", brand: "Asus", processor: "Intel Celeron N4020", ram: "4 Go", storage: "64 Go eMMC", os: "Chrome OS", price: 350 },
//   { id: 23, name: "Dell Vostro 15 5510", brand: "Dell", processor: "Intel Core i5-11320H", ram: "8 Go", storage: "512 Go SSD", os: "Windows 11", price: 800 },
//   { id: 24, name: "HP Envy x360", brand: "HP", processor: "AMD Ryzen 5 4500U", ram: "8 Go", storage: "256 Go SSD", os: "Windows 11", price: 950 },
//   { id: 25, name: "Lenovo ThinkPad E15", brand: "Lenovo", processor: "AMD Ryzen 7 5700U", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 1400 },
//   { id: 26, name: "Asus ROG Strix G15", brand: "Asus", processor: "AMD Ryzen 7 5800H", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 1500 },
//   { id: 27, name: "Dell Alienware m15 R6", brand: "Dell", processor: "Intel Core i7-11800H", ram: "16 Go", storage: "1 To SSD", os: "Windows 11", price: 2300 },
//   { id: 28, name: "HP Chromebook x2 11", brand: "HP", processor: "Qualcomm Snapdragon 7c", ram: "8 Go", storage: "128 Go eMMC", os: "Chrome OS", price: 700 },
//   // Ajoutez plus d'ordinateurs jusqu'à 50 pour compléter la liste.
// ]))  
// })
</script>

<template >
 <main class="max-w-5xl mx-auto p-5 min-h-screen relative">
  <h2 class="text-teal-700 text-2xl md:text-4xl font-bold text-center">Crud with Vue Js </h2>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet tenetur labore exercitationem, minus, voluptate aperiam distinctio repellat iusto dolorem sapiente facilis quo iure odio deserunt, repudiandae asperiores! Fugit, debitis cupiditate?</p>
  <!--  -->
  <div class="grid gri-cols-1 gap-5 sm:grid-cols-2 md:grid-cols-3 place-items-center my-12">
    <CardPc v-for="computer in listComputers" :computer="computer" :key="computer.id"  @action="action"  />
  </div> 
  <!--  -->
  <CreatePc v-if="type === 'Create' && show " />

  <!--  -->
  <ReadPc v-if="type === 'Read' && show" :id="idSelected"/>

  <!--  -->
  <UpdatePc v-if="type === 'Update' && show" :id="idSelected"/>

  <button @click="type = 'Create'" :class="[count_pc ? '' : 'animate-bounce','bg-teal-700 px-5 py-1.5 fixed bottom-5 left-1/2  -translate-x-1/2 text-white rounded-md font-semibold flex gap-2 items-center']">Ajouter<AkCirclePlus/></button>
 </main>
</template>

<style scoped>
  /* .app{
    background-color: #42b883;
  } */
</style>
