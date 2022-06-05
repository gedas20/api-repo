<template>
  <table>
  <thead>
  <tr>
    <th colspan="3">{{title}}</th>
  </tr>
  <tr> 
    <th>Name</th>
    <th>Value In BTC</th>
  </tr>
  </thead>
  <tbody>
  <tr v-if="currencies.length == 0">
    <td colspan="3">No data available.</td>
  </tr>
  <tr v-for="currency in currencies" :key="currency.unit">
    <td><a href="#" @click="showSymbol(currency.unit)">{{currency.name}}</a></td>
    <td class="num">{{currency.value}}</td>
  </tr>
  </tbody>
  </table>
</template>

<script>
  export default {
    name: 'ExchangeRates',
    emits: ['showSymbolEvent'],
    props: {
      title: {
          type: String,
          required: true
      },
      currencies: {
          type: Array,
          required: true
      }
    },
    data() {
      return {
        test: '',
      }
    },
    methods : {
      showSymbol(currencyName) {
          this.$emit("showSymbolEvent", currencyName);
      }
    }
  }
</script>

<style scoped>
  table {
      width: 100%;
      margin-bottom: 20px;
      border-spacing: 0;
      border-collapse: collapse;
      border: 1px solid #ddd;
  }
  td, th {
      padding: 7px 5px !important;
      border: 1px solid #ddd;
      text-align: left;
      vertical-align: middle;
      color: #333;
      font-size: 13px;
  }
  th {
    background-color:#337ab7;
    color: white;
  }
  thead>:nth-child(2) th  {
    background-color:#d9edf7;
    color: #333;
  }
  .num {
    text-align: right;
  }
  a {
    color: #337ab7;
  }
</style>
