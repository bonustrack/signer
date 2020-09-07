<template>
  <div>
    <Params
      v-for="(c, i) in param.components"
      :key="i"
      :param="c"
      v-model="input[c.name]"
      @input="$emit('input', input)"
      class="mb-3"
    />
  </div>
</template>

<script>
export default {
  props: ['value', 'param'],
  data() {
    return {
      input: {}
    };
  }
};
</script>
