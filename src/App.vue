<template>
  <h1>Exchange Rates To BTC</h1>
  <div class="box error" v-if="apiDataLoaded && error !== ''"> 
      {{ error }}
  </div>
  <div v-else v-cloak>
    <div class="flex-container">
      <div>
        <ExchangeRates
          title="Crypto"
          :currencies="cryptoExchangeRateData"
          v-on:showSymbolEvent="showSymbolName"
        />
      </div>
      <div>
        <ExchangeRates
          title="Fiat"
          :currencies="fiatExchangeRateData"
          v-on:showSymbolEvent="showSymbolName"
        />
      </div>
      <div>
        <ExchangeRates
          title="Commodity"
          :currencies="commodityExchangeRateData"
          v-on:showSymbolEvent="showSymbolName"
        />
      </div>
      <div>
        <ExchangeRates
          title="Other"
          :currencies="otherExchangeRateData"
          v-on:showSymbolEvent="showSymbolName"
        />
      </div>
    </div>
  </div>
  <div>
    <Bar
        v-if="apiDataLoaded"
        :chart-options="getBarChartOptions('Crypto')"
        :chart-data="cryptoDataSet"
        :height="100"
    />
    <Bar
        v-if="apiDataLoaded"
        :chart-options="getBarChartOptions('Fiat')"
        :chart-data="fiatDataSet"
        :height="100"
    />
    <Bar
        v-if="apiDataLoaded"
        :chart-options="getBarChartOptions('Commodity')"
        :chart-data="commodityDataSet"
        :height="100"
      />
  </div>
</template>

<script>
  import ExchangeRates from './components/ExchangeRates.vue'
  import { Bar } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, BarElement, CategoryScale, LinearScale } from 'chart.js'

  ChartJS.register(Title, Tooltip, BarElement, CategoryScale, LinearScale)

  export default {
    name: 'App',
    components: {
      ExchangeRates,
      Bar
    },
    data() {
      return {
          cryptoExchangeRateData: [],
          fiatExchangeRateData: [],
          commodityExchangeRateData: [],
          otherExchangeRateData: [],
          apiDataLoaded: false,
          error: ''
      }
    },
    computed: {
      cryptoDataSet() {
        const labels = [];
        const data = [];
        
        for (const index in this.cryptoExchangeRateData) {
          const objComodity = this.cryptoExchangeRateData[index];
          if (!["sats", "μBTC", "XLM", "XRP", "EOS", "LINK", "DOT"].includes(objComodity.unit)) {
            labels.push(objComodity.unit);
            data.push(objComodity.value);
          }
        }
        
        return this.getBarChartDataSet(labels, data);
      },
      fiatDataSet() {
        const labels = [];
        const data = [];
        
        for (const index in this.fiatExchangeRateData) {
          const objComodity = this.fiatExchangeRateData[index];
          if (!["₫", "Rp", "K", "₩", "CLP$"].includes(objComodity.unit)) {
            labels.push(objComodity.unit);
            data.push(objComodity.value);
          }
        }
        
        return this.getBarChartDataSet(labels, data);
      },
      commodityDataSet() {
        const labels = [];
        const data = [];
        
        for (const index in this.commodityExchangeRateData) {
          const objComodity = this.commodityExchangeRateData[index];
          labels.push(objComodity.unit);
          data.push(objComodity.value);
        }
        
        return this.getBarChartDataSet(labels, data);
      }
    },
    methods: {
      getBarChartDataSet(labels, data) {
        return {
            labels: labels,
            datasets: [
                {
                    backgroundColor: '#337ab7',
                    data: data
                }
            ]
        }
      },
      getBarChartOptions(title) {
        return {
          plugins: {
            legend: {
              display: false,
            },
            title: {
              display: true,
              text: title
            }
          }
        }
      },
      showSymbolName(symbol) {
        alert(symbol);
      },
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
                } else {

                  this.otherExchangeRateData.push(objCurrency);
                }
              }
            } else {
              this.error = 'Failed to fetch data.';
            }
          })
          .catch(() => {
              this.error = 'Failed to fetch data.';
          });
      }
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
    margin-top: 20px;
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
  @media (max-width: 800px) {
    .flex-container {
      flex-direction: column;
    }
  }
  .box {
    position: relative;
    padding: 1rem 1rem;
    margin-bottom: 1rem;
    border: 1px solid transparent;
    border-radius: 0.25rem;
  }
  .error {
    color: #842029;
    background-color: #f8d7da;
    border-color: #f5c2c7;
  }
</style>
