<script setup>
import { ref } from 'vue'
const emit = defineEmits(['add-todo'])
const ItemList=  ref([])
const NewItem = ref('')
const props = defineProps({
  ItemList: {
    type: Array,
  },
  maxTodos: {
    type: Number,
    default: 10
  }
});


const handleAddItem = () => {
  if (!NewItem.value.trim() || props.ItemList.length >= props.maxTodos) {
    return;
  }
  emit('add-todo', text=NewItem.value);
  NewItem.value = '';
};


</script>

<template>
     <form class="m-1 p-1">
        <input class="mr-5 p-2 border-2 border-stone-200 rounded-md"  v-model="NewItem" placeholder="新增待辦事項..." >
        <button class="m-1 p-2 bg-indigo-500 hover:bg-stone-300 text-neutral-100 text-lg " type="button" @click="handleAddItem">新增</button>
      </form>
      <div v-if="ItemList.length >= maxTodos">
      已達到最大待辦事項數量限制 ({{ maxTodos }}項)
      </div>
      <div>
      待辦事項數量: {{ ItemList.length }}/{{ maxTodos }}
    </div>
</template>