<template>
  <div>
    <Params
      v-for="(c, i) in param.components"
      :key="i"
      :defaultValue="defaultValue[c.name]"
      :param="c"
      v-model="input[c.name]"
      @input="$emit('input', input)"
      class="mb-3"
    />
  </div>
</template>

<script>
export default {
  props: ['defaultValue', 'param'],
  data() {
    return {
      input: {}
    };
  },
  mounted() {
    if (this.defaultValue) this.input = this.defaultValue;
    this.$emit('input', this.input);
  }
};
</script>
