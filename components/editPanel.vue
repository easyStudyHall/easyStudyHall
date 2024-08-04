<template>
  <div class="editPanelContainer">
    <div class="table">
      <el-table :data="homeworkDetails" style="width: 100%" border>
        <el-table-column
          prop="subject"
          label="科目"
          width="90"
        ></el-table-column>
        <el-table-column
          prop="category"
          label="作业类目"
          width="100"
        ></el-table-column>
        <el-table-column prop="detail" label="作业详情"></el-table-column>
        <el-table-column label="操作" width="100">
          <template #default="scope">
            <el-button @click="deleteItem(scope.row)" type="danger"
              >删除</el-button
            >
          </template>
        </el-table-column>
        <!-- <el-table-column prop="created_at" label="操作日期"></el-table-column> -->
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  name: "editPanel",
  props: {
    subjectHomework: {
      type: Array,
      required: true,
    },
  },

  methods: {
    /**
     * 删除项目
     *
     * @param index 要删除项目的索引
     * @emits deleteItem 触发 deleteItem 事件，传递要删除项目的索引
     */
    deleteItem(index) {
      this.$emit("deleteItem", index);
    },
  },
  computed: {
    /**
     * 获取作业详情
     *
     * @returns 返回作业对象
     */
    homeworkDetails() {
      return this.subjectHomework;
    },
  },
};
</script>

<style scoped>
</style>