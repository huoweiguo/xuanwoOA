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
            <template slot-scope="scope">
              <el-button
                @click="open2 = true"
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

    <!-- 添加项目界面 -->
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

    <!-- 查看项目界面 -->
    <el-dialog :visible.sync="open2" width="1080px" append-to-body>
      <div class="check-box">
        <h3>成本明细</h3>
        <p>成本总计：7777666元</p>
        <el-table>
          <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
          </template>
        </el-table-column>
          <el-table-column
            label="负责人"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="服务内容"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="供应商"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="付款金额"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="专票/普票"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="供应商发票"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="审批状态"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="付款状态"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
        </el-table>
        <h3>收入明细</h3>
        <div class="income">
          <p>税前收入总计：1222222</p>
          <p>税后收入总计：1222222</p>
        </div>
        <el-table>
          <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
          </template>
        </el-table-column>
          <el-table-column
            label="区域/部门"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="对接人"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="PO"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="税前收入"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="税后收入"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="开票情况"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="收款状态"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
        </el-table>
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
      form:{

      },
      rules:[],
      // 添加项目开关
      open: false,
      // 查看项目开关
      open2:false,
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
</style>