<script setup>
import { ref, reactive, watchEffect } from "vue";
import { BsPatchCheckFill, MdComputer ,AnOutlinedCloseCircle } from "@kalimahapps/vue-icons";
const props = defineProps({
    id : String
})
const emit = defineEmits(["close"]);
const oldvalues = ref({})
const name = ref('');
const processor = ref('');
const ram = reactive({ size: 2, unit: 'Go'});
const storage = reactive({ size: 256, unit: 'Go' , type: 'To' });
const state = ref( 'venu');
const price = ref( 20);
const picture = ref( '');
const error = reactive({ picture: "", name: "", processor: "" });

// Fonction pour gérer l'événement 'change'
const handleFileUpload = (event) => {
  console.log(event.target.value);
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
  if (error.name || error.name || error.processor) {
    return false;
  } else {
    storeData();
    
  }
};
const storeData = () => {
  let oldComputer = JSON.parse(localStorage.getItem("computers")) ;
  
    oldComputer[props.id] = {
      'id': props.id,
      'picture' : picture.value,
      'name': name.value,
      'processor': processor.value,
      'ram': ram.size + " " + ram.unit,
      'storage': storage.size + " " + storage.unit + " " + storage.type,
      'state': state.value,
      'price': price.value,
    };
  
  localStorage.setItem(
    "computers",
    JSON.stringify(oldComputer)
  );
  emit("close");
};

watchEffect(() => {
    oldvalues.value = JSON.parse(localStorage.getItem('computers'))[props.id]
    console.log(oldvalues.value, oldvalues.value.ram, )
    name.value = oldvalues.value.name;
    processor.value = oldvalues.value.processor ;
    
    ram.size = oldvalues.value.ram.split(' ')[0];
    ram.unit = oldvalues.value.ram.split(' ')[1] ;

    storage.size = oldvalues.value.storage.split(' ')[0];
    storage.unit = oldvalues.value.storage.split(' ')[1] ;
    storage.type = oldvalues.value.storage.split(' ')[2] ;
    
    state.value = oldvalues.value.state;
    price.value = oldvalues.value.price;
    picture.value = oldvalues.value.picture;
})


//   { id: 1, name: "Lenovo ThinkPad X1 Carbon", brand: "Lenovo", processor: "Intel Core i7-1260P", ram: "16 Go", storage: "512 Go SSD", os: "Windows 11 Pro", price: 1800 },
</script>
<template>
  <div class="relative bg-white">
    <button type="button" class="absolute top-5 right-2 "  @click.stop="$emit('close')"><AnOutlinedCloseCircle class="text-teal-700 text-xl" /></button>
  <form action="" method="post" enctype="multipart/form-data" >

    <div class="flex flex-col space-y-4 p-3 md:p-6 rounded-lg shadow-md ">
      <div>
        <img :src="picture" alt="" class="w-40 h-40 mb-2">
        <label
          for="picture"
          class="bg-teal-700 px-5 py-1.5 w-fit text-white rounded-md font-semibold flex gap-2 items-center"
        >
          Modifier une image
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
        Modify
      </button>
    </div>
  </form>
  </div>
</template>
