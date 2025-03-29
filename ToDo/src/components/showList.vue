<script setup>
// import {onMounted} from 'vue'
//主元件傳ItemList
const props = defineProps({
  ItemList: String
})

// const editingIndex = ref(-1);

const EditItem=(item ,id)=> {
  editingIndex.value = id;
  saveTodos()
}

const SaveItem=(item,id)=> {
  editingIndex.value = id;
  
}
const cancelEdit = () => {
  editingIndex.value = -1;
  
};

const removeItem=(item)=> {
  ItemList.value.splice(ItemList.value.indexOf(item), 1) //刪除對應id項目
  console.log(ItemList)
  saveTodos()
}

// watch(ItemList)

</script>

<template>
  <ul> 
        <li class="shadow- rounded-m flex items-center m-1 px-5 rounded-md"  v-for="(item, id) in ItemList" :key="item.id">
          <input type="checkbox" v-model="item.done">
          <span>{{item.text}} {{item.done}}</span>
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
</template>