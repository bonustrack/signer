<template>
  <div>
    <ParamsTuple
      v-if="param.type === 'tuple'"
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsArray
      v-else-if="param.type.endsWith('[]')"
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsAddress
      v-else-if="param.type === 'address'"
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsUint256
      v-else-if="param.type === 'uint256'"
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsBool
      v-else-if="param.type === 'bool'"
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
    <ParamsString
      v-else
      :defaultValue="defaultValue"
      :param="param"
      v-model="input"
      @input="$emit('input', input)"
    />
  </div>
</template>

<script>
export default {
  props: ['param', 'defaultValue'],
  data() {
    return {
      input: []
    };
  }
};
</script>
