<template>
  <div class="app-container">
    <!-- 搜索 -->
    <el-form ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="68px">
      <el-form-item label="项目编号" prop="">
        <el-input
          placeholder="请输入项目编号"
          clearable
        />
      </el-form-item>
      <el-form-item label="项目名称" prop="">
        <el-input
          placeholder="请输入项目名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="项目时间" prop="">
        <el-date-picker
          style="width: 240px"
          value-format="yyyy-MM-dd"
          type="daterange"
          range-separator="-"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        ></el-date-picker>
      </el-form-item>
      <el-form-item label="项目经理" prop="">
        <el-input
          placeholder="请输入项目经理名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="开票情况" prop="">
        <el-select placeholder="请选择开票情况" clearable>
          <el-option
            v-for="dict in []"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="收款情况" prop="">
        <el-select placeholder="请选择收款情况" clearable>
          <el-option
            v-for="dict in []"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="供应商发票" prop="" label-width="90px">
        <el-select placeholder="请选择供应商发票" clearable>
          <el-option
            v-for="dict in []"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini">重置</el-button>
      </el-form-item>
    </el-form>

    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button @click="open = true" type="primary" size="mini">添加项目</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="info"
          plain
          icon="el-icon-upload"
          size="mini"
        >导出</el-button>
      </el-col>
    </el-row>

    <el-table v-loading="loading" :data="tableList" @selection-change="handleSelectionChange">
      <el-table-column label="序号" type="index" width="50" align="center">
        <template slot-scope="scope">
          <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="项目编号"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="项目名称"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="项目时间"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="项目经理"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="项目成本"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="税前收入"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="税后收入"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="供应商发票"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="开票情况"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="收款情况"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="文件"
        align="center"
        prop="tableName"
        :show-overflow-tooltip="true"
        width="120"
      >
        <template>
          <el-button type="text">查看</el-button>
        </template>
      </el-table-column>
      <el-table-column
            label="操作"
            align="center"
            width="160"
            class-name="small-padding fixed-width"
            fixed="right"
          >
            <template >
              <el-button
                type="text"
                size="small"
                icon="el-icon-view"
              >查看</el-button>
              <el-button
                size="mini"
                type="text"
                icon="el-icon-edit"
              >编辑</el-button>
              <el-button
                size="mini"
                type="text"
                icon="el-icon-delete"
              >删除</el-button>
            </template>
          </el-table-column>
    </el-table>
    <pagination
      v-show="total>0"
      :total="total"
      :page.sync="queryParams.pageNum"
      :limit.sync="queryParams.pageSize"
      @pagination="getList"
    />

    <!-- 预览界面 -->
    <el-dialog :title="title" :visible.sync="open" width="780px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="120px">
        <el-row>
          <el-col :span="11">
            <el-form-item label="项目名称：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="项目时期：" prop="">
              <el-date-picker
                style="width: 240px"
                value-format="yyyy-MM-dd"
                type="daterange"
                range-separator="-"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
              ></el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm">确 定</el-button>
        <el-button @click="cancel">取 消</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 遮罩层
      loading: false,
      // 总条数
      total: 0,
      // 表数据
      tableList: [
        {}
      ],
      // 显示搜索条件
      showSearch: true,
      // 非多个禁用
      multiple: true,
      queryParams: {
        pageNum: 1,
        pageSize: 10,
      },
      open: false,
      title: '添加项目'
    }
  },
  methods:{
    /** 查询表集合 */
    getList() {
      // this.loading = true;
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.queryParams.pageNum = 1;
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.dateRange = [];
      this.resetForm("queryForm");
      this.handleQuery();
    },
    /** 预览按钮 */
    handleEditTable(row, state) {
      this.open = true
      this.form = row
    },
    // 多选框选中数据
    handleSelectionChange(selection) {
      this.ids = selection.map(item => item.tableId);
      this.tableNames = selection.map(item => item.tableName);
      this.single = selection.length != 1;
      this.multiple = !selection.length;
    },
    submitForm(){
      this.$refs["form"].validate(valid => {
        if (valid) {}
      })  
    },
    cancel(){
      this.open = false
    }
  }

}
</script>

<style scoped lang="scss">
</style>