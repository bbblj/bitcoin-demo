<template>
  <div id="app">
    <div class="title">Bitcoin Price</div>
    <div class="content">
      <div v-show="errored">We're sorry, we're not able to retrieve this information at the moment, please try back later</div>
      <div v-show="loading">Loading...</div>
      <div v-for="currency in info" class="currency">
        {{ currency.description }}:
        <span class="lighten">
      <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
    </span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      info: null,
      loading:true,
      errored:false
    }
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted() {
    this.$axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
            .then(response => (this.info = response.data.bpi))
            .catch(error => {
              console.log(error)
              this.errored = true
            })
            .finally(() => this.loading = false)
  }
}
</script>

<style>
  #app{
    text-align: center;
    align-items: center;
    width: 500px;
    height: 200px;
    margin: 10px auto;
    background-color: saddlebrown;
    border-radius: 20px;
  }
  .title{
    font-size: 30px;
    font-weight: 800;
    padding: 30px 0px 10px;
    color: aliceblue;
  }
  .content{
    margin-left: 100px;
  }
  .currency{
    width: 300px;
    text-align: left;
    font-weight: 600;
  }
  .lighten{
    color: aliceblue;
  }

</style>
