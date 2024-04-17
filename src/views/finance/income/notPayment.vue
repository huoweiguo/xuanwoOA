<template>
  <div class="app-container">
    <div class="title">未收款项目统计</div>
    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button
          type="info"
          plain
          icon="el-icon-upload"
          size="mini"
          v-hasPermi="['tool:gen:import']"
        >导出</el-button>
      </el-col>
    </el-row>

    <el-table v-loading="loading" :data="tableList">
      <el-table-column
        label="品牌"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="(不含税)未收款"
        align="center"
        prop="className"
      />
      <el-table-column
        label="(含税)未收款"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
      />
    </el-table>
    <el-row style="margin-top: 5px">
      <el-col :span="8" class="txt"></el-col>
      <el-col :span="8" class="txt">(不含税)未收款合计：</el-col>
      <el-col :span="8" class="txt">(含税)未收款合计：</el-col>
    </el-row>
    <!-- <pagination
      v-show="total>0"
      :total="total"
      :page.sync="queryParams.pageNum"
      :limit.sync="queryParams.pageSize"
      @pagination="getList"
    /> -->
  </div>
</template>

<script>

export default {
  name: "CMaker",
  data() {
    return {
      // 遮罩层
      loading: false,
      // 总条数
      total: 0,
      // 表数据
      tableList: [],
      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        tableName: undefined,
        tableComment: undefined
      },
    };
  },
  created() {
    this.getList();
  },
  activated() {
    const time = this.$route.query.t;
    if (time != null && time != this.uniqueId) {
      this.uniqueId = time;
      this.queryParams.pageNum = Number(this.$route.query.pageNum);
      this.getList();
    }
  },
  methods: {
    /** 查询表集合 */
    getList() {
      // this.loading = true;
    }
  }
};
</script>
<style lang="scss" scoped>
.title{
  width: 100%;
  text-align: center;
}
.txt{
  text-align: center;
  height: 30px;
  font-size: 14px;
}
</style>
