<script setup lang="ts">
import router from "@/router";
import {routes} from "@/router";
const handleSelect = (index: string) => {
  router.push(index)
}
</script>

<template>
  <div class="layout-wrapper">
    <div class="layout-sidebar">
      <el-scrollbar>
        <el-menu
          @select="handleSelect"
        >
          <template v-for="item in routes" :key="item.path">
            <el-sub-menu
              v-if="item.children && item.children.length > 0"
              :index="item.path"
            >
              <template #title>{{ item.meta?.title }}</template>
              <el-menu-item
                v-for="child in item.children"
                :key="child.path"
                :index="item.path+'/' + child.path"
              >
                {{ child.meta?.title }}
              </el-menu-item>
            </el-sub-menu>
            <el-menu-item
              v-else
              :index="item.path"
            >
              {{ item.meta?.title }}
            </el-menu-item>
          </template>
        </el-menu>

<!--        <el-menu-->
<!--          @select="handleSelect"-->
<!--        >-->
<!--          <el-menu-item-->
<!--            v-for="item in routes"-->
<!--            :key="item.path"-->
<!--            :index="item.path"-->
<!--            >-->
<!--            {{ item.name }}-->
<!--          </el-menu-item>-->
<!--        </el-menu>-->
      </el-scrollbar>
    </div>
    <div class="layout-main">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped>
.layout-wrapper{
  width: 100%;
  height: 100%;
  .layout-sidebar{
    position: fixed;
    top: 0;
    bottom: 0;
    z-index: 999;
    width: 15%;
    :deep(.el-sub-menu__title){
      font-size: 20px;
      font-weight: bold;
      padding: 0 10%;
    }
    :deep(.el-menu-item){
      font-size: 15px;
      padding: 0 15%;
    }
  }
  .layout-main{
    margin-left: 15%;
    z-index: 999;
    background-color: burlywood;
  }
}


</style>
