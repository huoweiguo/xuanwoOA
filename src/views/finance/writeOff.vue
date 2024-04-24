<template>
  <div class="app-container">
    <el-form :model="queryParams" ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="88px">
      <el-form-item label="借款编号" prop="tableName">
        <el-input
          v-model="queryParams.tableName"
          placeholder=""
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="申请人" prop="tableComment">
        <el-input
          v-model="queryParams.tableComment"
          placeholder=""
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="审核状态" prop="status">
        <el-select v-model="queryParams.status" placeholder="" clearable>
          <!-- <el-option
            v-for="dict in dict.type.sys_normal_disable"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          /> -->
        </el-select>
      </el-form-item>
      <el-form-item label="借款状态" prop="status">
        <el-select v-model="queryParams.status" placeholder="" clearable>
          <!-- <el-option
            v-for="dict in dict.type.sys_normal_disable"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          /> -->
        </el-select>
      </el-form-item>
      <el-form-item label="补齐/退回" prop="status">
        <el-select v-model="queryParams.status" placeholder="" clearable>
          <!-- <el-option
            v-for="dict in dict.type.sys_normal_disable"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          /> -->
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
      </el-form-item>
    </el-form>

    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button
          type="primary"
          plain
          size="mini"
          :disabled="multiple"
          v-hasPermi="['tool:gen:code']"
        >借款中</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="primary"
          plain
          size="mini"
          v-hasRole="['admin']"
        >已核销</el-button>
      </el-col>
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

    <el-table border v-loading="loading" :data="tableList" @selection-change="handleSelectionChange">
      <el-table-column type="selection" align="center" width="55"></el-table-column>
      <el-table-column label="序号" type="index" width="50" align="center">
        <template slot-scope="scope">
          <span>{{(queryParams.pageNum - 1) * queryParams.pageSize + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="借款编号"
        align="center"
        prop="tableComment"
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
      <el-table-column
        label="借款金额"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column label="付款状态" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column
        label="核销金额"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="借款/核销差额"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="发票金额(含冲票)"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="发票/核销差额"
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
      <el-table-column label="补齐/退回" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column label="操作" align="center" class-name="small-padding fixed-width" width="160" fixed="right">
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
            icon="el-icon-edit"
            @click="handleEditTable(scope.row, 2)"
            v-hasPermi="['tool:gen:edit']"
          >编辑</el-button>
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
    <el-dialog :title="title" :visible.sync="open" width="790px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="100px">
        <template v-if="state">
          <el-row>
            <el-col :span="8">
              <el-form-item label="项目编号：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="借款日期：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="借款金额：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <template>
              <el-col :span="8">
                <el-form-item label="项目编号：" prop="noticeTitle">
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="项目名称：" prop="noticeTitle">
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="金额：" prop="noticeTitle">
                </el-form-item>
              </el-col>
            </template>
            <el-col :span="16">
              <el-form-item label="核销发票总计：" prop="noticeTitle" label-width="120px">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="支出总计：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-table
                  :data="[]"
                  style="width: 100%">
                  <el-table-column
                    prop="date"
                    label="类别"
                    width="250">
                  </el-table-column>
                  <el-table-column
                    prop="name"
                    label="金额"
                    width="250">
                  </el-table-column>
                  <el-table-column
                    prop="address"
                    label="后续操作"
                    width="250">
                  </el-table-column>
                </el-table>
            </el-col>
            <el-col :span="24">
              <el-form-item label="附件" prop="noticeType">
              
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="借款状态：" prop="noticeTitle">
                <el-select v-model="form.status" placeholder="" clearable>
                  <el-option :value="0" label="待借出"/>
                  <el-option :value="1" label="借款中"/>
                </el-select>
              </el-form-item>
            </el-col>
            
            <el-col :span="8">
              <el-form-item label="审核状态：">

              </el-form-item>
            </el-col>
          </el-row>
          <p>票面开票日期:2024-02-27</p>
        </template>
        <template v-else>
           <el-row>
            <el-col :span="8">
              <el-form-item label="项目编号：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="项目名称：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="备注：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="项目编号：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="项目名称：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="项目备注：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="借款对象：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="开户行：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="银行账号：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-form-item label="借款金额：" prop="noticeTitle">
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-form-item label="借款状态：" prop="noticeTitle">
                <el-select v-model="form.status" placeholder="" clearable>
                  <el-option :value="0" label="待借出"/>
                  <el-option :value="1" label="借款中"/>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
        </template>
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
      title: '查看',
      state: 1
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
      this.$refs["form"].validate(valid => {
        if (valid) {}
      })  
    },
    cancel(){
      this.open = false
    }
  }
};
</script>
