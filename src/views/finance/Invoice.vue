<template>
  <div class="app-container">
    <el-form :model="queryParams" ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="68px">
      <el-form-item label="项目编号" prop="tableName">
        <el-input
          v-model="queryParams.tableName"
          placeholder=""
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="项目名称" prop="tableComment">
        <el-input
          v-model="queryParams.tableComment"
          placeholder=""
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="品牌方" prop="status">
        <el-select v-model="queryParams.status" placeholder="" clearable>
          <!-- <el-option
            v-for="dict in dict.type.sys_normal_disable"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          /> -->
        </el-select>
      </el-form-item>
      <el-form-item label="对接人" prop="tableComment">
        <el-input
          v-model="queryParams.tableComment"
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
      <el-form-item label="PO" prop="tableComment">
        <el-input
          v-model="queryParams.tableComment"
          placeholder=""
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="开票状态" prop="status">
        <el-select v-model="queryParams.status" placeholder="" clearable>
          <!-- <el-option
            v-for="dict in dict.type.sys_normal_disable"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          /> -->
        </el-select>
      </el-form-item>
      <el-form-item label="收款状态" prop="status">
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
          type="info"
          plain
          icon="el-icon-upload"
          size="mini"
          v-hasPermi="['tool:gen:import']"
        >导出</el-button>
      </el-col>
    </el-row>

    <el-table border v-loading="loading" :data="tableList">
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
        label="品牌方"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="区域/部门"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="对接人"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="PO"
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
      <el-table-column
        label="税前金额"
        align="center"
        prop="className"
        :show-overflow-tooltip="true"
        width="120"
      />
      <el-table-column
        label="税后金额"
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
      <el-table-column label="开票状态" align="center" prop="status">
        <!-- <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_common_status" :value="scope.row.status"/>
        </template> -->
      </el-table-column>
      <el-table-column label="收款状态" align="center" prop="status">
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
          <el-col :span="24">
            <el-form-item label="申请人：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="品牌方：" prop="noticeTitle">
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
            <el-form-item label="税号：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="地址电话：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="发票内容：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="附件：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="审核状态：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="开票状态：" prop="noticeTitle">
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
            <el-form-item label="发票：" prop="noticeType">
               <file-upload />
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="开票日期：">
              <el-date-picker
                v-model="dateRange"
                style="width: 240px"
                value-format="yyyy-MM-dd"
                type="daterange"
                range-separator="-"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
              ></el-date-picker>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="收款状态：">
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
            <el-form-item label="收款日期：">
              <el-date-picker
                v-model="dateRange"
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
        <p>票面开票日期:2024-02-27</p>
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
      this.$refs["form"].validate(valid => {
        if (valid) {
          this.open = false
        }
      })  
    },
    cancel(){
      this.open = false
    }
  }
};
</script>
