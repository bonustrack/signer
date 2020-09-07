<template>
  <div>
    <div :key="i" v-for="(input, i) in command.inputs" class="mb-3">
      <h4 class="mb-2" v-text="_startCase(input.name)" />
      <Params
        :param="input"
        :defaultValue="value[i]"
        v-model="params[i]"
        @input="$emit('input', params)"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: ['command', 'value'],
  data() {
    return {
      params: []
    };
  }
};
</script>
