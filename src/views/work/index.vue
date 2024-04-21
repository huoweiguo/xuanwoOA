<template>
  <div class="app-container">
    <!-- 项目经理权限 -->
    <div>
        <div>
          <h3>项目付款</h3>
          <el-table
            v-loading="loading"
            :data="payList"
            row-key="menuId"
            border=""
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="id" label="项目编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="name" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="服务内容" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center">
              <template slot-scope="scope">
                <dict-tag :options="dict.type.sys_notice_status" :value="scope.row.status"/>
              </template>
            </el-table-column>
            <el-table-column prop="" label="付款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商发票" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="date" label="票面开票日期" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
          <h3>项目待开票</h3>
          <el-table
            v-loading="loading"
            row-key="menuId"
            border=""
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="品牌方" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="区域/部门" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="对接人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="PO" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税前金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税后金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="开票状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="收款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
          <h3>借款待核销</h3>
          <el-table
            v-loading="loading"
            row-key="menuId"
            border
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="借款编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="核销金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票金额（含冲票）" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="补齐/退回" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
    </div>
    <!-- 老板权限 -->
    <div>
      <div>
          <h3>项目待审批</h3>
          <el-table
            v-loading="loading"
            :data="payList"
            row-key="menuId"
            border
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="id" label="项目编号" :show-overflow-tooltip="true" align="center" header="60"></el-table-column>
            <el-table-column prop="name" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="服务内容" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="付款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center">
              <template slot-scope="scope">
                <dict-tag :options="dict.type.sys_notice_status" :value="scope.row.status"/>
              </template>
            </el-table-column>
            <el-table-column prop="" label="付款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商发票" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
          <h3>开票待审批</h3>
          <el-table
            v-loading="loading"
            row-key="menuId"
            border=""
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="品牌方" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="区域/部门" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="对接人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="PO" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税前金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税后金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
        <h3>借款待审批</h3>
        <el-table
            v-loading="loading"
            row-key="menuId"
            border
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="借款编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
        <h3>核销待审批</h3>
        <el-table
            v-loading="loading"
            row-key="menuId"
            border=""
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="借款编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="核销金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票金额（含冲票）" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
         </el-table>
      </div>
    </div>
    <!-- 财务权限 -->
    <div>
      <div>
          <h3>制单待完成</h3>
          <el-table
            v-loading="loading"
            :data="payList"
            row-key="menuId"
            border
            width="200"
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="付款日期" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="id" label="项目编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="name" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="服务内容" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="付款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center">
              <template slot-scope="scope">
                <dict-tag :options="dict.type.sys_notice_status" :value="scope.row.status"/>
              </template>
            </el-table-column>
            <el-table-column prop="" label="付款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商发票" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="票面开票时间" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
          <h3>开票待完成</h3>
          <el-table
            v-loading="loading"
            row-key="menuId"
            border
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="品牌方" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="项目名称" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="区域/部门" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="对接人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="PO" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税前金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="税后金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="开票状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="收款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
      <div>
        <h3>借款核销待完成</h3>
        <el-table
            v-loading="loading"
            row-key="menuId"
            border=""
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="借款编号" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="核销金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="借款/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票金额（含冲票）" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="发票/核销差额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="补齐/退回" :show-overflow-tooltip="true" align="center"></el-table-column>
         </el-table>
      </div>
      <div>
        <h3>行政付款申请</h3>
        <el-table
            v-loading="loading"
            row-key="menuId"
            border
            :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
          >
            <el-table-column prop="" label="项目名称" :show-overflow-tooltip="true" width="160" align="center"></el-table-column>
            <el-table-column prop="" label="服务内容" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="付款金额" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="供应商" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="申请人" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="审核状态" :show-overflow-tooltip="true" align="center"></el-table-column>
            <el-table-column prop="" label="付款状态" :show-overflow-tooltip="true" align="center"></el-table-column>
          </el-table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  dicts: ['sys_notice_status'],
  data() {
    return {
      
      loading:false,
      payList:[
        {
          id:'111',
          name:'LV1月活动',
          status:0,
          date:'2020-11-02'
        },
        {
          id:'222',
          name:'LV2333月活动',
          status:1,
          date:'2020-11-03'
        },
      ],
      ticketList:[],
      borrowList:[]
    }
  }
}
</script>

<style scoped lang="scss">
.flex{
  display: flex;
}
</style>