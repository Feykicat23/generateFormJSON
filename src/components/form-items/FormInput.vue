<template>
  <div>
    <p>{{ label }}</p>
    <input
        v-bind="$attrs"
        :type="type"
        required
        v-model="internalValue"
    />
  </div>
</template>

<script>
export default {
  name: "FormInput",

  props: {
    label: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    },
    value: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      internalValue: this.value 
    };
  },

  watch: {
    internalValue(newValue) {
      this.$emit('input', newValue);
    }
  }
};
</script>

<style scoped>

</style>
