<template>
  <fieldset>
    <legend>{{ label }}</legend>

    <div
        v-for="option in options"
        :key="option.value"
    >
      <input @change="handleChange" required type="radio" name="radio" :id="option.name" :value="option.value" />
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
    name:{
      type:String,
      required: true,
    },
    parentName:{
      type:String,
      default: "parent",
    },
    options: {
      type: Object,
      required: true
    }
  },
  methods:{
    handleChange(event) {
      const selectedValue = event.target.value;
      this.$emit("change-value", this.parentName,this.name, selectedValue);
    }
  }
}
</script>

<style scoped>

</style>
