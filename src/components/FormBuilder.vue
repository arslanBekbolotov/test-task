<template>
  <div class="form-builder">
    <div v-for="(form,index) in formData" :key="index">
    <form @submit.prevent="onSubmit">
      <h1>{{form.name}}</h1>

      <div v-for="(formItem,i) in form.items" :key="i">
        <form-input @blur="updateParentValue" :parent-name="form.id"  v-if="formItem.type === 'input'" :name="formItem.name" :label="formItem.label" />
        <form-select @change-value="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'select'" :options="formItem.additional.options" :label="formItem.label"/>
        <form-radio @change-value="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'radio'" :options="formItem.additional.options" :label="formItem.label"/>
        <form-password :updateValue="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'password'" :name="formItem.name" :label="formItem.label"/>
      </div>

      <button type="submit">Отправить</button>
      <button type="reset">Стереть</button>
    </form>
    </div>
  </div>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";
import data from "../../form-config.json";

export default {
  name: "FormBuilder",
  data(){
    return{
     formData: data,
    }
  },
  methods: {
    onSubmit() {
      alert('Submit')
    },
    updateParentValue(parentName,name, newValue) {
      console.log(name)
      if(name === 'repeat-pass'){
        newValue = this.checkRepeatedPass(parentName,newValue)
      }

      this.formData[parentName].items[name] = newValue;
    },
    checkRepeatedPass(parentName,newValue){
      if(this.formData[parentName].items.pass === newValue ){
        return newValue;
      }

      alert("Данные не совпадают")
      return ""
    }
  },
  components: {FormPassword, FormRadio, FormSelect, FormInput},
  beforeMount() {
   for (let key in this.formData) {
     this.formData[key].id = key;
     this.formData[key].items.forEach(item => item.value = '')
   }
 }
}
</script>

<style scoped>

</style>
