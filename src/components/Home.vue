<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/heima.png" alt="logo">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 左边侧边栏 -->
      <el-aside :width="isCollapse ? '64px':'200px'" >
        <div class="toggle-button" @click="toggleCollapse" >
          |||
        </div>
        <el-menu background-color="#333744"
                 text-color="#fff"
                 active-text-color="#ffd04b"
                 :collapse="isCollapse"
                 :collapse-transition='false'
                 :router="true"
                 :default-active="activePath"
        >

          <!--            一级菜单-->
          <el-submenu :index="item.id + ''"
                      v-for="item in meanList"
                      :key="item.id"
          >
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <!--            二级菜单-->
            <el-menu-item :index=" '/' + subItem.path "
                          v-for="subItem in item.children"
                          :key="subItem.id"
                          @click="saveNavState('/' + subItem.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                {{ subItem.authName }}
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: 'Home',
  created () {
    this.getMenuList()
    this.activePath = sessionStorage.getItem('path')
  },
  data() {
    return {
      meanList: [],
      iconsObj: {
        125: 'el-icon-s-custom',
        103: 'el-icon-s-check',
        101: 'el-icon-s-goods',
        102: 'el-icon-s-order',
        145: 'el-icon-s-marketing'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error('获取列表失败')
      this.meanList = res.data
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    saveNavState(path) {
      sessionStorage.setItem('path', path)
      this.activePath = path
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #333744;

}

.el-main {
  background-color: #eaedf1;
}
.el-menu {
  border-right: none;
}
.iconfont {
  margin-right: 10px;
}
.toggle-button {
  background-color: #4a5064;
  text-align: center;
  letter-spacing: 0.2em;
  color: #fff;
  cursor: pointer;
  line-height: 24px;
  font-size: 10px;
}

//.toggle-button {
//  background-color: #4a5064;
//  font-size: 10px;
//  line-height: 24px;
//  color: #fff;
//  text-align: center;
//  letter-spacing: 0.2em;
//  cursor: pointer;
//}
</style>
