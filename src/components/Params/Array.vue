<template>
  <div>
    <p v-text="_startCase(param.name)" />
    <Params
      v-for="(item, i) in input"
      :key="i"
      :defaultValue="defaultValue[i]"
      :param="paramSingle"
      v-model="input[i]"
      @input="$emit('input', input)"
      class="mb-3"
    />
  </div>
</template>

<script>
import { clone } from '@/helpers/utils';

export default {
  props: ['defaultValue', 'param'],
  data() {
    return {
      input: ['', '']
    };
  },
  computed: {
    paramSingle() {
      const paramSingle = clone(this.param);
      paramSingle.type = paramSingle.type.replace('[]', '');
      return paramSingle;
    }
  },
  created() {
    if (this.defaultValue) this.input = this.defaultValue;
    this.$emit('input', this.input);
  }
};
</script>
