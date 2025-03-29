<script setup>
import { ref } from 'vue'
const emit = defineEmits([])
const ItemList=  ref([])
const NewItem = ref('')
const STORAGE_KEY = 'vue-todolist-items';
const props = defineProps({
  ItemList: String
})
emit('OnAdd', ItemList)


const AddItem =()=>{
  if (NewItem.value.trim()=='' || ItemList.value.length>=10) {
      return; 
    }
  ItemList.value.push({ id: ItemList.value.length , text: NewItem.value, done:false })
  // console.log(ItemList)
  NewItem.value = ''
  saveTodos()
  loadTodos ()
  console.log(ItemList)
  // window.location.reload();
}

const saveTodos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(ItemList.value));
};

const loadTodos = () => {
  const savedTodos = localStorage.getItem(STORAGE_KEY);
  console.log(savedTodos)
  if (savedTodos) {
    ItemList.value = JSON.parse(savedTodos);
  }
};



</script>

<template>
     <form class="m-1 p-1" @submit.prevent="AddItem">
        <input class="mr-5 p-2 border-2 border-stone-200 rounded-md"  v-model="NewItem" placeholder="待辦事項">
        <button class="m-1 p-2 bg-indigo-500 hover:bg-stone-300 text-neutral-100 text-lg " type="button" @click="AddItem">新增</button>
      </form>
</template>