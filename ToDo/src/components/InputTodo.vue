<script setup>
import { ref,onMounted,watch } from 'vue'
const emit = defineEmits(['add-todo'])
const NewItem = ref('')
const error = ref('')
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
  if (!NewItem.value.trim()) {
    error.value = '待辦事項不能為空'
    return 
  }
    // 重複檢查
    if (props.ItemList.some(item => item.text === NewItem.value.trim())) {
    error.value = '待辦事項已存在'
    return 
  }
  
  // 數量限制檢查
  if (props.ItemList.length >= 10) {
    error.value = '待辦事項最多只能有10個'
    return
  }
  if (NewItem.value.length >= 10) {
    error.value = '待辦事項字數最多只能10個字'
    return
  }
  emit('add-todo', NewItem);
  NewItem.value = '';
};
watch(NewItem, () => {
  if(NewItem.value.length==10){
    alert("待辦事項最多十個字")
    NewItem.value = ''
  }
  if (error.value) {
    error.value = ''
  }
})

</script>

<template>
     <form class="m-1 p-1">
        <input @keyup.enter="handleAddItem" class="mr-5 p-2 border-2 border-stone-100 rounded-md" maxlength="10" v-model="NewItem" placeholder="新增待辦事項..." >
        <button class="m-1 p-2 bg-indigo-500 hover:bg-indigo-700 text-neutral-100 text-lg " type="button" @click="handleAddItem">新增</button>
      </form>
      <div v-if="error" class="mt-1 text-sm text-red-600">{{ error }}</div>
      <div v-if="ItemList.length >= maxTodos">
      已達到最大待辦事項數量限制 ({{ maxTodos }}項)
      </div>
      <div class="mx-2 flex justify-end">
      <span class="text-pink-600">待辦事項數量: {{ ItemList.length }}/{{ maxTodos }}</span>

    </div>
</template>