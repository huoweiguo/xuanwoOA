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
      <el-form-item label="品牌方" prop="">
        <el-select placeholder="请选择品牌方" clearable>
          <el-option
              v-for="dict in dict.type.sys_user_sex"
              :key="dict.value"
              :label="dict.label"
              :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="对接人" prop="">
        <el-input
          placeholder="请输入对接人"
          clearable
        />
      </el-form-item>
      <el-form-item label="PO" prop="">
        <el-input
          placeholder="请输入PO"
          clearable
        />
      </el-form-item>
      <el-form-item label="审核状态" prop="">
        <el-select placeholder="请选择审核状态" clearable>
          <el-option
              v-for="dict in dict.type.sys_user_sex"
              :key="dict.value"
              :label="dict.label"
              :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="开票状态" prop="">
        <el-select placeholder="请选择开票状态" clearable>
          <el-option
              v-for="dict in dict.type.sys_user_sex"
              :key="dict.value"
              :label="dict.label"
              :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="收款状态" prop="">
        <el-select placeholder="请选择开票状态" clearable>
          <el-option
              v-for="dict in dict.type.sys_user_sex"
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
        <el-button @click="open = true" type="primary" size="mini">添加开票申请</el-button>
      </el-col>
    </el-row>

    <el-table v-loading="loading" :data="tableList" @selection-change="handleSelectionChange" border="">
      <el-table-column label="序号" type="index" width="50" align="center">
        <template slot-scope="scope">
          <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="项目编号"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="项目名称"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="品牌方"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
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
        label="税前金额"
        align="center"
        prop=""
        :show-overflow-tooltip="true"
      />
      <el-table-column
        label="税后金额"
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
        label="开票状态"
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
                @click="open3 = true"
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

    <!-- 添加开票申请 -->
    <el-dialog :title="title" :visible.sync="open" width="780px" append-to-body>
      <el-form label-position="top" ref="form" :model="form" :rules="rules" label-width="120px">
        <el-row>
          <el-col :span="11">
            <el-form-item label="项目编号：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="项目名称：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="项目收入：" prop="">
              <el-input class="inputW"/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="区域/部门：" prop="">
              <el-select placeholder="请选择开票状态" clearable>
                <el-option
                    v-for="dict in dict.type.sys_user_sex"
                    :key="dict.value"
                    :label="dict.label"
                    :value="dict.value"
                />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="对接人：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="品牌方：" prop="">
              <el-select placeholder="请选择品牌方" clearable>
                <el-option
                    v-for="dict in dict.type.sys_user_sex"
                    :key="dict.value"
                    :label="dict.label"
                    :value="dict.value"
                />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="开户行：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="银行账号：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="税号：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="地址电话：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="发票内容：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="付款备注：" prop="">
              <el-input  type="textarea" :rows="2"/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="PO：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="税前地址：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="税后金额：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="备注：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11">
            <el-form-item label="发票金额：" prop="">
              <el-input/>
            </el-form-item>
          </el-col>
          <el-col :span="11" :push="2">
            <el-form-item label="发票寄送地址：" prop="">
              <el-select placeholder="请选择发票寄送地址" clearable>
                <el-option
                    v-for="dict in dict.type.sys_user_sex"
                    :key="dict.value"
                    :label="dict.label"
                    :value="dict.value"
                />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
            <el-col :span="8">
              <el-form-item label="附件：" prop="">
                <FileUpload disabled :limit="1"/>
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
  dicts: ['sys_user_sex'],
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
      rules:{},
      // 添加项目开关
      open: false,
      title: '添加开票申请',
      editForm:{},
      editRules:{},
      name:''
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
.flex{
  display: flex;
  align-items: end;
}
.inputW{
  display: block;
  width: 218px;
  margin:0px 10px 10px 0;
}
::v-deep .el-form-item--medium .el-form-item__label{
  line-height: 15px;
}
</style>