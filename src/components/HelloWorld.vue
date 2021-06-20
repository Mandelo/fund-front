<template>
  <div class="hello">
    <el-container>
      <el-header>demo</el-header>
      <el-main>
        <div class="stock-holding">
          <div class="search-wrap">
            <el-row>
              <el-input
                placeholder="请输入基金编码"
                v-model="fundCode"
                style="width:360px;float:left"
                suffix-icon="el-icon-search"
              />
              <el-button
                type="success"
                style="width:100px;float:left;margin-left:20px"
                @click="queryStockHolding"
              >查询持仓</el-button>
            </el-row>
          </div>
          <transition name="plus-icon">
            <el-card
              class="box-card"
              v-if="visable"
            >
              <div
                slot="header"
                class="clearfix"
              >
                <span>前十大持仓</span>
              </div>
              <div class="content-wrap">
                <span class="data-span">
                  <b>
                    股票名称
                  </b>
                </span>
                <span class="data-span">
                  <b>
                    持仓占比
                  </b>
                </span>
                <span class="data-span">
                  <b>
                    持仓变化
                  </b>
                </span>
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
                  <span v-bind:class="item.percentChange.indexOf('-') == -1 && item.percentChange.indexOf('--') == -1? textRed : textGreen">
                    {{item.percentChange}}
                  </span>
                </div>
              </div>
            </el-card>
          </transition>
        </div>

        <div class="stock-analy">
          <div class="search-wrap">
            <el-row>
              <el-select
                placeholder="请选择基金组合"
                v-model="combinationId"
                style="width:360px;float:left"
              >
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                  :disabled="item.disabled"
                >
                </el-option>

              </el-select>
              <el-button
                @click="queryComboInfo"
                style="float: right;"
                type="primary"
              >分析持仓</el-button>

            </el-row>
          </div>
          <transition name="plus-icon">
            <el-card
              class="box-card"
              v-if="comboVisiable"
            >
              <div
                slot="header"
                class="clearfix"
              >
                <span>组合股票分析</span>
              </div>
              <div class="content-wrap">
                <span class="data-span-2">
                  <b>
                    股票名称
                  </b>
                </span>
                <span class="data-span-2">
                  <b>
                    存在数量
                  </b>
                </span>
              </div>
              <div style="overflow:auto"></div>
              <div
                v-for="item in comboInfoList"
                :key="item.stockName"
              >
                <div class="content-wrap">
                  <span class="data-span-2">
                    {{item.stockName}}
                  </span>
                  <span class="data-span-2">
                    {{item.stockCount}}
                  </span>
                </div>
              </div>
            </el-card>
          </transition>
        </div>
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
      let fundCode = this.fundCode;
      request({
        url: "/api/fund/stockHolding/" + fundCode,
        method: 'get'
      }).then(res => {
        console.info(res)
        if (res.code === "200") {
          if (res.data) {
            this.visable = true;
            this.stockHoldingList = res.data;
            console.info(res)
          } else {
            this.$message({
              message: '未查询到相关信息',
              type: 'warning'
            });
          }
        }
      })

    },
    queryComboInfo() {
      request({
        url: "/api/fund/stockHolding/queryCombInfo",
        method: 'get'
      }).then(res => {
        console.info(res)
        if (res.code === "200") {
          if (res.data) {
            this.comboVisiable = true;
            this.comboInfoList = res.data;
            console.info(res)
          } else {
            this.$message({
              message: '未查询到相关信息',
              type: 'warning'
            });
          }
        }
      })

    }
  },
  data() {
    return {
      fundCode: '',
      visable: false,
      comboVisiable: false,
      stockHoldingList: [],
      textRed: "textRed",
      textGreen: "textGreen",
      options: [{
        value: '一定要回本',
        label: '一定要回本'
      }],
      combinationId: ''
    }
  },

}
</script>

<style scope>
.box-card {
  width: 480px;
  padding-bottom: 20px;
  margin-top: 20px;
  max-height: 400px;
}

.stock-holding {
  width: 480px;
  float: left;
}

.stock-analy {
  width: 480px;
  float: left;
  margin-left: 20px;
  max-height: 400px;
}

.stock-holding .search-wrap {
  width: 480px;
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

.data-span-2 {
  width: 200px;
  height: 30px;
  line-height: 30px;
  float: left;
  text-align: center;
}

.textRed {
  color: red;
  width: 140px;
  height: 30px;
  line-height: 30px;
  float: left;
  text-align: center;
}

.textGreen {
  color: green;
  width: 140px;
  height: 30px;
  line-height: 30px;
  float: left;
  text-align: center;
}

.plus-icon-enter-active {
  transition: opacity 1s;
}
.plus-icon-enter {
  opacity: 0;
}
.plus-icon-leave-active {
  transition: opacity 1s;
}
.plus-icon-enter-to {
  opacity: 1;
}

.plus-icon-leave-to {
  opacity: 0;
}
</style>