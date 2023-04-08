<template>
  <div>
    <div v-if="!addModal">
      <button
        @click="openAddModal"
        class="flex justify-end bg-blue-500  hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full m-5"
      >
        Add Project
      </button>
    </div>
    <div>
    <CollectionList :ProjectsData="ProjectsData" /></div>
    <div v-if="addModal">
    
     <CollectionAdd  :addModal="addModal"  @addDetails="addDetails"/>
     
    
  </div>

  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { CheckIcon } from "@heroicons/vue/24/outline";

// defineProps<{
//   title?: string
//   likes?: number
// }>()

const open = ref(true);

const addModal = ref(false);

const openAddModal = () => {
  console.log("addModal--->",addModal.value)
  addModal.value = true;
}

const closeModal =() => {
  addModal.value = false;
}


const addDetails = (obj: Object) => {
const postOptions = {
  
  method: "POST",

  // headers: {getData
  //   "Content-Type": "application/json",

  //   Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiODQxZGNhZGY4OThkNDBiOTgzM2ZlY2VjMGIyZTQ3MTAiLCJkIjoiMTY4MDEyMSIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODM1MjQyMDN9.uLBXTLIoafw6TwTSv7SBEEr-c-pkf8YGozaYkOvQhEI`,
  // },

  body: obj
};

const data = useAuthLazyFetchPost(
  "https://v1-orm-gharpe.mercury.infinity-api.net/api/projects/",

  postOptions
);

}

const ProjectsData = useLazyFetch(

"https://v1-orm-gharpe.mercury.infinity-api.net/api/projects/?offset=0&limit=100&sort_column=id&sort_direction=desc"

);

console.log("ProjectsData----------", ProjectsData)

</script>
