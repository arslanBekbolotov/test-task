<template>
  <div class="form-builder container">
    <div v-for="(form,index) in formData" :key="index">
      <form class="form" @submit.prevent="onSubmit(form.id)">
        <h1>{{form.name}}</h1>

        <div class="form-row" v-for="(formItem,i) in form.items" :key="i">
          <component
              :is="getComponentName(formItem.type)"
              @change-value="updateParentValue"
              :parent-name="form.id"
              :name="formItem.name"
              :label="formItem.label"
              :options="formItem.additional?.options"
              :updateValue="updateParentValue"
          />
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


export default {
  name: "FormBuilder",
  data() {
    return {
      formData: null,
      isDisabled: false,
      passMatch:true,
    }
  },
  methods: {
    async fetchFromData() {
      try {
        const response = await fetch("../../form-config.json");
        this.formData = await response.json();
      } catch (error) {
        console.error("Ошибка при получении файла:", error);
      }
    },
    onSubmit(parentName) {
      try {
        if (!this.formData) {
          console.error("Форм дата еще не получена");
          return;
        }

        const newData = {}
        newData[parentName] = {};

        this.formData[parentName].items.forEach(item => {
          newData[parentName][item.name] = item.value
        })

        alert("Success")
      } catch (e) {
        console.error(e)
      }
    },
    updateParentValue(parentName, name, newValue) {
      if (name === 'repeat-pass') {
        this.validateFormPassword(newValue,name,parentName)
      }

      const item = this.formData[parentName].items.find(item => item.name === name);

      if (item) {
        item.value = newValue;
      }
    },
    resetFields(parentName) {
      this.formData[parentName].items.forEach(item => item.value = '')
    },
    validateFormPassword(newValue,name,parentName){
      const passwordField = document.getElementById(`password-${name}`);
      const passwordValue = this.formData[parentName].items.find(item => item.name === 'pass').value;

      if (newValue !== passwordValue) {
        passwordField.setCustomValidity('Пароли не совпадают');
      } else {
        passwordField.setCustomValidity('');
      }
    },
    getComponentName(type) {
      switch (type) {
        case 'input':
          return 'FormInput';
        case 'select':
          return 'FormSelect';
        case 'radio':
          return 'FormRadio';
        case 'password':
          return 'FormPassword';
        default:
          return 'FormInput';
      }
    }
  },
  components: {FormPassword, FormRadio, FormSelect, FormInput},
  async beforeMount() {
    await this.fetchFromData();

    for (let key in this.formData) {
      this.formData[key].id = key;
      this.formData[key].items.forEach(item => item.value = "")
    }
  }
}
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 30px;
}

button {
  align-self: flex-start;
}
</style>
