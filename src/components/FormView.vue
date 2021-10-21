<template>
  <form class="form" action="#" method="post" @submit="checkForm">

    <FormControl v-model="newPassword" labelText="Новый пароль" id="new-password" placeholder="Введите новый пароль" />
    <FormControl v-model="newPasswordRepeat" labelText="Повторите новый пароль" id="new-password-repeat" placeholder="Повторите новый пароль" />

    <Checkbox checkboxText="Завершить сеансы на других устройствах" id="end-sessions"/>

    <div class="form__errors-wrap" v-if="errors.length">
      <ul class="form__errors">
        <li class="form__error" v-bind:key="error" v-for="error in errors">{{ error }}</li>
      </ul>
    </div>
  
    <Button submitButtonName="Сменить пароль" />
  </form>
</template>

<script>
import FormControl from './FormControl.vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';

export default {
  name: 'FormView',
  components: {
    FormControl,
    Checkbox,
    Button,
  },
  // props: {
  //   submitButtonName: String,
  //   checkboxText: String,
  //   id: String,
  // },

  data: ()=> {
    return {
      errors: [],
      newPassword: null,
      newPasswordRepeat: null,
      checkedValue: null,
      regExp: /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/,
    }
  },

  methods: {
    openModal: function () {
      document.querySelector('.modal').classList.add('modal--show');
      document.querySelector('.page-body').classList.add('page-body--overlay');
      
    },

    checkForm: function (e) {
      if (this.newPassword && this.newPasswordRepeat && (this.regExp.test(this.newPassword))) {
        this.openModal();
        e.preventDefault();
      } else {
        e.preventDefault();
      }

      this.errors = [];

      if (!this.newPassword) {
        this.errors.push('Введите пароль');
      }
      if (this.newPassword && !this.newPasswordRepeat) {
        this.errors.push('Повторите пароль');
      }
      if(!(this.newPassword === this.newPasswordRepeat)){
        this.errors.push('Пароли не совпадают');
      }
      if(!(this.regExp.test(this.newPassword))){
        this.errors.push('Пароль должен состоять как минимум из 8 символов, иметь хотя бы одну букву и одно число');
      }

      console.log(this.newPassword);
    },

  }
}
</script>

<style lang="scss">
@import '../sass/style.scss';

.form {
  @media(max-width: $tablet-width-only) {
    max-width: 328px;
    width: 100%;
    display: flex;
    flex-direction: column;
  }

  @media(max-width: $mobile-width-only) {
    max-width: 100%;
    order: 1;
  }
}


</style>