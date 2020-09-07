<template>
  <div>
    <p v-text="startCase(param.name)" />
    <Params
      v-for="(item, i) in input"
      :key="i"
      :param="paramSingle"
      v-model="input[i]"
      @input="$emit('input', input)"
      class="mb-3"
    />
  </div>
</template>

<script>
import startCase from 'lodash/startCase';
import { clone } from '@/helpers/utils';

export default {
  props: ['param'],
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
  methods: {
    startCase
  }
};
</script>
