<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, reactive } from 'vue'
const tableData = ref([
  {
    id: 1,
    kemu: "语文",
    cont: "唐诗",
    addr: "宿舍",
    status: true
  },
  {
    id: 2,
    kemu: "数学",
    cont: "一元二次方程",
    addr: "自习室",
    status: false
  },
  {
    id: 3,
    kemu: "英语",
    cont: "语法",
    addr: "宿舍",
    status: false
  },
]);

const deleteRow = (index) => {
  console.log(index)
  //tableData.value.splice(index, 1)
  tableData.value = tableData.value.filter(item => item.id != index)
}

const form = reactive({
  id:0,
  kemu: "",
  cont: "",
  addr: "",
})

const options = [
  {
    value: '教室',
    label: '教室',
  },
  {
    value: '宿舍',
    label: '宿舍',
  },
  {
    value: '阳台',
    label: '阳台',
  },
]

//获取数组对象tableData中的最大id值
function getMaxId() {    
  let maxId = 0;  
  for (let i = 0; i < tableData.value.length; i++) {  
    if (tableData.value[i].id > maxId) {  
      maxId = tableData.value[i].id;  
    }  
  }  
  return maxId;  
}  

const addRow = ()=>{
  //nextId = getMaxId()+1 //已弃用，改用 扩展运算符+map方法+Math.max
  let nextId = Math.max(...tableData.value.map(item=>item.id)) + 1
  form.id = nextId;
  // 将新数据添加到tableData中  
  tableData.value.push(form);  
}
</script>

<template>
  <el-card style="max-width: 100%">
    <template #header>
      <div class="card-header">
        <span>学习计划表</span>
      </div>
    </template>
    <el-form :model="form" label-width="auto">
      <el-form-item label="学习科目">
        <el-input v-model="form.kemu" />
      </el-form-item>
      <el-form-item label="学习内容">
        <el-input v-model="form.cont" />
      </el-form-item>
      <el-form-item label="学习地点">
        <el-select v-model="form.addr" placeholder="请选择学习地点" style="width: 240px">
          <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
      <el-button type="primary" @click="addRow()">添加</el-button>
    </el-form>

  </el-card>

  <el-table :data="tableData" stripe style="width: 100%">
    <el-table-column prop="id" label="序号" width="180" />
    <el-table-column prop="kemu" label="学习科目" width="180" />
    <el-table-column prop="cont" label="学习内容" width="280" />
    <el-table-column prop="addr" label="学习地点" width="180" />
    <el-table-column label="完成状态">
      <template #default="scope">
        <el-switch v-model="scope.row.status" />
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template #default="scope">
        <el-button link type="primary" size="small" @click.prevent="deleteRow(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<style lang="less" scoped>
div.card-header {
  background: #ccc;

  span {
    display: inline-block;
    font-size: 26px;
    line-height: 30px;
    text-align: left;
    width: 100%;
  }


}
</style>
