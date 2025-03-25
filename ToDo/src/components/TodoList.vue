<script setup>
import { ref,onMounted } from 'vue'

const editingIndex = ref(-1);
const NewItem = ref('')
const ItemList=  ref([])
const STORAGE_KEY = 'vue-todolist-items';
onMounted(() => {
  loadTodos();
});

const AddItem =()=>{
  if (NewItem.value.trim()=='' || ItemList.value.length>=10) {
      return; 
    }
  ItemList.value.push({ id: ItemList.value.length , text: NewItem.value })
  console.log(ItemList)
  NewItem.value = ''
  saveTodos()
  window.location.reload();
}

const EditItem=(item ,id)=> {

  editingIndex.value = id;
}

const SaveItem=(item,id)=> {
  editingIndex.value = id;
}
const cancelEdit = () => {
  editingIndex.value = -1;
  saveTodos()
};

const removeItem=(item)=> {
  // ItemList.value = ItemList.value.filter((t) => t !== item)
  ItemList.value.splice(ItemList.value.indexOf(item), 1)
  console.log(ItemList)
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
    <div class="card px-6 shadow-md rounded-md bg-white-50">
      <form class="m-1 p-1" @submit.prevent="AddItem">
        <input class="mr-5 p-2 border-2 border-stone-200 rounded-md"  v-model="NewItem" placeholder="待辦事項">
        <button class="m-1 p-2 bg-indigo-500 hover:bg-stone-300 text-neutral-100 text-lg " type="button" @click="AddItem">新增</button>
      </form>
      <ul> 
        <li class="shadow- rounded-m flex items-center m-1 px-5 rounded-md"  v-for="(item, id) in ItemList" :key="item.id">{{item.text}}
          <div  class="m-1 p-2" v-if="editingIndex == id">
            <input class="m-1 p-2 transition delay-1500  border-4 border-indigo-500 rounded-md" v-model=item.text>
            <button class="m-1 p-2 bg-cyan-500 text-neutral-100" @click="EditItem()">儲存</button>
            <button class="m-1 p-2 bg-pink-500 text-neutral-100" @click="cancelEdit()">取消</button>
          </div>
          <div class=" ml-30" v-else>
            <button class="m-2 p-3 bg-cyan-500 text-neutral-100 font-bold" @click="SaveItem(item,id)">編輯</button>
            <button class="m-2 p-3 bg-pink-500 text-neutral-100 font-bold" @click="removeItem(item)">刪除</button>
          </div> 
        </li>
      </ul>  
    </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>


