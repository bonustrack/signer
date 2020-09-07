<template>
  <div>
    <ParamsTuple
      v-if="param.type === 'tuple'"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsArray
      v-else-if="param.type.endsWith('[]')"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsAddress
      v-else-if="param.type === 'address'"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsUint256
      v-else-if="param.type === 'uint256'"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsBool
      v-else-if="param.type === 'bool'"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsString
      v-else
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
  </div>
</template>

<script>
export default {
  props: ['param'],
  data() {
    return {
      input: []
    };
  }
};
</script>
