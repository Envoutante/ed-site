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
            投资金额
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            年化利率
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            预期收益
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            实际收益
          </span>
          <span class="pmain-title2 fb" style="width: 100px;">
            投资状态
          </span>
          <span class="pmain-title1 fb" style="width: 150px;">
            投资时间
          </span>
        </div>
        <ul>
          <li
            v-for="lendItem in lendItemList"
            :key="lendItem.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 100px;">
              {{ lendItem.param.lendTitle }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItem.investAmount }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItem.lendYearRate }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItem.expectAmount }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItem.realAmount }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 100px;">
              {{ lendItem.param.status }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 150px;">
              {{ lendItem.investTime }}
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
      lendItemList: [],
    }
  },

  created() {
    this.fetchLendItemList()
  },

  methods: {
    fetchLendItemList() {
      this.$axios.$get('/api/core/lendItem/show').then((res) => {
        this.lendItemList = res.data.lendItemList
      })
    },
  },
}
</script>
