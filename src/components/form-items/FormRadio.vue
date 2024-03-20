<template>
  <fieldset>
    <legend>{{ label }}</legend>

    <div v-for="option in options" :key="option.value">
      <input 
        required 
        type="radio" 
        name="radio"
        :id="option.value"
        :value="option.value"
        :checked="option.value === value"
        @change="yearsChange(parseInt(option.value))"
      />
      <label :for="option.value">{{ option.text }}</label>
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

  methods: {
    yearsChange(years) {
     
      this.$emit('years', years)
    }
  }
}
</script>

<style scoped>

</style>
