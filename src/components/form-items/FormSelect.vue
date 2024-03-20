<template>
  <div>
    <p>{{ label }}</p>
    <select @change="genChange">
      <option value="" disabled selected>Выберите значение</option>
      <option
        v-for="option in options"
        :key="option.value"
        :selected="option.selected"
        :value="option.value"
      >{{ option.text }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "FormSelect",

  props: {
    label: {
      type: String,
      default: ''
    },
    options: {
      type: Array,
      required: true
    }
  },

  methods: {
    genChange(event) {
    const gender = event.target.value;
  
    this.$emit('gender', gender);
  }
 }
}
</script>

<style scoped>

</style>
