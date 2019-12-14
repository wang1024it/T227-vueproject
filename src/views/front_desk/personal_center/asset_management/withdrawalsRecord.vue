<template>
  <div  style="padding: 20px;background-color: #FFFFFF">

    <el-row>
      <el-col style="font-weight: bold;" :span="12" >申请时间
        <el-date-picker
          v-model="queryParams.applyTime"
          type="daterange"
          align="right"
          unlink-panels
          range-separator="To"
          start-placeholder="Start date"
          end-placeholder="End date"
          :picker-options="pickerOptions2">
        </el-date-picker>
        <el-button type="primary" icon="el-icon-search" @click="onQuery">查询</el-button>
      </el-col>

    </el-row>

    <el-table :data="dictList"  height="520" :fit="true" :show-header="true" v-loading="loading">
      <el-table-column prop="id" label="ID" min-width="2">
      </el-table-column>
      <el-table-column value-format="yyyy-MM-dd-HH-mm-ss" prop="applyTime" label="申请时间" min-width="4">
      </el-table-column>
      <el-table-column prop="amount" label="提现金额" min-width="4">
      </el-table-column>
      <el-table-column prop="free" label="手续费" min-width="4">
      </el-table-column>
      <el-table-column prop="state_dictText" label="状态" min-width="4">
      </el-table-column>
    </el-table>
    <el-pagination style="margin: 15px;" background @size-change="handleSizeChangeDictItem" @current-change="handleCurrentChangeDictItem"
                   :current-page="queryParams.page" :page-sizes="[5, 10, 15, 20]" :page-size="queryParams.rows" layout="total, sizes, prev, pager, next, jumper"
                   :total="queryParams.total">
    </el-pagination>
  </div>

</template>

<script>
    import commonUtils from "../../../../api/commonUtils";

    export default {
        name: "withdrawalsRecord",
        data:function(){
            return {
                //将日期回显到表格上
                pickerOptions2: {
                    shortcuts: [{
                        text: 'Last week',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: 'Last month',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: 'Last 3 months',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                            picker.$emit('pick', [start, end]);
                        }
                    }]
                },
                value:"",
                //这是查询参数
                queryParams: {
                    membersId:null,
                    //定义搜索维度
                    applyTime: null,
                    // 分页
                    page: 1,
                    rows: 10,
                    total: 0,

                },
                //这是表格是否占时加载动画
                loading: false,
                //这书数据对象（用于表格占时）
                dictList: [],
                //这是新值修改弹出
                dictDialogTitle: null,
                //这是新值和修改弹出是否占时
                dictDialogFormVisible: false,
                //这是文本框中文字说明的长度
                formLabelWidth: "100px",
                //绑定数据
                recordList: [{
                    titleid:null,
                    applyTime:null,
                    name:null,
                    membersId: null,
                    tradeCode: null,
                    applyTime: null,
                    amount: null
                }],
            }
        },
        methods:{

            onQuery() {
                this.queryParams.page = 1;
                this.search();

            },
            //这是搜索加展示数据的方法
            search: function() {
                let url = this.axios.urls.ASSETS_MONEYWITHDRAW_QUERYPAGER;
                let params = {
                    id:this.queryParams.titleid,
                    applyTime:this.queryParams.applyTime,
                    page:this.queryParams.page,
                    rows:this.queryParams.rows,
                    total:this.queryParams.total
                }
                //这是处理开始时间和结束时间
                if(this.queryParams.applyTime && this.queryParams.applyTime.length > 1){
                    params.applyTimeStart = commonUtils.formatDate(this.queryParams.applyTime[0], 'yyyy/MM/dd')
                    params.applyTimeEnd = commonUtils.formatDate(this.queryParams.applyTime[1], 'yyyy/MM/dd')
                }
                //查询动画
                this.loading = true;
                //向后端请求数据
                this.axios.get(url, {params: params}).then(response => {
                    this.dictList = response.data.data
                    this.queryParams.total = response.data.total;
                    //数据查询到了关闭查询动画
                    this.loading = false;
                }).catch(function (error) {
                    console.log(error);
                });
            },
            //一页的数量发送变化的时候调用此方法
            handleSizeChangeDictItem: function(rwos) {
                this.queryParams.page = 1;
                this.queryParams.rows = rwos;
                this.search();
            },
            //当前页面发送变化的时候调用
            handleCurrentChangeDictItem: function(page) {
                this.queryParams.page = page;
                this.search();
            }
        },
        created() {
            //加载用户id
            this.queryParams.membersId  = this.$store.getters.getUser.id
            this.onQuery();
        }
    }
</script>

<style scoped>

</style>
