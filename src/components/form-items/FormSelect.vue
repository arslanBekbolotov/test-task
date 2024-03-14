<template>
  <div>
    <p>{{ label }}</p>
    <select :name="name" @change="handleChange" required>
      <option
        value=""
        selected
      >Выберите значение</option><option
        v-for="option in options"
        :key="option.value"
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
    parentName:{
      type:String,
      default: "parent",
    },
    name:{
      type:String,
      required: true,
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
