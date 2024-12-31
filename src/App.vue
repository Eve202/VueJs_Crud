<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import CardPc from "./components/Card.vue";
import CreatePc from "./components/Create.vue";
import ReadPc from "./components/Read.vue";
import UpdatePc from "./components/Update.vue";
import { AkCirclePlus } from "@kalimahapps/vue-icons";
import { ref, computed, watchEffect } from "vue";
const show = ref(false);
const type = ref("");
const idSelected = ref("");
const listComputers = ref([]);
const count_pc = computed(() => listComputers.length);

const action = (newtype, id = "") => {
  type.value = newtype;
  idSelected.value = id;
  show.value = true;
};
const close = () => {
  listComputers.value = localStorage.getItem("computers") ? JSON.parse(localStorage.getItem("computers")) : [];
  show.value = false;
}
watchEffect(() => {
  listComputers.value = localStorage.getItem("computers") ? JSON.parse(localStorage.getItem("computers")) : [];

}) ;


</script>

<template>
  <main class="max-w-5xl  mx-auto p-5 min-h-screen relative">
    <h2 class="text-teal-700 text-2xl md:text-4xl font-bold text-center">
      Crud with Vue Js
    </h2>
    <!--  -->
    <div
      class="grid grid-cols-1 gap-5 md:grid-cols-2 lg:grid-cols-3 place-items-center my-12"
    >
      <CardPc
        v-for="computer in listComputers"
        :computer="computer"
        :key="computer.id"
        @action="action"
        @close="close"
      />
    </div>
    <Teleport to="body" >
      <div v-show="show" @click.self="show = false" class="bg-black bg-opacity-20 flex items-center  justify-center px-2 py-5 md:py-8 fixed top-0 left-0 h-screen w-full">
        <div class="scrollYDiv overflow-y-scroll  max-w-xl w-full max-h-[680px] mx-auto">
      <!--  -->
      <CreatePc v-if="type === 'Create'"  @close="close" />

      <!--  -->
      <ReadPc v-else-if="type === 'Read' " :id="idSelected" @close="close" />

      <!--  -->
      <UpdatePc v-else-if="type === 'Update' " :id="idSelected" @close="close"  />
    </div>
  </div>
    </Teleport>

    <button
      @click="action('Create')"
      :class="[
        count_pc ? '' : 'animate-bounce',
        'bg-teal-700 px-5 py-1.5 fixed bottom-5 translate-x-1/2 text-white rounded-md font-semibold flex gap-2 items-center',
      ]"
    >
      Ajouter<AkCirclePlus />
    </button>
  </main>
</template>

<style scoped>
 .scrollYDiv::-webkit-scrollbar {
  display: none;
}

</style>
