<template>
  <UiButton class="d-flex width-full">
    <input
      v-model.trim="input"
      @input="$emit('input', input !== 'false')"
      :placeholder="_startCase(param.name)"
      type="text"
      class="input width-full"
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
      input: true
    };
  },
  created() {
    if (this.defaultValue) this.input = this.defaultValue;
    this.$emit('input', this.input);
  }
};
</script>
