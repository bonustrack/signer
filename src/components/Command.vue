<template>
  <div>
    <div :key="i" v-for="(input, i) in command.inputs" class="mb-3">
      <h4 class="mb-2" v-text="startCase(input.name)" />
      <Params
        :param="input"
        v-model="params[i]"
        @input="$emit('input', params)"
      />
    </div>
  </div>
</template>

<script>
import startCase from 'lodash/startCase';

export default {
  props: ['command'],
  data() {
    return {
      params: []
    };
  },
  methods: {
    startCase
  }
};
</script>
