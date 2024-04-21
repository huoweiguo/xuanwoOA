<template>
  <div class="app-container">
    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button
          type="primary"
          plain
          size="mini"
          :disabled="multiple"
          v-hasPermi="['tool:gen:code']"
        >通过</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
           type="danger"
          plain
          size="mini"
          :disabled="multiple"
          v-hasPermi="['tool:gen:import']"
        >驳回</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="primary"
          plain
          size="mini"
          v-hasRole="['admin']"
        >已支付</el-button>
      </el-col>
    </el-row>

    <el-table border v-loading="loading" :data="tableList" @selection-change="handleSelectionChange">
      <el-table-column type="selection" align="center" width="55"></el-table-column>
      <el-table-column label="序号" type="index" width="50" align="center">
        <template slot-scope="scope">
          <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="项目编号"
        align="center"
        prop="tableComment"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="项目名称"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="服务内容"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="供应商"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="金额"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="申请人"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column label="审核状态" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column label="付款状态" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column label="供应上发票" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column label="票面开票时间" align="center" prop="updateTime" width="160" />
      <el-table-column label="操作" align="center" width="160" class-name="small-padding fixed-width" fixed="right">
        <template slot-scope="scope">
          <el-button
            type="text"
            size="small"
            icon="el-icon-view"
            @click="handleEditTable(scope.row, 1)"
            v-hasPermi="['tool:gen:preview']"
          >查看</el-button>
          <el-button
            type="text"
            size="small"
            icon="el-icon-crop"
            @click="handleWithdraw(scope.row)"
            v-hasPermi="['tool:gen:edit']"
          >撤回</el-button>
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
      <el-form ref="form" :model="form" :rules="rules" label-width="100px">
        <el-row>
          <el-col :span="12">
            <el-form-item label="项目编号：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="项目名称：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="申请人：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="服务内容：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="供应商：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="开户行：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="银行账号：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="付款金额：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="付款备注：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="审核状态：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="审核状态：" prop="noticeTitle">
              <el-radio-group v-model="form.status">
                <!-- <el-radio
                  v-for="dict in dict.type.sys_notice_status"
                  :key="dict.value"
                  :label="dict.value"
                >{{dict.label}}</el-radio> -->
              </el-radio-group>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="付款状态：" prop="noticeTitle">
              <el-radio-group v-model="form.status">
                <!-- <el-radio
                  v-for="dict in dict.type.sys_notice_status"
                  :key="dict.value"
                  :label="dict.value"
                >{{dict.label}}</el-radio> -->
              </el-radio-group>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="供应商发票：" prop="noticeTitle">
              <el-radio-group v-model="form.status">
                <!-- <el-radio
                  v-for="dict in dict.type.sys_notice_status"
                  :key="dict.value"
                  :label="dict.value"
                >{{dict.label}}</el-radio> -->
              </el-radio-group>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="合同：" prop="noticeType">
             
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="报价单：">

            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="发票：">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="发票类型：" prop="noticeTitle">
              <el-radio-group v-model="form.status">
                <!-- <el-radio
                  v-for="dict in dict.type.sys_notice_status"
                  :key="dict.value"
                  :label="dict.value"
                >{{dict.label}}</el-radio> -->
              </el-radio-group>
            </el-form-item>
          </el-col>
        </el-row>
        <p>开票日期:2024-02-27</p>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel">取 消</el-button>
        <el-button type="primary" @click="submitForm(1)">通过</el-button>
        <el-button  type="danger" @click="submitForm(2)">驳回</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>

export default {
  name: "CMaker",
  data() {
    return {
      // 遮罩层
      loading: false,
      // 唯一标识符
      uniqueId: "",
      // 选中数组
      ids: [],
      // 选中表数组
      tableNames: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 显示搜索条件
      showSearch: true,
      // 总条数
      total: 0,
      // 表数据
      tableList: [{id: 1}],
      // 日期范围
      dateRange: "",
      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        tableName: undefined,
        tableComment: undefined
      },
      // 预览参数
      form: {},
      rules: {},
      open: false,
      title: '查看'
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
      this.open = false
    },
    cancel(){
      this.open = false
    }
  }
};
</script>
