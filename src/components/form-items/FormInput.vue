<template>
  <div>
    <p>{{ label }}</p>
    <input
        :id="'password-' + name"
        :name="name"
        :type="type"
        required
        v-model.trim="localValue"
        @blur="updateValue"
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
    name:{
      type:String,
      default:"",
    },
    type: {
      type: String,
      default: 'text'
    },
    parentName:{
      type:String,
      default:"parent",
    }
  },
  data(){
    return{
      localValue: "",
    }
  },
  methods: {
    updateValue() {
      this.$emit("change-value",this.parentName, this.name, this.localValue);
    },

  }
}
</script>

<style scoped>
input {
  padding: 5px 10px;
}
</style>
