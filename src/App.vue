<script setup>
import Edit from './components/Edit.vue'
// 載入各個需要的方法
import { onMounted, ref } from 'vue'
import axios from'axios'

// TODO: 列表渲染
// 思路：聲明響應式list -> 調用接口獲取資料 -> 後端數據賦值給list -> 綁定到table組件

const list = ref([])
const getList = async() => {
  // 接口調用
  const res = await axios.get('/list')
  // 交給list
  list.value = res.data
}
onMounted(() => getList())

// TODO: 删除功能
// 思路：獲取當前行的id -> 通過id調用刪除接口 -> 更新最新的列表

const onDelete = async (id) => {
  console.log(id)
  axios.delete(`/del/${id}`)
  getList()
}
// TODO: 编辑功能
// 思路：打開彈框 -> 回填資料 -> 更新資料

// 1. 打開彈框(獲取子組件實例 調用方法或修改屬性[連接到template中的</Ediit>])
// 2. 回調資料(調用詳情接口 / 當前行的靜態數據)
const editRef = ref(null)
const onEdit = (row) => {
  editRef.value.open(row)
}

</script>

<template>
  <div class="app">
    <!-- 靜態資料
    [{
      id: 1,
      name: 'jack',
      place: 'none'
    }] -->
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @on-update="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
