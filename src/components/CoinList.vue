<template>
  <div class="coin-list">
    <div class="coin-list__top-wrapper">
      <div class="coin-list__info-wrapper">
        <h2 class="coin-list__title">Etam eniam dolor epsilon</h2>
        <p class="coin-list__total-amount">{{ searchedCoins.length }} crypto coins</p>
      </div>
      <div class="coin-list__search-wrapper">
        <input
          ref="search"
          placeholder="Search"
          type="text"
          class="coin-list__search-input"
        />
        <!-- todo use icon after provide that feature for buttons -->
        <Button
          label="Search"
          class="coin-list__search-button"
          @click="onSearch"
        />
      </div>
    </div>
    <div
      v-for="(coin, index) in paginatedCoins"
      :key="index"
    >
      <CoinItem
        :coin="coin"
      />
    </div>

    <div class="coin-list__pagination">
      <Button
        v-show="isPreviousPageButtonVisible"
        label="Previous Page"
        class="coin-list__button coin-list__button--previous"
        @click="previousPage"
      />
      <Button
        v-show="isNextPageButtonVisible"
        label="Next Page"
        class="coin-list__button coin-list__button--next"
        @click="nextPage"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import CoinItem from './CoinItem.vue'
import Button from './Button.vue'

const START_PAGE = 1

export default defineComponent({
  name: 'CoinList',
  components: {
    CoinItem,
    Button
  },
  data () {
    return {
      searchValue: '',
      pageNumber: START_PAGE,
      pageSize: 11
    }
  },
  props: {
    coins: {
      type: Array,
      required: true
    }
  },

  computed: {
    /**
     * @return {boolean}
     */
    isNextPageButtonVisible () {
      return this.pageNumber < this.lastPage
    },

    /**
     * @return {boolean}
     */
    isPreviousPageButtonVisible () {
      return this.pageNumber > START_PAGE
    },

    searchedCoins () {
      return this.coins.filter((coin) => {
        const coinName = coin.name.toLowerCase()
        return coinName.startsWith(this.searchValue)
      })
    },

    paginatedCoins () {
      return this.paginate(this.searchedCoins, this.pageSize, this.pageNumber)
    },

    /**
     * @return {number}
     */
    totalCoins () {
      return this.searchedCoins.length
    },

    /**
     * @return {number}
     */
    lastPage () {
      return Math.ceil(this.totalCoins / this.pageSize)
    }
  },
  methods: {
    onSearch () {
      this.searchValue = this.$refs.search.value.toLowerCase()
    },

    /**
     * @param {Array} array
     * @param {number} pageSize
     * @param {number} pageNumber
     * @return {Array}
     */
    paginate (array, pageSize, pageNumber) {
      // human-readable page numbers usually start with 1, so we reduce 1 in the first argument
      return array.slice((pageNumber - 1) * pageSize, pageNumber * pageSize)
    },

    nextPage () {
      if (this.pageNumber >= this.lastPage) return

      this.pageNumber++
    },

    previousPage () {
      if (this.pageNumber <= 1) return

      this.pageNumber--
    }
  }
})
</script>

<style scoped lang="scss">
.coin-list {
  &__top-wrapper {
    display: flex;
    justify-content: space-between;
    margin-bottom: 40px;
    margin-top: 100px;
  }

  &__title {
    color: #050a16;
    font-size: 22px;
    font-weight: 400;
    line-height: 1;
    margin: 0 0 13px 0
  }

  &__total-amount {
    color: #050a16;
    font-size: 14px;
    font-weight: 400;
    line-height: 1;
  }

  &__search-button {
    min-height: 45px;
  }

  &__search-input {
    min-width: 320px;
    padding: 0 15px;
    min-height: 45px;
    margin-right: 10px;
    border: 1px solid #dadada;
    &:focus {
      outline: none;
      border: 1px solid #61c5e2;
    }
  }

  &__pagination {
    display: flex;
    justify-content: flex-end;
    padding-top: 10px
  }

  &__button {
    &--previous {
      margin-right: 10px
    }
  }
}
</style>
