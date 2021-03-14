<template>
  <div class="container mt-5">
    <loading :active.sync="isLoading"></loading>
    <div class="d-flex justify-content-center">
      <h4>選擇交易貨幣：</h4>
      <select v-model="currency1">
        <option :value="keys" v-for="(value, keys) in allData" :key="value.id">
          {{ keys }}
        </option>
      </select>
    </div>
    <div class="d-flex justify-content-center mt-3">
      <h4>選擇兌換貨幣：</h4>
      <select v-model="currency2">
        <option value=""> ------ </option>
        <option :value="keys" v-for="(value, keys) in allData" :key="value.id" >
          {{ keys }}
        </option>
      </select>
    </div>
    <div class="text-center mt-4">
      <button class="btn btn-primary px-5" @click="getTwoData(currency1, currency2)">換算</button>
    </div>
    <!-- 計算結果 -->
    <div class="context text-center mt-4" v-if="getApi">
      <div class="show">
        <span v-if="value <= 0"> 1 </span>
        <span v-else>{{ value }}</span>
        <span>{{ baseCurrency }} = {{ totalMoney }} {{ targetCurrency }}</span>
      </div>
      <div class="input-group justify-content-center mt-3">
        <div class="input-group-prepend">
          <span class="input-group-text" id="inputGroup-sizing-default">金額
          </span>
        </div>
        <input type="text" v-model="value">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      allData: {},
      currency1: '',
      currency2: '',
      baseCurrency: '', // 獲獲的資料
      targetCurrency: '',
      value: 1, // 顯示預設值
      conversionRate: 0, // 匯率
      getApi: false, // 判斷有沒有計算
      nextUpdate: '', // 更新時間
      lastUpdate: '',
      isLoading: false,
    };
  },
  methods: {
    getData() {
      const url = 'https://v6.exchangerate-api.com/v6/018d4f58673496b7d8c8916e/latest/TWD';
      const vm = this;
      vm.isLoading = true;
      vm.axios.get(url).then((response) => {
        vm.currency1 = response.data.base_code;
        vm.allData = response.data.conversion_rates;
        vm.isLoading = false;
      });
    },
    getTwoData(currency1, currency2) {
      const url = `https://v6.exchangerate-api.com/v6/018d4f58673496b7d8c8916e/pair/${currency1}/${currency2}/`;
      const vm = this;
      vm.isLoading = true;
      vm.axios.get(url).then((response) => {
        vm.baseCurrency = response.data.base_code;
        vm.targetCurrency = response.data.target_code;
        vm.conversionRate = response.data.conversion_rate;
        vm.lastUpdate = response.data.time_last_update_utc;
        vm.nextUpdate = response.data.time_next_update_utc;
        vm.getApi = true;
        vm.isLoading = false;
        // console.log(vm.baseCurrency, vm.targetCurrency, vm.conversionRate);
      });
    },
  },
  computed: {
    totalMoney() {
      if (this.value <= 0) {
        return (1 * this.conversionRate).toFixed(2);
      }
      return (this.value * this.conversionRate).toFixed(2);
    },
  },
  created() {
    this.getData();
  },
};
</script>

<style lang="scss">
@import "~@/assets/scss/index";
</style>
