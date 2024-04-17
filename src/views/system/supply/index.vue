<template>
  <div class="app-container">
    <!-- 搜索 -->
    <el-form ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="60px"> 
      <el-form-item label="类型" prop="">
        <el-input
          placeholder="类型"
          clearable
        />
      </el-form-item>
      <el-form-item label="供应商名称" prop="" label-width="90px">
        <el-input
          placeholder="请输入供应商名称"
          clearable
        />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini">重置</el-button>
      </el-form-item>
    </el-form>

    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button @click="open = true" type="primary" size="mini">添加供应商</el-button>
      </el-col>
    </el-row>

    <el-table v-loading="loading" :data="tableList" @selection-change="handleSelectionChange">
      <el-table-column label="序号" type="index" width="50" align="center">
        <template slot-scope="scope">
          <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="类型"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="供应商名称"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="开户行"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="银行账号"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="税号"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="地址"
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

    <!-- 添加品牌商 -->
    <el-dialog :title="title" :visible.sync="open" width="780px" append-to-body>
      <el-form ref="form" label-position="top" :model="form" :rules="rules" label-width="120px">
        <el-row>
          <el-col :span="10">
            <el-form-item label="类型：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="10" :push="2">
            <el-form-item label="供应商名称：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="开户行：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="10" :push="2">
            <el-form-item label="银行账号：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="税号：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="10" :push="2">
            <el-form-item label="地址：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="联系人：" prop="">
              <el-input class="inputW"/>
              <el-input class="inputW"/>
            </el-form-item>
          </el-col>
          <el-col :span="8" :push="1">
            <el-form-item label="联系人电话：" prop="">
              <div class="flex">
                  <div>
                    <el-input class="inputW"/>
                    <el-input class="inputW"/>
                  </div>
                  <el-button class="button-new-tag" type="danger">删除</el-button>
                  <el-button class="button-new-tag" type="primary">+ 添加</el-button>
               </div>
            </el-form-item>
          </el-col>

        </el-row>
        <div slot="footer" class="dialog-footer">
          <el-button type="primary" @click="submitForm">确 定</el-button>
          <el-button @click="cancel">取 消</el-button>
        </div>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 显示搜索条件
      showSearch: true,
      open:false,
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
      form:{},
      rules:{},
      queryParams: {
        pageNum: 1,
        pageSize: 10,
      },
      title: '添加供应商'
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
.flex{
  display: flex;
  align-items: end;
}
.inputW{
  display: block;
  width: 270px;
  margin:0px 10px 10px 0;
}
.button-new-tag{
  height: 40px;
  margin-bottom: 10px;
}
::v-deep .el-form-item--medium .el-form-item__label{
  line-height: 15px;
}
</style>