<script setup>
import {ref,computed,onMounted} from 'vue'
//主元件傳ItemList
const props = defineProps({
  eventDone: {
    type: Array,
  }
})
// 定義 emits
const emit = defineEmits(['update-todo', 'remove-todo']);

// 編輯相關的狀態
const editingIndex = ref(-1);

const EditItem=(item ,id)=> {
  console.log(item)
  emit('update-todo', item,id);
}

const SaveItem=(item,id)=> {
  editingIndex.value = id;
}
const cancelEdit = () => {
  editingIndex.value = -1;
};

const removeItem=(item,id)=> {
  emit('remove-todo', item,id);
}

</script>

<template>
  <ul> 
        <li class="flex items-center m-1 px-5 rounded-md"  v-for="(item, id) in eventDone" :key="item.id">
          <input  :checked="item.done" type="checkbox" v-model="item.done">
          <span class="text-gray-500">{{item.text}}</span>
          <div  class="m-1 p-2" v-if="editingIndex == id">
            <input class="m-1 p-2 transition delay-1500  border-4 border-indigo-500 rounded-md" v-model=item.text>
            <button class="m-1 p-2 bg-cyan-500 text-neutral-100" @click="EditItem(item,id)">儲存</button>
            <button class="m-1 p-2 bg-pink-500 text-neutral-100" @click="cancelEdit()">取消</button>
          </div>
          <div class=" ml-30" v-else>
            <button class="m-2 p-3 bg-cyan-500 text-neutral-100 font-bold" @click="SaveItem(item,id)">編輯</button>
            <button class="m-2 p-3 bg-pink-500 text-neutral-100 font-bold" @click="removeItem(item)">刪除</button>
          </div> 
        </li>
      </ul>  
</template>