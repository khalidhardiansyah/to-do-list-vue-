<template>
  <div class="px-10 py-10 w-full">
    <div >
      
     <div class="grid grid-cols-2 gap-x-24">
      <ul
        role="list"
        class="mt-3 grid grid-cols-1 gap-5 sm:gap-6 sm:grid-cols-2 "
      >
      <div class="tombol col-span-2 flex items-center justify-between">
      <h2 class="text-sm font-medium text-gray-500 ">Todo List</h2>
      <BaseButton btnlabel="Tambah" type="button" @click="handleModal"/>
      <BaseModal :open="openModal" has-top-cancel-button @handle-close="handleModal">
        <div class="title text-center">
          <h3 class="font-bold text-lg">Tambah Activity</h3>
        </div>
        <div class="content">
          <div class="preview my-3">
            <Item :activity="activity.modelValue" :color="color.value" action-button @handlePopOver="popover(color.value)"/>
          </div>
          <form class="space-y-4" @submit="onSubmit">
          <BaseInput label="Activity" inputname="activity" inputtype="text"  v-bind="activity"/>
          <BaseInput label="Warna" inputname="bg-color" inputtype="color" v-bind="color" inputstyle="py-0"/>
          <BaseButton btnlabel="submit" type="submit"/>
          </form>
        </div>
      </BaseModal>
      </div>

        <li
          v-for="item in todos"
          :key="item.id"
          class="relative col-span-1 flex rounded-md shadow-sm"
          draggable="true" @dragstart="dragItem($event, item)"
        >
          <div
            class="bg-red-700 flex-shrink-0 flex items-center justify-center w-16 text-white text-sm font-medium rounded-l-md uppercase"
          >
            {{ item.activity.slice(0, 1) }}
          </div>
          <div
            class="flex flex-1 py-2 max-h-14 items-center justify-between truncate rounded-r-md border-t border-r border-b border-gray-200 bg-white"
          >
            <div class="flex-1 truncate px-4 py-2 text-sm">
          
              <p class="text-gray-500">{{ item.activity }}</p>
            </div>
            <div class="flex-shrink-0 pr-2">
              <button
                type="button"
                class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-white bg-transparent text-gray-400 hover:text-gray-500"
                @click="handlePopOver(item.id)"
              >
                <span class="sr-only">Open options</span>
                <EllipsisVerticalIcon class="h-5 w-5" aria-hidden="true" />
              </button>
            </div>
          </div>
          <transition
            enter-active-class="transition duration-200 ease-out"
            enter-from-class="translate-y-1 opacity-0"
            enter-to-class="translate-y-0 opacity-100"
            leave-active-class="transition duration-150 ease-in"
            leave-from-class="translate-y-0 opacity-100"
            leave-to-class="translate-y-1 opacity-0"
          >
            <div
              class="absolute z-30 popover bg-white right-5 top-10 px-3 py-2 rounded shadow-md"
              v-if="isShow && activePopOver === item.id"
            >
              <ul>
                <li v-for="item in menu">{{ item.menu }}</li>
              </ul>
            </div>
          </transition>
        </li>
      </ul>

      <ul
        role="list"
        class="mt-3 grid grid-cols-1 gap-5 sm:gap-6 sm:grid-cols-2 bg-stone-100 px-5 py-3 rounded-md"
        @dragover.prevent
  @dragenter.prevent @drop="drop($event, 1)">
      <h2 class="text-sm font-medium text-gray-500 col-span-2" v-if="todos2.length">Todo List Done</h2>
      <div class="box" v-if="todos2.length">
      <Item v-for="item in todos2" :activity="item.activity" :color="item.color" />
      </div>
        <div class="h3 col-span-2 text-center text-3xl my-auto" v-else>
          Drag n Drop list disini
        </div>
      </ul>
     </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { ArrowLongDownIcon, EllipsisVerticalIcon } from "@heroicons/vue/20/solid";
import {useForm} from "vee-validate"
import BaseButton from "./components/BaseButton.vue";
import BaseModal from "./components/BaseModal.vue"
import BaseInput from "./components/BaseInput.vue";
import Item from "./components/Item.vue";
import * as yup from "yup"
const openModal = ref(false)
const todos2 = ref([]);

function dragItem(ev, item) {
  var data = (ev.dataTransfer.dropEffect = "move");
  var data2 = (ev.dataTransfer.effectAllowed = "move");
  var data3 = ev.dataTransfer.setData("itemId", item.id);
}

function drop(event) {
  const id = event.dataTransfer.getData("itemId");
  const result = todos.find((item) => item.id == id);
  console.log(result);
  todos2.value.push(result);
}

const todos = [
  { id: 1, activity: "makar", isDone: false },
  { id: 2, activity: "demo kraton", isDone: false },
  { id: 3, activity: "menjadi gubernur sintang", isDone: false },
  { id: 1, activity: "makar", isDone: false },
  { id: 2, activity: "demo kraton", isDone: false },
  { id: 3, activity: "menjadi gubernur sintang", isDone: false },
  { id: 1, activity: "makar", isDone: false },
  { id: 2, activity: "demo kraton", isDone: false },
  { id: 3, activity: "menjadi gubernur sintang", isDone: false },
];

const isShow = ref(false);
const activePopOver = ref(null);
function handlePopOver(id) {
  if (isShow.value === false) {
    activePopOver.value = id;
    isShow.value = true;
  } else {
    activePopOver.value = null;
    isShow.value = false;
  }
}

const menu = [
  {
    id: 1,
    menu: "edit",
  },
  {
    id: 2,
    menu: "delete",
  },
];

const validationrule = yup.object({
  activity: yup.string().min(4).required(),
  color: yup.string().min(2).required()
})

const {defineComponentBinds, defineInputBinds,handleSubmit, handleReset} = useForm({
  validationSchema: validationrule
});

const handleModal = (()=>{
  openModal.value = !openModal.value
  handleReset()
})

const activity = defineComponentBinds('activity');
const color = defineInputBinds('color')

const onSubmit = handleSubmit(values=>{
const formData = {
  todo: activity.value.modelValue,
  bgColor: color.value.value
}
handleReset()

})

function popover(d) {
  console.log(d);
}
</script>

<style lang="scss" scoped></style>
