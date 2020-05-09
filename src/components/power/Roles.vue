<template>
    <div>
      <!-- 面包屑导航 -->
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>权限管理</el-breadcrumb-item>
        <el-breadcrumb-item>角色列表</el-breadcrumb-item>
      </el-breadcrumb>
      <!-- 卡片视图 -->
      <el-card>
        <!-- 添加角色按钮区域 -->
        <el-row>
          <el-col>
            <el-button type="primary">添加角色</el-button>
          </el-col>
        </el-row>
        <!--        角色列表-->
        <el-table :data="roleList" border stripe>
<!--          展开列-->
          <el-table-column type="expand">
            <template slot-scope="scope">
              <el-row :class="['bdbottom',index1 === 0 ? 'bdtop' : '','vcenter']" v-for="(item1,index1) in scope.row.children" :key="item1.id">
<!--                渲染一级权限-->
                <el-col :span="5">
                  <el-tag>{{item1.authName}}</el-tag>
                  <i class="el-icon-caret-right"></i>
                </el-col>
<!--                渲染二级和三级权限-->
                <el-col :span="19">
<!--                 循环嵌套二级权限 -->
                  <el-row :class="[index2 === 0 ? '' : 'bdtop','vcenter']" v-for="(item2,index2) in item1.children" :key="item2.id" >
                    <el-col :span="6" >
                      <el-tag type="success">{{item2.authName}}</el-tag>
                      <i class="el-icon-caret-right"></i>
                    </el-col>
                    <el-col :span="18">
<!--                      循环嵌套三级权限-->
                        <el-tag type="warning" v-for="item3 in item2.children" :key="item3.id">{{item3.authName}}</el-tag>
                    </el-col>
                  </el-row>
                </el-col>
              </el-row>
            </template>
          </el-table-column>
<!--          索引列-->
          <el-table-column type="index"></el-table-column>
          <el-table-column label="角色名称" prop="roleName"></el-table-column>
          <el-table-column label="角色描述" prop="roleDesc"></el-table-column>
          <el-table-column label="操作" >
            <template>
              <el-button type="primary" icon="el-icon-edit" size="mini" >编辑</el-button>
              <el-button type="danger" icon="el-icon-delete" size="mini" >删除</el-button>
              <el-tooltip  effect="dark" content="分配权限" placement="top-start" :enterable="false">
                <el-button type="warning" icon="el-icon-setting" size="mini">分配权限</el-button>
              </el-tooltip>
            </template>
          </el-table-column>
        </el-table>
      </el-card>
    </div>
</template>

<script>
export default {
  data () {
    return {
      // 所有角色列表数据
      roleList: [],
      // 控制分配权限对话框的显示与隐藏
      setRightDialogVisible: false,
      // 所有权限的数据
      rightsList: [],
      // 树形控件的属性绑定对象
      treeProps: {
        label: 'authName',
        children: 'children'
      },
      // 默认选中的节点Id值数组
      defKeys: [],
      // 当前即将分配权限的角色id
      roleId: ''
    }
  },
  created () {
    this.getRolesList()
  },
  methods: {
  // 获取所有角色的列表
    async getRolesList () {
      const { data: res } = await this.$http.get('role/roles')

      if (res.status !== 200) {
        return this.$message.error('获取角色列表失败！')
      }
      this.roleList = res.data
      console.log(this.roleList)
    }
  }
}
</script>

<style lang="less" scoped>
  .el-tag{
    margin: 7px;
  }
  .bdtop {
    border-top: 1px solid #eee;
  }
  .bdbottom {
    border-bottom: 1px solid #eee;
  }
  .vcenter{
    display: flex;
    align-items: center;
  }
</style>
