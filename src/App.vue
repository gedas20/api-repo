<template>
  <h1>Exchange Rates To BTC</h1>
  <div class="flex-container">
    <div>
      <ExchangeRates
        title="Crypto"
        :currencies="cryptoExchangeRateData"
      />
    </div>
    <div>
      <ExchangeRates
        title="Fiat"
        :currencies="fiatExchangeRateData"
      />
    </div>
    <div>
      <ExchangeRates
        title="Commodity"
        :currencies="commodityExchangeRateData"
      />
    </div>
  </div>
</template>

<script>
  import ExchangeRates from './components/ExchangeRates.vue'

  export default {
    name: 'App',
    components: {
      ExchangeRates,
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
              this.apiDataLoaded = true;
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
  .flex-container {
    display: flex;
    background-color: #fff;
  }
  .flex-container > div {
    flex: 1;
    background: #fff;
    padding: .5em;
    margin: .5em;
  }
</style>
