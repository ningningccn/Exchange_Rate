<template>
  <div class="container text-center my-4">
    <loading :active.sync="isLoading"></loading>
    <h1>所有匯率</h1>
    <p>顯示貨幣：1元 {{ currency }}</p>
    <span>切換顯示貨幣：</span>
    <select v-on:change="changeCurrency(currency)" v-model="currency">
      <option :value="key" v-for="(value,key) in exchangeRateData" :key="key" >
        {{ key }}
      </option>
    </select>
    <div class="update d-flex justify-content-between text-secondary mt-4">
      <div class="lastUpdate">
        <h6>最後更新</h6>
        <h6 class="text-danger mb-0">{{ lastUpdate }}</h6>
      </div>
      <div class="nextUpdate">
        <h6>下次更新</h6>
        <h6 class="text-success">{{ nextUpdate }}</h6>
      </div>
    </div>
    <table class="table table-striped table-hover">
      <thead class="thead-dark">
        <tr>
          <td>貨幣</td>
          <td>匯率</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(value, key) in exchangeRateData" :key="value.id">
          <td>{{ key }}</td>
          <td>{{ value }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      money: 1, // 顯示價值
      currency: '', // 顯示貨幣
      exchangeRateData: {}, // 匯率
      lastUpdate: '',
      nextUpdate: '',
      isLoading: false,
    };
  },
  methods: {
    getExchangeRate() {
      const url = 'https://v6.exchangerate-api.com/v6/018d4f58673496b7d8c8916e/latest/TWD';
      const vm = this;
      vm.isLoading = true;
      vm.axios.get(url).then((response) => {
        vm.currency = response.data.base_code;
        vm.selectExchangeRateDate = response.data.conversion_rates;
        vm.exchangeRateData = response.data.conversion_rates;
        vm.lastUpdate = response.data.time_last_update_utc;
        vm.nextUpdate = response.data.time_next_update_utc;
        // delete vm.exchangeRateData[Object.keys(vm.exchangeRateData)[0]];
        vm.isLoading = false;
      });
    },
    changeCurrency(currency) {
      const url = `https://v6.exchangerate-api.com/v6/018d4f58673496b7d8c8916e/latest/${currency}`;
      const vm = this;
      vm.isLoading = true;
      vm.axios.get(url).then((response) => {
        vm.currency = response.data.base_code;
        vm.exchangeRateData = response.data.conversion_rates;
        vm.isLoading = false;
      });
    },
  },
  created() {
    this.getExchangeRate();
  },
};
</script>

<style lang='scss'>
@import '~@/assets/scss/all';
</style>
