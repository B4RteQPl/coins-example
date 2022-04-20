<template>
  <div class="quote">
    <div class="quote__price-wrapper">
      <div
          :class="[
          'quote__price',
          isCurrency ? 'quote__price--currency' : 'quote__price--crypto'
        ]"
          v-html="price"
      />

      <div
        :class="[
          'quote__change',
          isTrendingUp ? 'quote__change--green' : 'quote__change--red'
        ]"
      >
        {{ quoteChange }}%
      </div>
    </div>
    <div class="quote__trending-icon-wrapper">
      <img
        v-if="isTrendingUp"
        src="../assets/arrow_up.png"
      />
      <img
        v-if="isTrendingDown"
        src="../assets/arrow_down.png"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Quote',
  props: {
    name: {
      type: String,
      required: true
    },
    quoteValue: {
      type: Number,
      required: true
    },
    quoteChange: {
      type: Number,
      required: true
    }
  },
  computed: {
    /**
     * @return {boolean}
     */
    isTrendingUp () {
      return this.quoteChange > 0
    },

    /**
     * @return {boolean}
     */
    isTrendingDown () {
      return this.quoteChange < 0
    },

    /**
     * @return {boolean}
     */
    isCurrency () {
      // todo fill list of currencies supported by software - good idea is to create currency class
      const currencies = ['USD']

      return currencies.includes(this.name)
    },

    /**
     * @todo good idea is to move parsing price to price class with currency or crypto class
     * @return {string}
     */
    price () {
      return this.isCurrency
        ? this.getCurrencyPrice()
        : this.getCryptoPrice()
    }
  },
  methods: {
    getCurrencyPrice () {
      const currencySign = this.getCurrencySign()
      const roundedPrice = this.getRoundedPrice(3)

      return `${currencySign}${roundedPrice}`
    },

    /**
     * @return {string}
     */
    getCryptoPrice () {
      const roundedPrice = this.getRoundedPrice(8)

      // todo change to gray all characters after 4 decimal place
      return `${roundedPrice}`
    },

    /**
     * @param {number} decimals
     * @return {number}
     */
    getRoundedPrice (decimals = 0) {
      return Number(`${Math.round(`${this.quoteValue}e${decimals}`)}e-${decimals}`)
    },

    /**
     * @return {string}
     */
    getCurrencySign () {
      switch (this.name) {
        case 'USD':
          return '$'
          // todo add more examples if approach requires dynamic solution
        default:
          return ''
      }
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.quote {
  min-width: 200px;
  display: flex;
  justify-content: flex-end;

  &__trending-icon-wrapper {
    min-width: 10px;
    margin-left: 10px;
  }
  &__price-wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
  }

  &__price {
    color: #050a16;
    font-size: 16px;
    line-height: 1;
    margin-bottom: 5px;
    &--crypto {
      font-weight: 400;
    }
    &--currency {
      font-weight: 900;
    }
  }
  &__change {
    color: #050a16;
    font-size: 14px;
    font-weight: 400;
    line-height: 1;
    &--green {
      color: #43aa05;
    }
    &--red {
      color: #ca312c;
    }
  }
}
</style>
