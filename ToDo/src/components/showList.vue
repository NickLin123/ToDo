<script setup>
import {ref} from 'vue'
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




const timeFormat = (id) => {
  let now = new Date(id);
  let year = now.getFullYear();
  let month = String(now.getMonth() + 1).padStart(2, '0'); 
  let day = String(now.getDate()).padStart(2, '0');
  let hours = String(now.getHours()).padStart(2, '0');
  let minutes = String(now.getMinutes()).padStart(2, '0');

    return `${year}年${month}月${day}日${hours}點${minutes}分`;
};

const compeledTime = () => {
  let now = new Date();
  let year = now.getFullYear();
  let month = String(now.getMonth() + 1).padStart(2, '0');
  let day = String(now.getDate()).padStart(2, '0');
  let hours = String(now.getHours()).padStart(2, '0');
  let minutes = String(now.getMinutes()).padStart(2, '0');
    return `${year}年${month}月${day}日${hours}點${minutes}分`
};

const EditItem=(item ,id)=> {
  emit('update-todo', item,id);
  editingIndex.value = -1;
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
        <li class="flex items-center m-1 px-5 rounded-md shadow-md"   v-for="(item, id) in eventDone" :key="item.id">
          <input class="mr-2" :checked="item.done" type="checkbox" v-model="item.done" @click="compeledTime()">
          <span class="mx-5 text-gray-600 text-2xl">{{item.text}}</span>
          <span v-if="item.done" class="mx-5 text-gray-500 text-sm ">完成時間 : {{compeledTime()}}</span>
          <span v-else class="mx-5 text-gray-400 text-sm">登錄時間 : {{timeFormat(item.id)}} </span>
          
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