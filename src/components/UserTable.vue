<script lang="ts" setup>
import {reactive, ref} from "vue";
import {Search} from '@element-plus/icons-vue'
import userApi from "../api/user";
interface User {
  id: string
  name: string
}

let timer: any;
const userData = ref<User[]>([]);
const keyword = ref('');

interface userParams {
  name: string;
  orgId: string;
}

const getUserParams = reactive<userParams>({
  name: "",
  orgId: "",
});

const getUserList = async (key: keyof userParams, data: string) => {
  //根据key获取对应参数
  getUserParams[key] = data;
  const result: User[] = await userApi.query(getUserParams);
  userData.value = result;
};

// 防抖
const debounce = (fn: Function, time: number = 300) => {
  //默认300毫秒
  if (timer) clearTimeout(timer)
  timer = setTimeout(function () {
    fn()
  }, time)
}

let handleInput = () => {
  debounce(() => {
    console.log('防抖执行了')
    getUserList("name", keyword.value);
  }, 2000)
};

defineExpose({
  getUserList,
});
</script>

<template>
  <div class="tableBox">
    <div class="search">
      <el-input
          v-model="keyword"
          style="width: 200px"
          size="large"
          :suffix-icon="Search"
          placeholder="搜索"
          @input="handleInput"
      />
    </div>
    <el-table :data="userData" border style="width: 100%">
      <el-table-column prop="id" label="姓名"/>
      <el-table-column prop="name" label="用户名"/>
    </el-table>
  </div>
</template>

<style scoped>
.tableBox {
  flex: 1;
}
.search {
  width: 100%;
  height: 50px;
  border-bottom: 1px solid #eee;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding-left: 20px;
}
</style>
