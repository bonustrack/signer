<template>
  <UiButton class="d-flex width-full">
    <input
      v-model.trim="input"
      @input="$emit('input', input)"
      type="number"
      step="0"
      class="input width-full"
      placeholder="123"
    />
    <UiLabel style="margin-top: 11px;" class="mr-n2">
      {{ param.type }}
    </UiLabel>
  </UiButton>
</template>

<script>
export default {
  props: ['defaultValue', 'param'],
  data() {
    return {
      input: ''
    };
  },
  created() {
    if (this.defaultValue) this.input = this.defaultValue;
    this.$emit('input', this.input);
  }
};
</script>
