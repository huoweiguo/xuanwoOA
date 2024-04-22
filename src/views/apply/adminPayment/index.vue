<template>
  <div class="app-container">
     <!-- 搜索 -->
     <el-form ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="90px">
        <el-form-item label="项目名称" prop="">
          <el-input
            placeholder="请输入项目名称"
            clearable
          />
        </el-form-item>
        <el-form-item label="供应商" prop="">
          <el-input
            placeholder="请输入供应商"
            clearable
          />
        </el-form-item>
        <el-form-item label="服务内容" prop="">
          <el-input
            placeholder="请输入服务内容"
            clearable
          />
        </el-form-item>
        <el-form-item label="审核状态" prop="">
          <el-select placeholder="请选择审核状态" clearable>
            <el-option
            />
          </el-select>
        </el-form-item>
        <el-form-item label="付款状态" prop="" label-width="90px">
          <el-select placeholder="请选择付款状态" clearable>
            <el-option
            />
          </el-select>
        </el-form-item>
        <el-form-item label="供应商发票" prop="" label-width="90px">
          <el-select placeholder="请选择供应商发票" clearable>
            <el-option
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
          <el-button @click="open = true" type="primary" size="mini">添加行政付款单</el-button>
        </el-col>
      </el-row>

      <el-table v-loading="loading" :data="tableList" @selection-change="handleSelectionChange" border>
          <el-table-column label="序号" type="index" width="50" align="center">
            <template slot-scope="scope">
              <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="项目名称"
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
            label="审核状态"
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
          <el-table-column
            label="供应商发票"
            align="center"
            prop=""
            :show-overflow-tooltip="true"
          />
          <el-table-column
            label="票面开票日期"
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
        <!-- 添加付款申请 -->
        <el-dialog :title="title" :visible.sync="open" width="780px" append-to-body>
          <el-form ref="form" label-position="top" :model="form" :rules="rules" label-width="120px">
            <el-row>
              <el-col :span="11">
                <el-form-item label="项目名称：" prop="">
                  <el-input/>
                </el-form-item>
              </el-col>
              <el-col :span="11" :push="2">
                <el-form-item label="服务内容：" prop="">
                  <el-input/>
                </el-form-item>
              </el-col>
              <el-col :span="11">
                <el-form-item label="供应商：" prop="">
                  <el-select clearable>
                    <el-option
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
                <el-form-item label="付款金额：" prop="">
                  <el-input/>
                </el-form-item>
              </el-col>
              <el-col :span="24">
                <el-form-item label="付款备注：" prop="">
                  <el-input  type="textarea" :rows="2"/>
                </el-form-item>
              </el-col>
              <el-row>
                <el-col :span="8">
                  <el-form-item label="合同：" prop="">
                    <FileUpload disabled :limit="1"/>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="8">
                  <el-form-item label="报价单：" prop="">
                    <FileUpload disabled :limit="1"/>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="8">
                  <el-form-item label="发票：" prop="">
                    <FileUpload disabled :limit="1"/>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="11">
                  <el-form-item label="发票类型：" prop="">
                  <el-select clearable>
                    <el-option
                    />
                  </el-select>
                </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="11">
                  <el-form-item label="开票日期：" prop="">
                    <el-input />
                  </el-form-item>
                </el-col>
              </el-row>
            </el-row>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button type="primary" @click="submitForm">确 定</el-button>
            <el-button @click="cancel">取 消</el-button>
          </div>
        </el-dialog>

        <!-- 撤回 -->
        <el-dialog :title="cancelTitle" :visible.sync="cancelOpen" width="780px" append-to-body>
          <el-form ref="form" :model="cancelForm" :rules="cancelRules" label-width="120px">
            <el-col :span="24">
                <el-form-item label="撤回原因：" prop="">
                  <el-input  type="textarea" :rows="2"/>
                </el-form-item>
              </el-col>
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
  name:'adminPayment',
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
::v-deep .el-form-item--medium .el-form-item__label{
  line-height: 15px;
}
</style>