<template>
  <section>
    <h2>Commodity Exchange Rates</h2>
    <p>
      
    </p>
  </section>
</template>

<script>
export default {
  name: 'CommodityExchangeRates',
  props: {
    commodities: {
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
</style>
