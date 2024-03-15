<template>
  <FormInput :label="label" type="password" @change-value="updateValue"/>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";

export default {
  name: "FormPassword",
  props: {
    label: {
      type: String,
      default: ''
    },
    updateValue:{
      type:Function,
      required:true,
    }
  },
  components: {
    FormInput
  }
}
</script>

<style lang="scss" scoped>

</style>
