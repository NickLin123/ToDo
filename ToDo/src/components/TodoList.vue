<script setup>
import { ref, onMounted, computed ,watch} from 'vue'
import Input from './Input.vue';
import ShowList from './showList.vue';


const editingIndex = ref(-1);
const Message = ref('')
const ItemList=  ref([])
const maxTodos = 10;
const hideDone = ref(false)
const STORAGE_KEY = 'vue-todolist-items';

onMounted(() => {
  loadTodos();
});

//取得localstorage的資料
const loadTodos = () => {
  const savedTodos = localStorage.getItem(STORAGE_KEY);
  if (savedTodos) {
    ItemList.value = JSON.parse(savedTodos);
  }
  // console.log(savedTodos)
  console.log(ItemList)
};

// 儲存待辦事項到 localStorage
const saveTodos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(ItemList.value));
};


const AddItem =(text)=>{  
  ItemList.value.push({ id: ItemList.value.length , text:text, done:false })
  console.log(text)
  // NewItem.value = ''
  Message.value='新增成功'
  saveTodos()
  window.location.reload()
}

const EditItem=(item ,id)=> {
  editingIndex.value = id;
  Message.value='編輯成功'
  saveTodos()
  window.location.reload()
}

const removeItem=(item)=> {
  ItemList.value.splice(ItemList.value.indexOf(item), 1) //刪除對應id項目
  console.log(ItemList)
  Message.value='刪除成功'
  saveTodos()

}

const filteredItems = computed(() => {
  if(hideDone.value==true){
    return ItemList.value.filter(item => item.done)
  }else{
    return ItemList.value
  }
})

watch(ItemList, ()=>{
  saveTodos();
})
</script>

<template>
    <div class="card px-6 shadow-md rounded-md flex flex-col bg-black-100 justify-center sm:py-12">
      <h1 class="text-3xl font-extrabold text-gray-500 mb-6">ToDoList</h1>
       <p class="px-3 py-1 rounded-md text-gray-500 cursor-pointer" @click="hideDone = !hideDone">
        {{ hideDone ? '顯示全部待辦清單' : '顯示已完成待辦清單' }}</p>
      <Input   :ItemList="ItemList"
      :maxTodos="maxTodos"
      @add-todo="AddItem" />
     
      <ShowList   :eventDone="filteredItems"
      @update-todo="EditItem"
      @remove-todo="removeItem"
      />
      <div v-if="Message" class="mt-1 text-sm text-green-600">{{ Message }}</div>
    </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>


//1.篩選 可以選出已完成/未完成項目 (延伸項目 可不做: 關鍵字搜尋)
2.把組件拆開來 看你要怎麼拆
3.加入一些訊息提示回饋 新增成功 新增失敗 上限提醒
4.顯示目前筆數...之類的其他優化
前三樣必做