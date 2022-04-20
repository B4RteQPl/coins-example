<template>
  <div v-if="!isLoading && coins.length">
    <CoinList :coins="coins"/>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import CoinList from './components/CoinList.vue'
import CoinpaprikaAPI from '@coinpaprika/api-nodejs-client'

export default defineComponent({
  name: 'App',
  components: {
    CoinList
  },
  data () {
    return {
      coins: [],
      isLoading: false
    }
  },
  async mounted () {
    await this.fetchCoins()
  },
  methods: {
    // todo move it to client class
    async fetchCoins () {
      this.isLoading = true

      const client = new CoinpaprikaAPI()

      return client.getAllTickers({
        quotes: ['USD', 'BTC', 'ETH']
      })
        .then(this.onFetchCoinsSuccess)
        .catch(this.onFetchCoinsError)
        .finally(this.onFetchCoinsFinish)
    },

    onFetchCoinsSuccess (coins) {
      this.coins = coins
    },

    onFetchCoinsError (error) {
      console.log(error)
    },

    onFetchCoinsFinish () {
      this.isLoading = false
    }
  }
})
</script>

<style lang="scss">
  body {
    background: #dcdcdc;
  }
</style>
