<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import router from "@/router";

const route = useRoute()
const isCollapsed = ref(false)

// 菜单配置
const menuItems = [
  { index: '1', title: '仪表盘', route: '/dashboard', icon: 'Odometer' },
  { index: '2', title: '关于', route: '/about', icon: 'InfoFilled' },
  { index: '3', title: '用户', route: '/user', icon: 'User' },
  { index: '4', title: '404页面', route: '/404', icon: 'Warning' }
]

// 当前激活菜单
const activeIndex = computed(() => {
  const menuItem = menuItems.find(item => item.route === route.path)
  return menuItem ? menuItem.index : '1'
})

const toggleCollapse = () => {
  isCollapsed.value = !isCollapsed.value
}

const handleSelect = (index: string) => {
  const menuItem = menuItems.find(item => item.index === index)
  if (menuItem?.route) {
    router.push(menuItem.route)
  }
}
</script>

<template>
  <div class="layout-wrapper">
    <!-- 侧边栏 -->
    <aside class="layout-sidebar" :class="{ 'collapsed': isCollapsed }">
      <div class="sidebar-header">
        <h3 v-if="!isCollapsed">导航菜单</h3>
        <el-button
          @click="toggleCollapse"
          :icon="isCollapsed ? 'Expand' : 'Fold'"
          circle
          size="small"
          class="collapse-btn"
        />
      </div>

      <el-menu
        :default-active="activeIndex"
        :collapse="isCollapsed"
        @select="handleSelect"
        class="sidebar-menu">

        <el-menu-item
          v-for="item in menuItems"
          :key="item.index"
          :index="item.index"
          :route="item.route">
          <el-icon>
            <component :is="item.icon" />
          </el-icon>
          <template #title>{{ item.title }}</template>
        </el-menu-item>
      </el-menu>
    </aside>

    <!-- 主内容区 -->
    <main class="layout-main" :style="{ marginLeft: isCollapsed ? '64px' : '220px' }">
      <div class="main-content">
        <router-view v-slot="{ Component }">
          <transition name="fade" mode="out-in">
            <component :is="Component" />
          </transition>
        </router-view>
      </div>
    </main>
  </div>
</template>

<style scoped>
.layout-wrapper {
  display: flex;
  min-height: 100vh;
}

.layout-sidebar {
  width: 220px;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  background: white;
  box-shadow: 2px 0 12px rgba(0, 0, 0, 0.05);
  transition: all 0.3s cubic-bezier(0.2, 0, 0, 1);
  z-index: 1000;
}

.layout-sidebar.collapsed {
  width: 64px;
}

.sidebar-header {
  padding: 20px 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #f0f0f0;
}

.sidebar-header h3 {
  margin: 0;
  font-size: 18px;
  color: #303133;
  font-weight: 600;
  white-space: nowrap;
}

.collapse-btn {
  flex-shrink: 0;
}

.sidebar-menu {
  border-right: none;
  height: calc(100vh - 80px);
  overflow-y: auto;
}

.sidebar-menu:not(.el-menu--collapse) {
  width: 100%;
}

.el-menu-item {
  height: 48px;
  line-height: 48px;
  margin: 4px 8px;
  border-radius: 8px;
  transition: all 0.3s;
}

.el-menu-item:hover {
  background-color: #f5f7fa;
}

.el-menu-item.is-active {
  background-color: #ecf5ff;
  color: #409eff;
}

.layout-main {
  flex: 1;
  margin-left: 220px;
  transition: margin-left 0.3s cubic-bezier(0.2, 0, 0, 1);
  min-height: 100vh;
  background: #f8f9fa;
}

.main-content {
  max-width: 1200px;
  //margin: 0 auto;
}

/* 路由过渡动画 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
