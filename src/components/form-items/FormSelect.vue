<template>
  <div>
    <p>{{ label }}</p>
    <select>
      <option
        v-for="option in options"
        :key="option.value"
        :selected="option.selected"
        @change="selectOption"
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
      default: "",
    },
    options: {
      type: Object,
      required: true
    }
  },
  data(){
    return{
      localValue:"",
    }
  },

  methods:{
    selectOption(option) {
      this.localValue = option.value;
      this.$emit("change", this.name, option);
    },

  }
}
</script>

<style scoped>

</style>
