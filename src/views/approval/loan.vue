<template>
  <div class="app-container">
    <div class="main">
      <div class="title">借款审核</div>
      <el-row :gutter="10" class="mb8">
        <el-col :span="1.5">
          <el-button
            type="primary"
            plain
            size="mini"
            :disabled="loanMultiple"
            v-hasPermi="['tool:gen:code']"
          >通过</el-button>
        </el-col>
        <el-col :span="1.5">
          <el-button
            type="danger"
            plain
            size="mini"
            :disabled="loanMultiple"
            v-hasPermi="['tool:gen:import']"
          >驳回</el-button>
        </el-col>
      </el-row>

      <el-table v-loading="loanLoading" :data="loanTableList" @selection-change="handleSelectionLoanChange">
        <el-table-column type="selection" align="center" width="55"></el-table-column>
        <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryLoanParams.pageNum - 1) * queryLoanParams.pageSize + scope.$index + 1}}</span>
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
        <el-table-column label="操作" align="center" class-name="small-padding fixed-width" fixed="right">
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              icon="el-icon-view"
              @click="handleEditLoanTable(scope.row, 1)"
              v-hasPermi="['tool:gen:preview']"
            >查看</el-button>
          </template>
        </el-table-column>
      </el-table>
      <pagination
        v-show="loanTotal>0"
        :total="loanTotal"
        :page.sync="queryLoanParams.pageNum"
        :limit.sync="queryLoanParams.pageSize"
        @pagination="getLoanList"
      />
    </div>
    <div class="main">
      <div class="title">核销审核</div>
      <el-row :gutter="10" class="mb8">
        <el-col :span="1.5">
          <el-button
            type="primary"
            plain
            size="mini"
            :disabled="offMultiple"
            v-hasPermi="['tool:gen:code']"
          >通过</el-button>
        </el-col>
        <el-col :span="1.5">
          <el-button
            type="danger"
            plain
            size="mini"
            :disabled="offMultiple"
            v-hasPermi="['tool:gen:import']"
          >驳回</el-button>
        </el-col>
      </el-row>

      <el-table v-loading="offLoading" :data="writeOffTableList" @selection-change="handleSelectionOffChange">
        <el-table-column type="selection" align="center" width="55"></el-table-column>
        <el-table-column label="序号" type="index" width="50" align="center">
          <template slot-scope="scope">
            <span>{{(queryWriteOffParams.pageNum - 1) * queryWriteOffParams.pageSize + scope.$index + 1}}</span>
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
        <el-table-column label="操作" align="center" class-name="small-padding fixed-width" fixed="right">
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              icon="el-icon-view"
              @click="handleEditOffTable(scope.row, 1)"
              v-hasPermi="['tool:gen:preview']"
            >查看</el-button>
          </template>
        </el-table-column>
      </el-table>
      <pagination
        v-show="writeOffTotal>0"
        :total="writeOffTotal"
        :page.sync="queryWriteOffParams.pageNum"
        :limit.sync="queryWriteOffParams.pageSize"
        @pagination="getWriteOffList"
      />
    </div>
    <!-- 借款查看 -->
    <el-dialog :title="title" :visible.sync="openLoan" width="780px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="100px">
        <el-row>
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
              <el-form-item label="备注：" prop="noticeTitle">
              </el-form-item>
            </el-col>
          </template>
          <el-col :span="8">
            <el-form-item label="借款对象：" prop="noticeTitle">
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="借款金额：" prop="noticeTitle">
            </el-form-item>
          </el-col>
        </el-row>  
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancelLoan">取 消</el-button>
        <el-button type="primary" @click="submitLoanForm(1)">通过</el-button>
        <el-button  type="danger" @click="submitLoanForm(2)">驳回</el-button>
      </div>
    </el-dialog>
    <el-dialog :title="title" :visible.sync="openOff" width="790px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="100px">
        <el-row>
          <el-col :span="24">
            <el-form-item label="借款对象：" prop="noticeTitle">
            </el-form-item>
          </el-col>
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
              <el-radio-group v-model="form.status">
                <!-- <el-radio
                  v-for="dict in dict.type.sys_notice_status"
                  :key="dict.value"
                  :label="dict.value"
                >{{dict.label}}</el-radio> -->
              </el-radio-group>
            </el-form-item>
          </el-col>
          
          <el-col :span="8">
            <el-form-item label="审核状态">

            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancelOff">取 消</el-button>
        <el-button type="primary" @click="submitOffForm(1)">通过</el-button>
        <el-button  type="danger" @click="submitOffForm(2)">驳回</el-button>
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
      loanLoading: false,
      offLoading: false,
      // 唯一标识符
      uniqueId: "",
      // 选中数组
      ids: [],
      // 选中借款表数组
      tableLoanData: [],
      // 选中核销表数组
      tableOffData: [],
      // 借款总条数
      loanTotal: 0,
      // 核销总条数
      writeOffTotal: 0,
      // 表数据
      loanTableList: [{id:1}],
      writeOffTableList: [{id:1}],
      // 日期范围
      dateRange: "",
      // 查询参数
      queryLoanParams: {
        pageNum: 1,
        pageSize: 10,
      },
      queryWriteOffParams: {
        pageNum: 1,
        pageSize: 10,
      },
      // 预览参数
      form: {},
      rules: {},
      openLoan: false,
      openOff: false,
      loanMultiple: false,
      offMultiple: false,
      title: '查看'
    };
  },
  created() {
    this.getLoanList();
    this.getWriteOffList();
  },
  activated() {
    const time = this.$route.query.t;
    if (time != null && time != this.uniqueId) {
      this.uniqueId = time;
      this.queryLoanParams.pageNum = Number(this.$route.query.pageNum);
      this.queryWriteOffParams.pageNum = Number(this.$route.query.pageNum);
      this.getLoanList();
      this.getWriteOffList()
    }
  },
  methods: {
    /** 查询借款表集合 */
    getLoanList() {
      // this.loading = true;
    },
    /** 查询核销表集合 */
    getWriteOffList() {
      // this.loading = true;
    },
    /** 预览按钮 */
    handleEditLoanTable(row, state) {
      this.openLoan = true
      this.form = row
    },
    /** 预览按钮 */
    handleEditOffTable(row, state) {
      this.openOff = true
      this.form = row
    },
    // 多选框选中借款数据
    handleSelectionLoanChange(selection) {
      this.ids = selection.map(item => item.tableId);
      this.tableLoanData = selection.map(item => item.tableName);
      this.single = selection.length != 1;
      this.loanMultiple = !selection.length;
    },
    // 多选框选中核销数据
    handleSelectionOffChange(selection) {
      this.ids = selection.map(item => item.tableId);
      this.tableOffData = selection.map(item => item.tableName);
      this.single = selection.length != 1;
      this.offMultiple = !selection.length;
    },
    //借款提交
    submitLoanForm(state){
      this.openLoan = false
    },
    //核销提交
    submitOffForm(state){
      this.openOff = false
    },
    cancelLoan(){
      this.openLoan = false
    },
    cancelOff(){
      this.openOff = false
    }
  }
};
</script>
<style lang="scss" scoped>
.main{
  margin-bottom: 20px;
  .title{
    width: 100%;
    text-align: center;
  }
}
</style>
