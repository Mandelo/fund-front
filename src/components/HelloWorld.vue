<template>
  <div class="hello">
    <el-container>
      <el-header>Header</el-header>
      <el-main>
        <el-card class="box-card">
          <div
            slot="header"
            class="clearfix"
          >
            <span>前十大持仓</span>
            <el-button
              @click="queryStockHolding"
              style="float: right; padding: 3px 0"
              type="text"
            >分析持仓</el-button>
          </div>
          <div
            v-for="item in stockHoldingList"
            :key="item.stockName"
          >
            <div class="content-wrap">
              <span class="data-span">
                {{item.stockName}}

              </span>
              <span class="data-span">
                {{item.currentPercent}}
              </span>
              <span class="data-span">

                {{item.percentChange}}
              </span>
            </div>
          </div>
        </el-card>
      </el-main>
      <el-footer></el-footer>
    </el-container>
  </div>
</template>

<script>
import request from '@/utils/request'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  mounted() {
  },
  methods: {
    queryStockHolding() {
      request({
        url: "/api/fund/stockHolding/005827",
        method: 'get'
      }).then(res => {
        console.info(res)
        if (res.code === "200") {
          this.stockHoldingList = res.data;
          console.info(res)
        }
      })

    }
  },
  data() {
    return {
      stockHoldingList: []
    }
  },

}
</script>

<style scope>
.box-card {
  width: 480px;
  padding-bottom: 20px;
}

.content-wrap {
  width: 420px;
  margin-left: 30px;
}

.el-header {
  background-color: #b3c0d1;
  color: #333;
  text-align: left;
  line-height: 60px;
}

.data-span {
  width: 140px;
  height: 30px;
  line-height: 30px;
  float: left;
  text-align: center;
}
</style>