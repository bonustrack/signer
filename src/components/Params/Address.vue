<template>
  <UiButton :class="!isValid && 'border-red'" class="d-flex width-full">
    <input
      v-model.trim="input"
      @input="$emit('input', input)"
      type="text"
      class="input width-full"
      placeholder="0x123..."
    />
    <UiLabel style="margin-top: 11px;" class="mr-n2">
      {{ param.type }}
    </UiLabel>
  </UiButton>
</template>

<script>
import { isAddress } from '@ethersproject/address';

const wildcards = ['$ME', '$DAI', '$WETH'];

export default {
  props: ['defaultValue', 'param'],
  data() {
    return {
      input: ''
    };
  },
  computed: {
    isValid() {
      return (
        !this.input || isAddress(this.input) || wildcards.includes(this.input)
      );
    }
  },
  created() {
    if (this.defaultValue) this.input = this.defaultValue;
    this.$emit('input', this.input);
  }
};
</script>
