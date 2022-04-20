<template>
  <button class="button">
    <div class="button__label">
      {{ label }}
    </div>
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Button',
  props: {
    label: {
      type: String,
      required: true
    }
    // todo add icons feature
  }
})
</script>

<style scoped lang="scss">
  .button {
    background: #61c5e2;
    border: none;
    cursor: pointer;
    transition: background-color 0.15s ease-in-out;
    &:hover {
      background: #49a0ec;
    }
    &__label {
      color: #ffffff;
      font-size: 14px;
      font-weight: 400;
      line-height: 32px;
    }
  }
</style>
