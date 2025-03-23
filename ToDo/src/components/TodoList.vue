<script setup>
import { ref,onMounted } from 'vue'

let id =0;
const NewItem = ref('')
const ItemList=  ref([])
const STORAGE_KEY = 'vue-todolist-items';
onMounted(() => {
  loadTodos();
});

const AddItem =()=>{
  ItemList.value.push({ id: id++, text: NewItem.value })
  NewItem.value = ''
  saveTodos()
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
    <div class="card">
      <form @submit.prevent="AddItem">
        <input v-model="NewItem" placeholder="Type here">
        <button type="button" @click="AddItem">新增</button>
      </form>
      <ul>
          <li v-for="item in ItemList" :key="item.id">{{ item.text }}
          <button @click="removeItem(item)">刪除</button>
        </li>
      </ul>  
    </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>


