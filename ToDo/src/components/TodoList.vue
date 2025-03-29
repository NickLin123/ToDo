<script setup>
import { ref, onMounted, computed ,watch} from 'vue'
import Input from './Input.vue';
import ShowList from './showList.vue';


// const editingIndex = ref(-1);
// const NewItem = ref('')
const ItemList=  ref([])
// const hideDone = ref(false)
const STORAGE_KEY = 'vue-todolist-items';

onMounted(() => {
  // saveTodos()
  loadTodos();
});

//取得localstorage的資料
const loadTodos = () => {
  const savedTodos = localStorage.getItem(STORAGE_KEY);
  // console.log(savedTodos)
  if (savedTodos) {
    ItemList.value = JSON.parse(savedTodos);
  }
  console.log(savedTodos)
  console.log(ItemList)
};




// const AddItem =()=>{
//   if (NewItem.value.trim()=='' || ItemList.value.length>=10) {
//       return; 
//     }
//   ItemList.value.push({ id: ItemList.value.length , text: NewItem.value, done:false })
//   console.log(ItemList)
//   NewItem.value = ''
//   saveTodos()
//   window.location.reload();
// }

// const EditItem=(item ,id)=> {
//   editingIndex.value = id;
//   saveTodos()
// }

// const SaveItem=(item,id)=> {
//   editingIndex.value = id;
  
// }
// const cancelEdit = () => {
//   editingIndex.value = -1;
  
// };

// const removeItem=(item)=> {
//   ItemList.value.splice(ItemList.value.indexOf(item), 1) //刪除對應id項目
//   console.log(ItemList)
//   saveTodos()
// }


// const eventDone = computed(() => {
//   return hideDone.value
//     ? ItemList.value.filter((item) => !item.done)
//     : ItemList.value
// })


// const saveTodos = () => {
//   localStorage.setItem(STORAGE_KEY, JSON.stringify(ItemList.value));
// };
// watch(ItemList)
</script>

<template>
    <div class="card px-6 shadow-md rounded-md bg-white-50">
      <Input  @OnAdd="(Item) => ItemList = Item" />
        <!-- <p>{{ItemList}}</p> -->
      <ShowList :ItemList="ItemList" />
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