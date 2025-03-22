<script setup>
import { ref } from 'vue'

let id =0;

const NewItem = ref('')
const ItemList=  ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])
const AddItem =()=>{
  ItemList.push(Item.id,Item.text)
}
</script>

<template>
  <!-- <h1>{{ msg }}</h1> -->
    <div class="card">
      <input v-model="NewItem" placeholder="Type here">
      <button type="button" @click="AddItem">新增</button>
      <div v-for=" Item in ItemList">{{ Item.text }}</div>
    </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
