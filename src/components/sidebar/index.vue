<template>
  <div id="sidebar">
    <el-menu :default-active="curIndex" :router="true" :collapse="collapse">
      <div v-for="menu in menus" :key="menu.path || menu.text">
        <div v-if="menu.submenus && menu.submenus.length > 0">
          <el-submenu 
            :index="menu.path || menu.text">

            <template slot="title">
              <i class="fa mr10" :class="menu.icon || 'el-icon-menu'"></i>
              <span slot="title">{{ menu.text }}</span>
            </template>
            
            <el-menu-item 
              v-for="submenu in menu.submenus" 
              v-if="menu.submenus.length > 0" 
              :index="submenu.path" 
              :key="submenu.path">
              <i class="fa mr10" :class="submenu.icon"></i>
              <span slot="title">{{ submenu.text }}</span>
            </el-menu-item>

          </el-submenu>
        </div>
        <div v-else>
          <el-menu-item 
            :index="menu.path" 
            :key="menu.path">
            <i class="fa mr10" :class="menu.icon"></i>
            <span slot="title">{{ menu.text }}</span>
          </el-menu-item>
        </div>
      </div>
    </el-menu>
  </div>
</template>

<script>
export default {
  name: 'sidebar',
  props: {
    menus: {require: true, default: []},
    collapse: {require: true, default: false}
  },
  computed: {
    curIndex () {
      return this.$route.path
    }
  }
}
</script>
