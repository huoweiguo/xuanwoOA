<template>
  <div class="app-container">
    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button @click="open = true" type="primary" size="mini">添加借款申请</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button @click="open = true" type="primary" size="mini">添加核销申请</el-button>
      </el-col>
      <el-col :span="1.5">
        <span class="fw-700">借款总额：110000</span>
      </el-col>
    </el-row>

    <el-table class="mt-20" v-loading="loading" :data="tableList" @selection-change="handleSelectionChange" border>
        <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="借款编号"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款金额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款状态"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="核销金额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款/核销差额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="发票金额（含冲票）"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="发票/核销差额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="审核状态"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="补齐/退回"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="操作"
          align="center"
          width="160"
          class-name="small-padding fixed-width"
          fixed="right"
        >
          <template slot-scope="scope">
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
              @click="cancelOpen = true"
            >撤回</el-button>
            <el-button
              @click="handleDelete(scope.row)"
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

      <el-table class="mt-20" v-loading="loading" :data="tableList" @selection-change="handleSelectionChange" border>
        <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="借款编号"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款金额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款状态"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="核销金额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="借款/核销差额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="发票金额（含冲票）"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="发票/核销差额"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="审核状态"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="补齐/退回"
          align="center"
          prop=""
          :show-overflow-tooltip="true"
        />
        <el-table-column
          label="操作"
          align="center"
          width="160"
          class-name="small-padding fixed-width"
          fixed="right"
        >
          <template>
            <el-button
              type="text"
              size="small"
              icon="el-icon-view"
            >查看</el-button>
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
  </div>
</template>

<script>
export default {
  name:'desBorrow',
  data() {
    return {
      // 显示搜索条件
      showSearch: true,
      // 遮罩层
      loading: false,
      // 总条数
      total: 0,
      // 表数据
      tableList: [
        {
          id:1
        }
      ],
      form:{

      },
      rules:{},
      cancelForm:{

      },
      cancelRules:{},
      queryParams: {
        pageNum: 1,
        pageSize: 10,
      },
      // 添加付款申请
      open: false,
      cancelOpen:false,
      title: '添加付款申请',
      cancelTitle:'撤回'
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
     /** 删除按钮操作 */
     handleDelete(row) {
      const userIds = row.id || this.ids;
      this.$modal.confirm('是否确认删除用户编号为"' + userIds + '"的数据项？').then(function() {
        return delUser(userIds);
      }).then(() => {
        this.getList();
        this.$modal.msgSuccess("删除成功");
      }).catch(() => {});
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
.fw-700{
  font-weight: 700;
  line-height: 1.5;
}
.check-box{
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.income{
  display: flex;
  p{
    margin-left: 20px;
  }
}
.flex{
  display: flex;
  align-items: end;
}
.inputW{
  display: block;
  width: 188px;
  margin:0px 10px 10px 0;
}
.mt-20{
  margin-top: 20px;
}
::v-deep .el-form-item--medium .el-form-item__label{
  line-height: 15px;
}
</style>