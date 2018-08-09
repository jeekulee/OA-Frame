<template>

  <div id="app-main">

      <!-- <router-view></router-view> -->
  <el-container>
    <el-aside :width="collapse ? '' : '230px'">
      <sidebar :menus="menus" :collapse="collapse"></sidebar>
    </el-aside>
    <el-container>
      <el-header height='55px'>
        <span class="header_left" @click="setMenus">
          <i class="fa fa-bars"></i>
        </span>
        
        <span class="header_right">
          <el-popover
            placement="bottom"
            trigger="hover">
            <ul class="headUl">
              <li>
                <a href="#" @click="logout()"><i class="fa fa-sign-out fa-lg mr4"></i>退出登录</a>
              </li>
            </ul>
            
            <el-button slot="reference" icon="el-icon-setting">{{user.name}}<i class="el-icon-caret-bottom"></i></el-button>
            
          </el-popover>
          
        </span>
      </el-header>
      <el-main class="side-content"  v-bind:style="styleObject">
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
  </div>

</template>

<script>
import sidebar from '@/components/sidebar'

export default {
  data () {
    return {
      styleObject: {
        'min-height': window.innerHeight - 56 + 'px',
        'min-width': '1024px'
      },
      menus: [
        {text: '图表',
          submenus: [
            {text: '最新出车情况', path: '/home/condition', icon: 'fa-truck'},
            {text: '月车辆利用率、耗油情况', path: '/home/test2', icon: 'fa-line-chart'}
          ]
        },
        {text: '最新', path: '/home/test', icon: 'fa-truck'},
        {text: '月车', path: '/home/test1', icon: 'fa-line-chart'},
        {text: '洒水车、扫路车作业速度', path: '/home/test4', icon: 'fa-dashboard'}
      ],
      collapse: false
    }
  },
  methods: {
    setMenus () {
      this.collapse = !this.collapse
    },
    hh () {
      this.styleObject['min-height'] = window.innerHeight - 56 + 'px'
    },
    logout () {
      this.$router.push('/login')
    }
  },
  created () {
    this.hh()
  },
  mounted () {
    let that = this
    window.onresize = function windowResize () {
      that.hh()
    }
  },
  computed: {
    user () {
      return this.$store.state.user.info
    }
  },
  components: {sidebar}
}
</script>

