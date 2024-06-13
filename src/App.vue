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


// TODO: 编辑功能

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
        <template #default>
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
