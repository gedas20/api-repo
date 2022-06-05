<template>
  <section>
    <h2>{{title}} Exchange Rates</h2>
    <p>
      <table>
      <thead>
      <tr> 
        <th>Name</th>
        <th>Symbol</th>
        <th>Value In BTC</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="currency in currencies" :key="currency.unit">
        <td>{{currency.name}}</td>
        <td>{{currency.unit}}</td>
        <td>{{currency.value}}</td>
      </tr>
      </tbody>
      </table>
    </p>
  </section>
</template>

<script>
export default {
  name: 'ExchangeRates',
  props: {
    title: {
        type: String,
        required: true
    },
    currencies: {
        type: Array,
        required: true
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
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
    text-align: center;
    vertical-align: middle;
    color: #333;
    height: 15px;
}
th {
    background-color:#337ab7;
    color: white;
}
thead>:nth-child(2) th  {
    background-color:#d9edf7;
    color: #333;
}
</style>
