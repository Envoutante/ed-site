<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>投资记录</i></h3>

      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle" style="width:100%">
          <span class="pmain-title1 fb" style="width: 100px;">
            投资项目
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            当前期数
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            应得本金
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            应得利息
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            应得总和
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            是否逾期
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            逾期金额
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            回款状态
          </span>
        </div>
        <ul>
          <li
            v-for="lendItemReturn in lendItemReturnList"
            :key="lendItemReturn.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 100px;">
              {{ lendItemReturn.param.lendTitle }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.currentPeriod }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.principal }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.interest }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.total }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.overdue }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.overdueTotal }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItemReturn.param.status }}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lendItemReturnList: [],
    }
  },

  created() {
    this.fetchLendItemReturnList()
  },

  methods: {
    fetchLendItemReturnList() {
      this.$axios.$get('/api/core/lendItemReturn/show').then((res) => {
        this.lendItemReturnList = res.data.lendItemReturnList
      })
    },
  },
}
</script>
