<script setup>
import { ref, onMounted, computed ,watch, Text} from 'vue'
import Input from './Input.vue';
import ShowList from './showList.vue';


// const editingIndex = ref(-1);
// const NewItem = ref('')

//清單及清單最大數量
const ItemList=  ref([])
const maxTodos = 10;
// const hideDone = ref(false)
const STORAGE_KEY = 'vue-todolist-items';

onMounted(() => {
  // saveTodos()
  loadTodos();
});

//取得localstorage的資料
const loadTodos = () => {
  const savedTodos = localStorage.getItem(STORAGE_KEY);
  if (savedTodos) {
    ItemList.value = JSON.parse(savedTodos);
  }
  console.log(savedTodos)
  console.log(ItemList)
};

// 儲存待辦事項到 localStorage
const saveTodos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(ItemList.value));
};


const AddItem =()=>{
  if (ItemList.value.length < maxTodos) {    
  ItemList.value.push({ id: ItemList.value.length , text:text, done:false })
  console.log(text)
  // NewItem.value = ''
  saveTodos()
  window.location.reload();
  }
}

const EditItem=(item ,id)=> {
  editingIndex.value = id;
  saveTodos()
}

// const SaveItem=(item,id)=> {
//   editingIndex.value = id;
  
// }
// const cancelEdit = () => {
//   editingIndex.value = -1;
  
// };

const removeItem=(item)=> {
  ItemList.value.splice(ItemList.value.indexOf(item), 1) //刪除對應id項目
  console.log(ItemList)
  saveTodos()
}


// const eventDone = computed(() => {
//   return hideDone.value
//     ? ItemList.value.filter((item) => !item.done)
//     : ItemList.value
// })

watch(ItemList, ()=>{
  saveTodos();
})

onMounted(() => {
  loadTodos();
});
</script>

<template>
    <div class="card px-6 shadow-md rounded-md bg-white-50">
      <h1>ToDoList</h1>
      <Input   :ItemList="ItemList"
      :maxTodos="maxTodos"
      @add-todo="AddItem" />
        <!-- <p>{{ItemList}}</p> -->
      <ShowList       :ItemList="ItemList"
      @update-todo="EditItem"
      @remove-todo="removeItem" />
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