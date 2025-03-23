<script setup>
import { ref,onMounted } from 'vue'

let id =0;
// let Edit = true;
const NewItem = ref('')
const ItemList=  ref([])
const STORAGE_KEY = 'vue-todolist-items';
onMounted(() => {
  loadTodos();
});


const AddItem =()=>{
  if (NewItem.value.trim()=='' || ItemList.value.length>=10) {
      return; // 
    }
  ItemList.value.push({ id: id++, text: NewItem.value })
  NewItem.value = ''
  saveTodos()
}
const EditItem=(Edit)=> {
  Edit !== Edit
  console.log(Edit)
  // saveTodos()
}
const removeItem=(item)=> {
  ItemList.value = ItemList.value.filter((t) => t !== item)
  saveTodos()
}
const loadTodos = () => {
  const savedTodos = localStorage.getItem(STORAGE_KEY);
  if (savedTodos) {
    ItemList.value = JSON.parse(savedTodos);
  }
};
const saveTodos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(ItemList.value));
};

</script>

<template>
    <div class="card m-4 p-3 shadow-md rounded-md">
      <form class="m-1 p-1" @submit.prevent="AddItem">
        <input class="m-4 p-3 border-4 border-indigo-500 rounded-md"  v-model="NewItem" placeholder="待辦事項">
        <button class="m-2 p-3 bg-blue-600 text-neutral-100" type="button" @click="AddItem">新增</button>
      </form>
      <ul>
          <li class="m-1 p-1" v-for="item in ItemList" :key="item.id">{{ item.text }}
          <button class="m-2 p-3 bg-blue-400 text-neutral-100" @click="EditItemItem(item)">編輯</button>
          <button class="m-2 p-3 bg-pink-300 text-neutral-100" @click="removeItem(item)">刪除</button>
        </li>
      </ul>  
    </div>

    <button class="m-2 p-3 bg-blue-400 text-neutral-100" @click="EditItem(Edit)">編輯</button>
    <div v-if="Edit==true">
         Now you see me
    </div>
    <div v-else>
      Now you don't
    </div>
  
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>


