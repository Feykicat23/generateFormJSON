<template>
  <div>
    <template v-for="(form, key) in formConfig" :key="key">
      <h2>{{ form.name }}</h2>
      <form @submit.prevent="onSubmit(form.items, key)">
        <template v-for="(item, index) in form.items" :key="index">
          <component 
            :is="getComponentType(item.type)"
            :label="item.label"
            :options="item.additional?.options"
            v-model="formData[key][item.name]"
            @years="receiveData(formData[key], item.name, $event, key)"
            @gender="receiveData(formData[key], item.name, $event, key)" />
        </template>
        
        <div v-if="showPasswordError[key]" class="error-message">
          Пароли не совпадают!
        </div>
        
        <button type="submit">Отправить</button>
        <button type="reset">Стереть</button>
      </form>
    </template>
  </div>
</template>

<script>
import formConfig from "../../../frontend-test/form-config.json"
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";

export default {
  name: "FormBuilder",
  data() {
    const formData = {};
    const showPasswordError = {}; // Добавляем свойство для отслеживания показа ошибки

    // Создаем объект formData для каждой формы из formConfig
    Object.keys(formConfig).forEach(key => {
      formData[key] = {};
      showPasswordError[key] = false; // Инициализируем свойство показа ошибки как false
    });

    return {
      formConfig: formConfig,
      formData: formData,
      showPasswordError: showPasswordError // Добавляем свойство для отслеживания показа ошибки
    };
  },
  
  methods: {
    receiveData(formData, name, value, formType) {
      formData[name] = value;
    },

    onSubmit(items, formType) {
      const formData = this.formData[formType];
      
      let password, repeatPassword;

      // Находим два значения паролей
      items.forEach(item => {
          if (item.type === 'password') {
              if (item.additional && item.additional.parent) {
                  repeatPassword = formData[item.name];
                  console.log(repeatPassword)
              } else {
                  password = formData[item.name];
                  console.log(password)
              }
          }
      });

      // Проверяем, совпадают ли значения паролей
      if (password === repeatPassword) {
          this.showPasswordError[formType] = false; // Сбрасываем ошибку, если пароли совпадают
          console.log('Пароли совпадают!');
      } else {
          this.showPasswordError[formType] = true; // Показываем ошибку, если пароли не совпадают
          console.log('Пароли не совпадают!');
          return
      }

      const requestData = {};
      for (const item of items) {
        console.log(item)
        if (!item.additional || !item.additional.parent) {
          requestData[item.name] = formData[item.name];
        }
      }

      console.log(requestData);
      alert(`${formType} форма валидна!`);
      this.fakeRequest(requestData);
    },

    getComponentType(type) {
      switch (type) {
        case 'input':
          return 'form-input';
        case 'select':
          return 'form-select';
        case 'radio':
          return 'form-radio';
        case 'password':
          return 'form-password';
      }
    }

  },
  components: { FormPassword, FormRadio, FormSelect, FormInput },
}
</script>

<style>
.error-message {
  color: red;
  margin-top: 5px;
}
</style>
