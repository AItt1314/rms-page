<template>
  <!-- 首页 -->
  <el-container class="home_container">
    <!-- 顶部栏 -->
    <el-header>
      <div class="home_title">
        <img src="../assets/img/homeLogo.png" alt="" /><span
          >电商后台管理系统</span
        >
      </div>
      <el-button type="info" @click="quit">退出</el-button>
    </el-header>
    <el-container>
      <!-- 左侧侧边栏 -->
      <!-- 根据是否折叠来决定侧边栏的宽度 -->
      <el-aside :width="collapse.isCollapse ? '64px' : '200px'">
        <!-- 折叠按钮 -->
        <div class="el-icon-s-operation toggle_btn" @click="toggle"></div>
        <!-- 大菜单 -->
        <!-- 最多可打开一项，是否可折叠，折叠动画，是否开启路由模式（在子菜单中用index指向路由） -->
        <el-menu
          class="el-menu-vertical-demo"
          background-color="#ecdef0"
          text-color="#590505"
          active-text-color="#2000ff"
          :unique-opened="true"
          :collapse="collapse.isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <!-- 每一个一级菜单都应该有自己独一无二的index，否则点击按钮会导致所有菜单同步 -->
          <el-submenu
            v-for="item in MenuList"
            :key="item.id"
            :index="'/' + item.path"
          >
            <!-- 一级菜单模板区 -->
            <template slot="title">
              <!-- 一级菜单图标 -->
              <!-- 动态绑定，obj[index] 表示对象中键为index的键值 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 一级菜标题名 -->
              <span>{{ item.authName }}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subitem.path"
              v-for="subitem in item.children"
              :key="subitem.id"
              @click="storagePath('/' + subitem.path)"
              ><template slot="title">
                <!-- 二级菜单图标 -->
                <i class="el-icon-menu"></i>
                <!-- 二级菜标题名 -->
                <span>{{ subitem.authName }}</span>
              </template></el-menu-item
            >
          </el-submenu>
        </el-menu></el-aside
      >
      <!-- 主要内容栏 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      //ajax 获取的数据
      MenuList: [{"id":125,"authName":"用户管理","path":"users","children":[{"id":110,"authName":"用户列表","path":"users","children":[],"order":null}],"order":1},{"id":103,"authName":"权限管理","path":"rights","children":[{"id":111,"authName":"角色列表","path":"roles","children":[],"order":null},{"id":112,"authName":"权限列表","path":"rights","children":[],"order":null}],"order":2},{"id":101,"authName":"商品管理","path":"goods","children":[{"id":104,"authName":"商品列表","path":"goods","children":[],"order":1},{"id":115,"authName":"分类参数","path":"params","children":[],"order":2},{"id":121,"authName":"商品分类","path":"categories","children":[],"order":3}],"order":3},{"id":102,"authName":"订单管理","path":"orders","children":[{"id":107,"authName":"订单列表","path":"orders","children":[],"order":null}],"order":4},{"id":145,"authName":"数据统计","path":"reports","children":[{"id":146,"authName":"数据报表","path":"reports","children":[],"order":null}],"order":5}],
      // 为不同的一级菜单添加不同的图标
      iconsObj: {
        125: "el-icon-user",
        103: "el-icon-s-tools",
        101: "el-icon-sell",
        102: "el-icon-s-ticket",
        145: "el-icon-postcard",
      },
      //是否折叠，及折叠后的侧边栏宽度
      collapse: {
        isCollapse: false,
      },
      // 当前链接地址
      activePath: "",
    };
  },
  methods: {
    //退出登录事务：1.清空存储的token window.sessionStorage.clear() 2.跳转至登录页 this.$router.push('/login')
    quit() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    async getMenuList() {
      const { data: res } = await this.$http.get("menus");
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg);
      this.MenuList = res.data;
    },
    toggle() {
      this.collapse.isCollapse = !this.collapse.isCollapse;
    },
    storagePath(activePath) {
      window.sessionStorage.setItem("activePath", activePath);
      this.activePath = activePath;
    },
  },
  //   获取左侧所有菜单
  created() {
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
  },
};
</script>


<style scoped>
/* header区域 */
.el-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #0a90e9;
}
.el-header img {
  width: 80px;
}
.home_title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 350px;
  font-size: 25px;
  color: #fff;
}
/* 折叠按钮 */
.toggle_btn {
  position: relative;
  width: 100%;
  height: 30px;
  cursor: pointer;
  background-color: #ced7e7;
}
.el-icon-s-operation:before {
  position: absolute;
  right: 0px;
  font-size: 30px;
  text-align: center;
}
/* 左侧侧边栏区域 */
.el-aside {
  background-color: #ecdef0;
}
.el-submenu [class^="el-icon-"] {
  font-size: 25px;
}
.el-submenu__title span {
  font-size: 18px;
}
.el-submenu [class^="el-icon-"] {
  margin-right: 15px;
}

.el-main {
  background-color: #f3e7e7;
}
.home_container {
  height: 100%;
}
</style>