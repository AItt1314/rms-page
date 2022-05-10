<template>
  <div id="rights" class="page">
    <!-- 头部面包屑区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片内容区域 -->
    <el-card class="box-card">
      <!-- 渲染的列表区域 -->
      <el-row>
        <el-table :data="RightsList" style="width: 100%" stripe :border="true">
          <el-table-column type="index" label="#" />
          <el-table-column prop="authName" label="权限名称" />
          <el-table-column prop="path" label="路径" />
          <el-table-column label="权限等级">
            <!-- 作用域插槽实现标签的展现 ， 在插槽中将三个标签都写上，再用v-if决定渲染哪一个 -->
            <template slot-scope="scope">
              <el-tag type="success" v-if="scope.row.level == 0">一级</el-tag>
              <el-tag v-if="scope.row.level == 1">二级</el-tag>
              <el-tag type="warning" v-if="scope.row.level == 2">三级</el-tag>
            </template>
          </el-table-column>
        </el-table>
      </el-row>
    </el-card>
  </div>
</template>


<script>
export default {
  data() {
    return {
      RightsList: [{"id":101,"authName":"商品管理","level":"0","pid":0,"path":"goods"},{"id":102,"authName":"订单管理","level":"0","pid":0,"path":"orders"},{"id":103,"authName":"权限管理","level":"0","pid":0,"path":"rights"},{"id":104,"authName":"商品列表","level":"1","pid":101,"path":"goods"},{"id":105,"authName":"添加商品","level":"2","pid":104,"path":"goods"},{"id":107,"authName":"订单列表","level":"1","pid":102,"path":"orders"},{"id":109,"authName":"添加订单","level":"2","pid":107,"path":"orders"},{"id":110,"authName":"用户列表","level":"1","pid":125,"path":"users"},{"id":111,"authName":"角色列表","level":"1","pid":103,"path":"roles"},{"id":112,"authName":"权限列表","level":"1","pid":103,"path":"rights"},{"id":115,"authName":"分类参数","level":"1","pid":101,"path":"params"},{"id":116,"authName":"商品修改","level":"2","pid":104,"path":"goods"},{"id":117,"authName":"商品删除","level":"2","pid":104,"path":"goods"},{"id":121,"authName":"商品分类","level":"1","pid":101,"path":"categories"},{"id":122,"authName":"添加分类","level":"2","pid":121,"path":"categories"},{"id":123,"authName":"删除分类","level":"2","pid":121,"path":"categories"},{"id":125,"authName":"用户管理","level":"0","pid":0,"path":"users"},{"id":129,"authName":"添加角色","level":"2","pid":111,"path":"roles"},{"id":130,"authName":"删除角色","level":"2","pid":111,"path":"roles"},{"id":131,"authName":"添加用户","level":"2","pid":110,"path":"users"},{"id":132,"authName":"删除用户","level":"2","pid":110,"path":"users"},{"id":133,"authName":"更新用户","level":"2","pid":110,"path":"users"},{"id":134,"authName":"角色授权","level":"2","pid":111,"path":"roles"},{"id":135,"authName":"取消角色授权","level":"2","pid":111,"path":"roles"},{"id":136,"authName":"获取用户详情","level":"2","pid":110,"path":"users"},{"id":137,"authName":"分配用户角色","level":"2","pid":110,"path":"users"},{"id":138,"authName":"获取角色列表","level":"2","pid":111,"path":"roles"},{"id":139,"authName":"获取角色详情","level":"2","pid":111,"path":"roles"},{"id":140,"authName":"更新角色信息","level":"2","pid":111,"path":"roles"},{"id":141,"authName":"更新角色权限","level":"2","pid":111,"path":"roles"},{"id":142,"authName":"获取参数列表","level":"2","pid":115,"path":"categories"},{"id":143,"authName":"创建商品参数","level":"2","pid":115,"path":"categories"},{"id":144,"authName":"删除商品参数","level":"2","pid":115,"path":"categories"},{"id":145,"authName":"数据统计","level":"0","pid":0,"path":"reports"},{"id":146,"authName":"数据报表","level":"1","pid":145,"path":"reports"},{"id":147,"authName":"查看权限","level":"2","pid":112,"path":"rights"},{"id":148,"authName":"查看数据","level":"2","pid":146,"path":"reports"},{"id":149,"authName":"获取分类详情","level":"2","pid":121,"path":"categories"},{"id":150,"authName":"更新商品图片","level":"2","pid":104,"path":"goods"},{"id":151,"authName":"更新商品属性","level":"2","pid":104,"path":"goods"},{"id":152,"authName":"更新商品状态","level":"2","pid":104,"path":"goods"},{"id":153,"authName":"获取商品详情","level":"2","pid":104,"path":"goods"},{"id":154,"authName":"订单更新","level":"2","pid":107,"path":"orders"},{"id":155,"authName":"获取订单详情","level":"2","pid":107,"path":"orders"},{"id":156,"authName":"分类参数添加","level":"2","pid":101,"path":"categories"},{"id":157,"authName":"分类参数删除","level":"2","pid":101,"path":"categories"},{"id":158,"authName":"分类参数详情","level":"2","pid":101,"path":"categories"},{"id":159,"authName":"设置管理状态","level":"2","pid":110,"path":"users"}],
    };
  },
  methods: {
    async getRightsList() {
      const { data: res } = await this.$http.get("rights/list");
      if (res.meta.status != 200)
        return this.$message.error("请求权限列表失败");
      this.RightsList = res.data;
      // console.log(res);
    },
  },
  created() {
    // 这里需要加this，否则会报错该方法没有被定义
    this.getRightsList();
  },
};
</script>

<style scoped>
</style>