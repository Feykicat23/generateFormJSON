<template>
  <fieldset :id="formId">
    <legend>{{ label }}</legend>

    <div v-for="option in options" :key="option.value">
      <input 
        required 
        type="radio" 
        :name="radioName"
        :id="generateId(option.value)"
        :value="option.value"
        :checked="option.value === value"
        @change="yearsChange(parseInt(option.value))"
      />
      <label :for="generateId(option.value)">{{ option.text }}</label>
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "FormRadio",

  props: {
    label: {
      type: String,
      default: ''
    },
    options: {
      type: Array,
      required: true
    },
    value: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      formId: 'form-' + Math.random().toString(36),
      radioName: 'radio-' + Math.random().toString(36)
    };
  },

  methods: {
    yearsChange(years) {
      this.$emit('years', years);
    },
    generateId(value) {
      return `radio-${this.formId}-${value}`;
    }
  }
}
</script>

<style scoped>

</style>
