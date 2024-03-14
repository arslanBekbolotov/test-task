<template>
  <div class="form-builder container">
    <div v-for="(form,index) in formData" :key="index">
    <form class="form" @submit.prevent="onSubmit(form.id)">
      <h1>{{form.name}}</h1>

      <div class="form-row" v-for="(formItem,i) in form.items" :key="i">
        <form-input @blur="updateParentValue" :parent-name="form.id"  v-if="formItem.type === 'input'" :name="formItem.name" :label="formItem.label" />
        <form-select @change-value="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'select'" :name="formItem.name" :options="formItem.additional.options" :label="formItem.label"/>
        <form-radio @change-value="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'radio'" :name="formItem.name" :options="formItem.additional.options" :label="formItem.label"/>
        <form-password :updateValue="updateParentValue" :parent-name="form.id" v-else-if="formItem.type === 'password'" :name="formItem.name" :label="formItem.label"/>
      </div>

      <button type="submit" :disabled="isDisabled">Отправить</button>
      <button type="reset" @click="resetFields(form.id)">Стереть</button>
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
      isDisabled:false,
    }
  },
  methods: {
    onSubmit(parentName) {
      try{
        const newData = {}
        newData[parentName] = {};

        this.formData[parentName].items.forEach(item => {
          newData[parentName][item.name] = item.value
        })

        console.log(newData)
        alert("Success")
      }catch (e){
        console.error(e)
      }
    },
    updateParentValue(parentName,name, newValue) {
      if(name === 'repeat-pass'){
        newValue = this.checkRepeatedPass(parentName,newValue)
      }

      const item = this.formData[parentName].items.find(item => item.name === name);
      console.log(item,name,newValue)

      if (item) {
        item.value = newValue;
      }
    },
    checkRepeatedPass(parentName,newValue){
      this.isDisabled = false
      const item = this.formData[parentName].items.find(item => item.name === 'pass');

      if(item.value !== newValue ){
        alert("Данные не совпадают")
        this.isDisabled = true
      }

      return newValue;
    },
    resetFields(parentName){
      this.formData[parentName].items.forEach(item => item.value = '')
    }
  },
  components: {FormPassword, FormRadio, FormSelect, FormInput},
  beforeMount() {
   for (let key in this.formData) {
     this.formData[key].id = key;
     this.formData[key].items.forEach(item => item.value = "")
   }
 }
}
</script>

<style scoped>
.form{
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 30px;
}

button{
  align-self: flex-start;
}
</style>
