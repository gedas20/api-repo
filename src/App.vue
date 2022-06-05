<template>
  <CryptoExchangeRates
    :cryptoCurrencies="cryptoExchangeRateData"
  />
 
  <FiatExchangeRates
    :fiatCurrencies="fiatExchangeRateData"
  />

  <CommodityExchangeRates
    :commodities="commodityExchangeRateData"
  />

</template>

<script>
  import CryptoExchangeRates from './components/CryptoExchangeRates.vue'
  import FiatExchangeRates from './components/FiatExchangeRates.vue'
  import CommodityExchangeRates from './components/CommodityExchangeRates.vue'

  export default {
    name: 'App',
    components: {
      CryptoExchangeRates,
      FiatExchangeRates,
      CommodityExchangeRates
    },
    data() {
      return {
          fiatExchangeRateData: [],
          cryptoExchangeRateData: [],
          commodityExchangeRateData: [],
          apiDataLoaded: false,
          error: ''
      }
    },
    methods: {
      loadApiData() {
        fetch('https://api.coingecko.com/api/v3/exchange_rates')
          .then(response => {
            if (response.ok) {
              return response.json();

            } else {

              this.error = 'Failed to fetch data.';
            }
          })
          .then(data => {
            if (data.rates) {
    
              const objRates = data.rates;

              for (const property in objRates) {

                let objCurrency = objRates[property];

                if (objCurrency['type'] == 'crypto') {

                   this.cryptoExchangeRateData.push(objCurrency);

                } else if (objCurrency['type'] == 'fiat') {

                  this.fiatExchangeRateData.push(objCurrency);

                } else if (objCurrency['type'] == 'commodity') {

                   this.commodityExchangeRateData.push(objCurrency);
                }
              }
            } else {

              this.error = 'Failed to fetch data.';
            }
          })
          .catch(() => {

              this.error = 'Failed to fetch data.';
          });
      },
    },
    mounted() {
      this.loadApiData();
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
