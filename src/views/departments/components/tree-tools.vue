<template>
  <el-row
    type="flex"
    justify="space-between"
    align="middle"
    style="height: 40px; width: 100%"
  >
    <el-col>
      <!-- 名称应该变成 对应的节点中的name -->
      <span>{{ treeNode.name }}</span>
    </el-col>
    <el-col :span="4">
      <el-row type="flex" justify="end">
        <!-- 两个内容 -->
        <el-col>{{ treeNode.manager }}</el-col>
        <el-col>
          <!-- 下拉菜单 element -->
          <el-dropdown @command="operateDepts">
            <span> 操作<i class="el-icon-arrow-down" /> </span>
            <!-- 下拉菜单 -->
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="add">添加子部门</el-dropdown-item>
              <el-dropdown-item v-if="!isRoot" command="edit"
                >编辑部门</el-dropdown-item
              >
              <el-dropdown-item v-if="!isRoot" command="del"
                >删除部门</el-dropdown-item
              >
            </el-dropdown-menu>
          </el-dropdown>
        </el-col>
      </el-row>
    </el-col>
  </el-row>
</template>

<script>
import {delDepartments} from "@/api/departments"
export default {
  props: {
    treeNode: {
      required: true, // 设置当前数据为必填
      type: Object, // 类型是Object
    },
    isRoot: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    operateDepts(type) {
      if (type === "add") {
        this.$emit("addDepts", this.treeNode);
        // localStorage.setItem('tan','2')
        // 添加子部门的操作
      } else if (type === "edit") {
        //  编辑部门的操作
        this.$emit('editDepts', this.treeNode)
        // localStorage.setItem('tan','1')
      } else {
        //  删除操作
        this.$confirm("确定要删除该部门吗")
          .then(() => {
            // 如果点击了确定就会进入then
            return delDepartments(this.treeNode.id); // 返回promise对象
          })
          .then(() => {
            //  如果删除成功了  就会进入这里
            this.$emit("delDepts"); // 触发自定义事件
            this.$message.success("删除部门成功");  
          });
      }
    },
    
  },
};
</script>
<style></style>
