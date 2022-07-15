<template>
  <div class="container">
    <div class="row">
      <h1 class="title">Coin Gecko Api</h1>

      <input type="text" name="" id="" class="form-control bg-dark text-light rounded-0 border-0 my-4"
        placeholder="Search Coin" @keyup="searchCoin()" v-model="textSearch">

      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td>
              {{ index + 1 }}</td>
            <td>
              <img :src="coin.image" alt="" class="me-2" />

              <span>{{ coin.name }}</span>
              <span class="ms-2 text-uppercase text-muted">{{ coin.symbol }}</span>
            </td>
            <td>
              ${{ coin.current_price }}
            </td>
            <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
              {{ Number(coin.price_change_percentage_24h).toFixed(2)
              }}%
            </td>
            <td>
              ${{ coin.total_volume.toLocaleString() }}
            </td>

          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24H Volume"],
      textSearch: ""
    }
  },
  async mounted() {
    console.log("Mounted")
    const res = await fetch("https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false")
    const data = await res.json()
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin() {
      console.log(this.textSearch)
      this.filteredCoins = this.coins.filter(coin => coin.name.toUpperCase().includes(this.textSearch.toUpperCase()) || coin.symbol.toUpperCase().includes(this.textSearch.toUpperCase()))
    }
  }
}
</script>

<style>
img {
  width: 2em;
}

.container .row .title {
  padding: 0;
}
</style>
