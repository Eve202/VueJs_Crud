<script setup>
import { ref, reactive } from "vue";
import { BsPatchCheckFill, MdComputer  ,AnOutlinedCloseCircle } from "@kalimahapps/vue-icons";
const emit = defineEmits(["close"]);
const name = ref("");
const processor = ref("");
const ram = reactive({ size: 2, unit: "Go" });
const storage = reactive({ size: 256, unit: "Go", type: "HDD" });
const state = ref("venu");
const price = ref(1);
const picture = ref("");
const error = reactive({ picture: "", name: "", processor: "" });

// Fonction pour gérer l'événement 'change'
const handleFileUpload = (event) => {
  const files = event.target.files;
  if (files && files.length > 0) {
    const url = new FileReader();
    url.readAsDataURL(files[0]);
    url.addEventListener("load", () => {
      picture.value = url.result;
    });
  }
};
const verifydata = () => {
  error.picture = error.name = error.processor = "";
  if (picture.value === "") {
    error.picture = "Choisissez une image de l'ordinateur";
  }
  if (name.value === "") {
    error.name = "Ce champ est requis.";
  }
  if (processor.value === "") {
    error.processor = "Ce champ est requis.";
  }
  if (error.picture || error.name || error.processor) {
    return false;
  } else {
    storeData();
    emit("close");
                     
  }
};
const storeData = () => {
  const characters =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
  let generateId = "";
  let oldComputer =  {};
  for (let i = 0; i < 4; i++) {
      const randomIndex = Math.floor(Math.random() * characters.length);
      generateId += characters[randomIndex];
    }
    // console.log(oldComputer,typeof oldComputer) 
    if (localStorage.getItem("computers") ) {
      oldComputer = JSON.parse(localStorage.getItem("computers"))
      // console.log(!Array.isArray(localStorage.getItem("computers")), localStorage.getItem("computers"))
    } 
    oldComputer[generateId] = {
      'id': generateId,
      'picture' : picture.value,
      'name': name.value,
      'processor': processor.value,
      'ram': ram.size + " " + ram.unit,
      'storage': storage.size + " " + storage.unit + " " + storage.type,
      'state': state.value,
      'price': price.value,
    };
    // console.log(oldComputer,JSON.stringify(oldComputer)) 
  localStorage.setItem("computers",JSON.stringify(oldComputer)
  );
};
//   { id: 1, name: "Lenovo ThinkPad X1 Carbon", brand: "Lenovo", processor: "Intel Core i7-1260P", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11 Pro", price: 1800 },
</script>
<template>
  <div class="bg-white relative">
    <button class="absolute top-2 right-2 "  @click="$emit('close')"><AnOutlinedCloseCircle class="text-teal-700 text-xl" /></button>
    <form action="" method="post" enctype="multipart/form-data" >

    <div class="flex flex-col space-y-4 p-3 md:p-6 rounded-lg shadow-md">
      <div>
        <label
          for="picture"
          class="bg-teal-700 px-5 py-1.5 w-fit text-white rounded-md font-semibold flex gap-2 items-center"
        >
          Choisissez une image
          <span v-if="picture !== ''"
            ><BsPatchCheckFill class="text-white text-xl"
          /></span>
          <span v-else><MdComputer class="text-white text-xl" /></span>
        </label>
        <input
          class="hidden"
          id="picture"
          name="picture"
          type="file"
          accept="image/*"
          @change="handleFileUpload"
        />
        <p v-show="error.picture !== ''" class="text-sm text-red-600">
          {{ error.picture }}
        </p>
      </div>
      <div>
        <label for="name">Insérer le nom du pc</label>
        <input
          class="block p-1.5 mt-2 w-full border-0 border-b border-teal-700 bg-teal-50"
          placeholder="Insérer le nom du pc"
          id="name"
          type="text"
          v-model="name"
        />
        <p v-show="error.name !== ''" class="text-sm text-red-600">
          {{ error.name }}
        </p>
      </div>
      <div>
        <label for="processor">Processeur</label>
        <input
          class="block p-1.5 mt-2 w-full border-0 border-b border-teal-700 bg-teal-50"
          placeholder="Intel Core i7-1260P"
          id="processor"
          type="text"
          v-model="processor"
        />
        <p v-show="error.processor !== ''" class="text-sm text-red-600">
          {{ error.processor }}
        </p>
      </div>
      <div>
        <label>Mémoire Ram</label>
        <div class="grid grid-cols-3 gap-5">
          <select
            class="block p-1.5 mt-2 w-full border border-teal-700 bg-teal-50 col-span-2"
            name="R_size"
            id="R_size"
            v-model="ram.size"
          >
            <option v-for="n in 7" :key="n">{{ Math.pow(2, n) }}</option>
          </select>
          <select
            class="block p-1.5 mt-2 w-full border border-teal-700 bg-teal-50 col-span-1"
            name="R_unit"
            id="R_unit"
            v-model="ram.unit"
          >
            <option>Go</option>
            <option>To</option>
          </select>
        </div>
      </div>
      <div>
        <label>Disque dur</label>
        <div class="grid grid-cols-3 gap-5">
          <select
            class="block p-1.5 mt-2 w-full border border-teal-700 bg-teal-50"
            name="D_size"
            id="D_size"
            v-model="storage.size"
          >
            <option>256</option>
            <option>512</option>
            <option>1</option>
          </select>
          <select
            class="block p-1.5 mt-2 w-full border border-teal-700 bg-teal-50"
            name="D_unit"
            id="D_unit"
            v-model="storage.unit"
          >
            <option>Go</option>
            <option>To</option>
          </select>
          <select
            class="block p-1.5 mt-2 w-full border border-teal-700 bg-teal-50"
            name="D_type"
            id="D_type"
            v-model="storage.type"
          >
            <option>HDD</option>
            <option>SSD</option>
          </select>
        </div>
      </div>
      <div>
        <label for="price">Insérer le prix</label>
        <input
          class="block p-1.5 mt-2 w-full border-0 border-b border-teal-700 bg-teal-50"
          placeholder="300000"
          id="price"
          type="number"
          min="1"
          v-model="price"
        />
      </div>
      <div>
        <label for="state">L'état de l'ordinateur</label>
        <div>
          <input type="radio" id="neuf" value="neuf" v-model="state" />
          <label for="neuf" class="ml-2">Neuf</label>

          <input
            type="radio"
            id="venu"
            value="venu"
            v-model="state"
            class="ml-6 mr-2"
          />
          <label for="venu">Venu</label>
        </div>
      </div>
      <button
        @click.prevent="verifydata"
        class="bg-teal-700 px-5 py-1.5 w-fit mx-auto text-white rounded-md font-semibold flex gap-2 items-center"
      >
        Créer
      </button>
    </div>
  </form>
  </div>
</template>
