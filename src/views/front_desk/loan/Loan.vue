<template>
    <el-main>
      <el-row :gutter="9">
        <el-col :span="12">
          <el-card header="信用贷">
            <i style="color: rgb(250, 212, 0);">认证最高额度 5000 ￥</i>
            <div style="margin-top: 9px;">
              <span style="color: rgb(0, 206, 209); ">贷款条件:</span>
              <ul>
                <li>基本资料填写</li>
                <li>身份认证通过 </li>
                <li>资料认证完善</li>
              </ul>
            </div>
            <p align="center" style="margin-top: 60px;">
              <!--信用贷  creditting    -->
              <el-tooltip placement="top" effect="light">
                <div slot="content">
                  个人信用贷款:<br/><br/>
                  贷款申请人无需任何抵押或担保，只需凭个人信用状况，即可从平台获得贷款。<br/><br/>
                  信誉越高，贷款额度也会有所提高。<br/><br/>
                  贷款用途:小额信用消费贷款主要用于借款人正常消费需求及劳务等费用支付。<br/><br/>
                  贷款限额:小额信用消费贷款的最高限额为10000元人民币。<br/><br/>
                  贷款期限:小额信用消费贷款的最长期限为一年。<br/><br/>
                </div>
                <el-button type="default" icon="el-icon-warning-outline" round>查 看 详 情</el-button>
              </el-tooltip>
              <el-button round icon="el-icon-thumb" style="color: rgb(255, 255, 255); background-color: rgb(1, 170, 237);"   @click="toLoanApplication('/CreditLoan')" plain>申 请 信 用 贷</el-button>
            </p>
          </el-card>
        </el-col>

      </el-row>
    </el-main>
</template>

<script>
    import commonUtils from "../../../api/commonUtils";
    export default {
        name: "Loan",
        methods:{
            //这是跳转到对应的表单申请页面
            toLoanApplication(toRouter){
                commonUtils.toRouter(toRouter)
            }
        },
        created() {
            commonUtils.init(this);
        }

    }
</script>

<style scoped>

</style>
