<template>
  <h1 class="app__title">Смена пароля</h1>
  <div class="app__wrapper">

    <form class="form" action="#" method="post" @submit.prevent="submitHandler">
      
      <FormControl 
        :error="errors.newPassword"
        v-model:value="newPassword" 
        labelText="Новый пароль" 
        id="new-password" 
        placeholder="Введите новый пароль"/>
      <FormControl 
        :error="errors.newPasswordRepeat"
        v-model:value="newPasswordRepeat"  
        labelText="Повторите новый пароль" 
        id="new-password-repeat" 
        placeholder="Повторите новый пароль" />

      <Checkbox checkboxText="Завершить сеансы на других устройствах" id="end-sessions"/>

      <SubmitButton submitButtonName="Сменить пароль" />
    </form>

    <div class="notion">
      <strong class="notion__emphasis">Используйте сложные пароли и не сохраняйте их&nbsp;в браузере.</strong>
      <p class="notion__text">Данные меры цифровой безопасности помогают предотвратить взлом профиля.</p>
    </div>

    <div class="modal" v-on:click="closeModal">
      <button 
        class="modal__cross"
        type="button" 
        aria-label="Закрыть"
        v-on:click="closeModal">
      </button>
      <div class="modal__form-wrapper" v-on:click.stop>
        
        <button 
          class="modal__close" 
          type="button" 
          v-on:click="closeModal">
          {{getText()}}
        </button>
        <p class="modal__warning">Рекомендуем сменить пароль, если заметили подозрительную активность в профиле</p>
        <form class="modal__form form" action="#" method="post" @submit.prevent="submitHandler">
         
          <FormControl
            v-model:value="newPasswordRepeat" 
            className="modal__form-control" 
            labelText="Новый пароль" 
            id="new-password" 
            placeholder="Введите новый пароль" />
          <FormControl 
            v-model:value="newPasswordRepeat"
            className="modal__form-control" 
            labelText="Повторите новый пароль" 
            id="new-password-repeat" 
            placeholder="Повторите новый пароль" />

          <Checkbox checkboxText="Выйти со всех других устройств" id="end-sessions-modal" checked/>
           <div class="form__errors-wrap" v-if="errors.length">
            <ul class="form__errors">
              <li class="form__error" v-bind:key="error" v-for="error in errors">{{ error }}</li>
            </ul>
          </div>

          <SubmitButton v-on:click="showMessage" submitButtonName="Сменить пароль" />
        </form>
        <SuccessMessage v-if="isSubmit" />
      </div>
    </div>
  </div>

  <section class="info">
    <div class="info__wrapper">
      <h3 class="info__title">Пароли для FTP и SSH</h3>
      <p class="info__text">По умолчанию для авторизации используется пароль от профиля.</p>
      <p class="info__text">Сменить пароль, узнать сервер и IP адрес можно в разделе «Управление сайтами».</p>
      <a class="info__link info__link--action" href="#" target="_blank">Управление сайтами</a>
      <a class="info__faq" href="#" target="_blank">Как настроить?</a>
    </div>
    <div class="info__wrapper info__wrapper--for-devs">
      <h3 class="info__title">Доступ для разработчиков</h3>
      <p class="info__text">Безопасная передача прав для управления и&nbsp;настройки сайтов, с помощью функции делегирования доступа.</p>
      <a class="info__link" href="#" target="_blank">Связанные аккаунты</a>
    </div>
  </section>
</template>

<script>
import FormControl from './components/FormControl.vue';
import Checkbox from './components/Checkbox.vue';
import SubmitButton from './components/SubmitButton.vue';
import SuccessMessage from './components/SuccessMessage.vue';

export default {
  name: 'App',
  components: {
    FormControl,
    Checkbox,
    SubmitButton,
    SuccessMessage
  },
  data() {
    return {
      getText() {
        if(window.innerWidth < 751) {
          return 'Назад'
        } 
        return 'Закрыть'
      },

      isSubmit: false,
      regExp: /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/,
      newPassword: '',
      newPasswordRepeat: '',
      errors: {
        newPassword: null,
        newPasswordRepeat: null,
      }
    }
  },
  methods: {
    openModal() {
      document.querySelector('.modal').classList.add('modal--show');
      document.querySelector('.page-body').classList.add('page-body--overflow-hidden');
    },

    closeModal() {
      document.querySelector('.modal').classList.remove('modal--show');
      document.querySelector('.page-body').classList.remove('page-body--overflow-hidden');
    },

    formIsValid() {
      let isValid = true

      if (!this.newPassword) {
        this.errors.newPassword = 'Введите пароль'
        isValid = false
      } else if(!(this.regExp.test(this.newPassword))) {
        this.errors.newPassword = 'Пароль должен состоять как минимум из 8 символов, иметь хотя бы одну букву и одно число'
        isValid = false
      } else {
        this.errors.newPassword = null
      }

      if(this.newPassword && this.newPasswordRepeat && (this.newPasswordRepeat !== this.newPassword)) {
        this.errors.newPasswordRepeat = 'Введенные пароли не совпадают'
        isValid = false
      } else if (!this.newPasswordRepeat) {
        this.errors.newPasswordRepeat = 'Повторите пароль'
        isValid = false
      } else {
        this.errors.newPasswordRepeat = null
      }

      return isValid
    },

    submitHandler() {
      if (this.formIsValid()) {
        this.openModal();
      }
    },

    showMessage() {
      const forms = document.querySelectorAll('.form');
      this.isSubmit = true;
      forms.forEach(form => form.reset());
    }
  }
};
</script>

<style lang="scss">
@import 'sass/global.scss';
@import 'sass/blocks/info.scss';
@import 'sass/blocks/modal.scss';
@import 'sass/blocks/notion.scss';
@import 'sass/blocks/form.scss';
@import 'sass/blocks/page-body.scss';

.app {
  font-family: "Open Sans", Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $basic-dark;
  padding-top: 64px;
  max-width: 1040px;
  margin: 0 auto;
    
  @media(max-width: $tablet-width-only) {
    // padding-top: 64px;
    max-width: 680px;
    // margin: 0 auto;
  }

  @media(max-width: $mobile-width-only) {
    max-width: 100%;
    padding: 32px 43px 0;
  }
}

.app__title {
  @include font(26px, 28px, 700);
  padding-bottom: 15px;
  border-bottom: 1px solid rgba($basic-darkest, 0.12);
  margin: 0;
}

.app__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding-top: 62px;

  @media(max-width: $mobile-width-only) {
    flex-direction: column;
    padding-top: 64px;
  }
}

</style>
