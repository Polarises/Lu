<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
     <el-table :data="rightsList"
               :stripe="true"
               :border="true"
               height="500px"
     >
       <el-table-column
         type="index"
         label=""
         width="180">
       </el-table-column>
       <el-table-column
         label="权限名称"
         prop="authName"
         width="180">
       </el-table-column>
       <el-table-column
         prop="path"
         label="路径"
         width="180">
       </el-table-column>
       <el-table-column
         prop="level"
         label="权限等级"
         width="180"
       >
         <template v-slot="scope">
           <el-tag v-if="scope.row.level === '0'" >一级</el-tag>
           <el-tag type="success" v-else-if="scope.row.level === '1'">二级</el-tag>
           <el-tag type="warning" v-else>三级</el-tag>
         </template>
       </el-table-column>
     </el-table>
    </el-card>

  </div>
</template>

<script>
export default {
  created () {
    this.getRightsList()
  },
  data () {
    return {
      rightsList: []
    }
  },
  name: 'Right',
  methods: {
    async getRightsList () {
      const { data: res } = await this.$http.get('rights/list')
      if (res.meta.status !== 200) return this.$message.error('读取权限列表错误')
      this.rightsList = res.data
      console.log(this.rightsList)
    }
    //  结束
  }
}
</script>

<style scoped>

</style>
