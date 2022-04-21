<template>
  <div class="coin-item">
    <div class="coin-item__left-wrapper">
      <div class="coin-item__favourite-wrapper">
        <!-- todo connect and change to button-->
        <img
          src="../assets/add_to_faves_icon.png"
          class="coin-item__coin-logo"
        />
      </div>
      <div class="coin-item__coin-logo-wrapper">
        <img
          src="../assets/coin_logo.png"
          class="coin-item__coin-logo"
        />
      </div>
      <div class="coin-item__coin-info-wrapper">
        <div class="coin-item__coin-name">
          {{ coin.name }}
        </div>
        <div class="coin-item__coin-symbol">
          {{ coin.symbol }}
        </div>
      </div>
    </div>
    <div class="coin-item__right-wrapper">
      <div
        v-for="(quote, name) in coin.quotes"
        :key="name"
      >
        <Quote
          :name="name"
          :quote-value="quote.price"
          :quote-change="quote.volume_24h_change_24h"
          class="coin-item__quote"
        />
      </div>

      <!-- todo handle better names to avoid NOT_SUPPORTED -->
      <Quote
        name="NOT_SUPPORTED"
        :quote-value="usdQuote.market_cap"
        :quote-change="usdQuote.market_cap_change_24h"
        class="coin-item__market-cap"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import Quote from './Quote.vue'

export default defineComponent({
  name: 'CoinItem',
  components: {
    Quote
  },
  props: {
    coin: {
      type: Object,
      required: true
    }
  },
  computed: {
    /**
     * return {Object}
     */
    usdQuote () {
      // todo not parsed to correct format and without checking if exists
      return this.coin.quotes.USD
    }
  }
})
</script>

<style scoped lang="scss">
  .coin-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: white;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    margin-bottom: 10px;
    min-height: 62px;
    cursor: pointer;

    &:hover {
      background-color: #ebfbff;
    }

    &__left-wrapper {
      display: flex;
    }

    &__right-wrapper {
      display: flex;
    }

    &__favourite-wrapper, {
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0 20px;
    }
    &__coin-logo-wrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 15px;
    }
    &__coin-info-wrapper {
      display: flex;
      flex-direction: column;
    }
    &__coin-name {
      color: #050a16;
      font-size: 16px;
      font-weight: 900;
      line-height: 1;
      text-transform: uppercase;
      margin-bottom: 5px;
    }
    &__coin-symbol {
      color: #050a16;
      font-size: 14px;
      font-style: italic;
      line-height: 1;
      text-transform: uppercase;
    }
  }
</style>
