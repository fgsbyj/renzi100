<template>
  <div>
    <el-card class="tree-card">
      <tree-tools :treeNode="company" :is-root="true" @addDepts="addDepts"></tree-tools>
    </el-card>
    <el-card class="tree-card">
      <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick">
        <tree-tools slot-scope="{ data }" :treeNode="data" @delDepts="getDepartments" @addDepts="addDepts" @editDepts="editDepts"/>
      </el-tree>
    </el-card>
    <add-dept ref="showDialog" :showDialog.sync="showDialog" :treeNode="node" @rushnew="getDepartments"/>
  </div>
</template>

<script>
import treeTools from "@/views/departments/components/tree-tools.vue";
import { getDepartments } from '@/api/departments'
import {tranListToTreeData} from "@/utils/index"
import AddDept from '@/views/departments/components/add-dept.vue' 
export default {
  components: {
    treeTools,
    AddDept
  },
  created() {
    this.getDepartments() // 调用自身的方法
  },
  data() {
    return {
      company: { name: '江苏传智播客教育科技股份有限公司', manager: '负责人' },
      data: [],
      showDialog: false,
      node:{},
      defaultProps: {
        label: "name",
      },
    };
  },
  methods: {
    handleNodeClick(data) {
      // console.log(data);
    },
    async getDepartments() {
      const result = await getDepartments()
      this.company = { name: result.companyName, manager: '负责人' }
      this.data = tranListToTreeData(result.depts, '')
    },
    addDepts(node) {
      this.showDialog = true // 显示弹层
      // 因为node是当前的点击的部门， 此时这个部门应该记录下来,
      this.node = node
    },
    editDepts(node) {
      // 首先打开弹层
      this.showDialog = true
      this.node = node // 赋值操作的节点
      this.$refs.showDialog.getDepartDetail(node.id)
    }
  },
};
</script>

<style scoped>
.tree-card {
  padding: 30px  140px;
  font-size:14px;
}
</style>
