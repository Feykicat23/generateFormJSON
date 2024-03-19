<template>
  <div>
    <h2>{{ parent.name }}</h2>
    <form novalidate @submit.prevent="onSubmit(parent.items, 'parent')">
      <template v-for="(item, index) in parent.items" :key="index">
        <component 
          :is="getComponentType(item.type)"
          :label="item.label"
          :options="item.additional?.options"
          v-model="parentFormData[item.name]"
          @years="yearsChange(parentFormData, $event, 'parent')"
          @gender="genderChange(parentFormData, $event, 'parent')" />
      </template>

      <button type="submit">Отправить</button>
      <button type="reset">Стереть</button>
    </form>

    <h2>{{ child.name }}</h2>
    <form novalidate @submit.prevent="onSubmit(child.items, 'child')">
      <template v-for="(item, index) in child.items" :key="index">
        <component 
          :is="getComponentType(item.type)"
          :label="item.label"
          :options="item.additional?.options"
          v-model="childFormData[item.name]"
          @years="yearsChange(childFormData, $event, 'child')"
          @gender="genderChange(childFormData, $event, 'child')" />
      </template>

      <button type="submit">Отправить</button>
      <button type="reset">Стереть</button>
    </form>
  </div>
</template>

<script>
import formConfig from "/Users/user/Desktop/untitled folder/frontend-test/form-config.json"
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";

export default {
  name: "FormBuilder",
  data() {
    return {
      parent: formConfig.parent,
      child: formConfig.child,
      formConfig: formConfig,
      parentFormData: {},
      childFormData: {},
    };
  },
  
  methods: {
    yearsChange(formData, years, formType) {
      formData.age = years;
    },
    genderChange(formData, gender, formType) {
      formData.gender = gender;
    },
    onSubmit(items, formType) {
      const formData = formType === 'parent' ? this.parentFormData : this.childFormData;

      this.errors = [];
      console.log(formData)


      if (!formData || Object.keys(formData).length === 0) {
        return alert(`${formType} форма пуста!`);
      }

      if (Object.values(formData).some(value => value === '') || Object.keys(formData).length !== 5) {
        return alert(`${formType} имеет пустые поля!`);
      }

      if (formData.pass !== formData['repeat-pass']) {
        return alert('Пароли не совпадают!');
      }

        const requestData = {
          name: formData.name,
          gender: formData.gender,
          age: parseInt(formData.age),
          pass: formData.pass
      };

        console.log(requestData)
        alert(`${formType} form is valid!`);
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
