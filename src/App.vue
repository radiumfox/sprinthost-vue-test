<template>
  <h1 class="app__title">Смена пароля</h1>
  <div class="app__wrapper">

    <form class="form" action="#" method="post" @submit="checkForm">
      <div class="form__errors-wrap" v-if="errors.length">
        <ul class="form__errors">
          <li class="form__error" v-bind:key="error" v-for="error in errors">{{ error }}</li>
        </ul>
      </div>
    
      <FormControl v-model="newPassword" labelText="Новый пароль" id="new-password" placeholder="Введите новый пароль" />
      <FormControl v-model="newPasswordRepeat" labelText="Повторите новый пароль" id="new-password-repeat" placeholder="Повторите новый пароль" />

      <Checkbox checkboxText="Завершить сеансы на других устройствах" id="end-sessions"/>

      <Button submitButtonName="Сменить пароль" />
    </form>

    <div class="notion">
      <strong class="notion__emphasis">Используйте сложные пароли и не сохраняйте их&nbsp;в браузере.</strong>
      <p class="notion__text">Данные меры цифровой безопасности помогают предотвратить взлом профиля.</p>
    </div>

    <div class="modal" v-on:click="closeModalWindow">
      <button 
        class="modal__cross"
        type="button" 
        aria-label="Закрыть"
        v-on:click="closeModalWindow">
      </button>
      <div class="modal__form-wrapper" v-on:click.stop>
        <button 
          class="modal__close" 
          type="button" 
          v-on:click="closeModalWindow">
          {{getText()}}
        </button>
        <p class="modal__warning">Рекомендуем сменить пароль, если заметили подозрительную активность в профиле</p>
        <form class="form modal__form" action="#" method="post" @submit="checkForm">
          <div class="form__errors-wrap" v-if="errors.length">
            <ul class="form__errors">
              <li class="form__error" v-bind:key="error" v-for="error in errors">{{ error }}</li>
            </ul>
          </div>
          <FormControl labelText="Новый пароль" id="new-password" placeholder="Введите новый пароль" />
          <FormControl labelText="Повторите новый пароль" id="new-password-repeat" placeholder="Повторите новый пароль" />

          <Checkbox checkboxText="Выйти со всех других устройств" id="end-sessions-modal" checked/>

          <Button submitButtonName="Сменить пароль" />
        </form>
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
import Button from './components/Button.vue';

export default {
  name: 'App',
  components: {
    FormControl,
    Checkbox,
    Button,
  },
  data: function() {
    return {
      getText: function() {
        if(window.innerWidth < 751) {
          return 'Назад'
        } 
        return 'Закрыть'
      },
      errors: [],
      newPassword: null,
      newPasswordRepeat: null,
      checkedValue: null,
      regExp: /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/,
    }
  },
  methods: {
    closeModalWindow: function() {
      document.querySelector('.modal').classList.remove('modal--show');
      document.querySelector('.page-body').classList.remove('page-body--overflow-hidden');
    },

    openModal: function () {
      document.querySelector('.modal').classList.add('modal--show');
      document.querySelector('.page-body').classList.add('page-body--overflow-hidden');
      
    },

    checkForm: function (evt) {
      const newPassword = document.getElementById('new-password');
      const newPasswordRepeat = document.getElementById('new-password-repeat');

      if (newPassword.value && newPasswordRepeat.value && (this.regExp.test(newPassword.value))) {
        this.openModal();
        evt.preventDefault();
      } else {
        evt.preventDefault();
      }

      this.errors = [];

      if (!newPassword.value) {
        this.errors.push('— Введите пароль');
      }
      if (newPassword.value && !newPasswordRepeat.value) {
        this.errors.push('— Повторите пароль');
      }
      if(!(newPassword.value === newPasswordRepeat.value)){
        this.errors.push('— Пароли не совпадают');
      }
      if(!(this.regExp.test(newPassword.value))){
        this.errors.push('— Пароль должен состоять как минимум из 8 символов, иметь хотя бы одну букву и одно число');
      }
    },

  }
};
</script>

<style lang="scss">
@import 'sass/style.scss';

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

.notion {
  box-sizing: border-box;
  max-width: 240px;
  padding: 60px 16px 16px;
  border: 1px solid $accent-light-special;
  border-radius: 8px;
  background-image: url('assets/notion-icon.svg');
  background-repeat: no-repeat;
  background-position: 16px 16px;

  @media(max-width: $mobile-width-only) {
    order: 0;
    margin-bottom: 21px;
    padding: 59px 15px 15px;
    background-position: 15px 15px;
  }
}

.notion__emphasis {
  @include font(14px, 20px, 700);
  padding-bottom: 5px;
  display: block;
}

.notion__text {
  @include font(12px, 16px, 400);
  margin: 0;
}

.info__wrapper {
  max-width: 578px;
  padding-top: 65px;
  
  &--for-devs {
    padding-right: 300px;
    background-image: url("assets/illustration.svg");
    background-repeat: no-repeat;
    background-position: right 100px;
    padding-bottom: 158px;
    padding-top: 63px;
  }

  @media(max-width: $mobile-width-only) {
    display: flex;
    flex-direction: column;

    &--for-devs {
      padding-right: 0;
      padding-top: 248px;
      padding-bottom: 128px;
      background-position: right 102px;
    }
  }
}



.info__title {
  @include font(22px, 24px, 700);
  color: $basic-dark;
  margin: 0;
  padding-bottom: 18px;
}

.info__text {
  @include font(14px, 20px, 400);
  color: $basic-grey;
  margin: 0;
  
  &:last-of-type {
    padding-bottom: 17px;
  }
}

.info__wrapper--for-devs .info__title {
  padding-bottom: 17px;
}

.info__wrapper--for-devs .info__text {
  padding-bottom: 22px;

  @media(max-width: $mobile-width-only) {
    &:last-of-type {
      padding-bottom: 24px;
    }
  }
}

.info__link {
  @include font(16px, 24px, 700);
  color: $basic-darkest;
  padding: 6px 15px;
  text-decoration: none;
  border: 2px solid $accent;
  border-radius: 4px;
  display: inline-block;
  vertical-align: middle;

  &:hover {
    color: rgba($basic-darkest, 0.8);
  }

  &--action {
    padding: 7px 54px 7px 15px;
    background-image: url("assets/action-arrow.svg");
    background-repeat: no-repeat;
    background-position: 92% 8px;
  }

  @media(max-width: $mobile-width-only) {
    max-width: 211px;
    box-sizing: border-box;
    padding: 6px 14px;

    &--action {
      padding: 7px 44px 6px 15px;
      max-width: 252px;
      margin-bottom: 12px;
    }
  }
}

.info__faq {
  @include font(16px, 20px, 700);
  color: $accent-special;
  text-decoration: none;
  padding-bottom: 3px;
  margin-left: 24px;
  position: relative;

  &::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 2px;
    background-image: url("assets/dashed-line.svg");
    background-repeat: repeat-x;
    bottom: 0;
  }

  &:hover {
    color: rgba($accent-special, 0.8);
    border-color: rgba($accent-special, 0.8);
  }

  @media(max-width: $mobile-width-only) {
    margin-left: 0;
    max-width: 130px;
  }
}

.modal {
  display: none;
  // background-color: $basic-light;
  background-color: rgba($basic-darkest, 0.45);
  // padding: 32px;
  // max-width: 392px;
  width: 100vw;
  box-sizing: border-box;

  @media(max-width: $mobile-width-only) {
    max-width: 100%;
    height: 100vh;
    overflow-y: hidden;
  }
}

.modal--show {
  display: block;
  position: fixed;
  // top: 266px;
  top: 0;
  left: 0;
  height: 100vh;
  overflow-y: hidden;
  // left: calc(50% - 196px);
  z-index: 1;

  @media(max-width: $mobile-width-only) {
    top: 0;
    left: 0;
  }
}

.modal__cross {
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: transparent;
  border: none;
  top: 47px;
  right: 34px;
  cursor: pointer;

  &::before,
  &::after {
    content: "";
    position: absolute;
    width: 27px;
    height: 2px;
    background-color: $basic-light;
    top: 0;
    right: 0;
  }

  &::before {
    transform: rotate(45deg);
  }

  &::after {
    transform: rotate(-45deg);
  }

  @media(max-width: $mobile-width-only) {
    display: none;
  }
}

.modal__close {
  @include font(10px, 10px, 700);
  border: none;
  background-color: transparent;
  cursor: pointer;
  display: flex;
  margin-left: auto;
  padding: 0 0 2px;
  color: $special-grey;
  border-bottom: 1px dashed $accent-special;

  &:hover {
    color: rgba($special-grey, 0.8);
  }

  @media(max-width: $mobile-width-only) {
    @include font(14px, 20px, 700);
    color: $accent-special;
    border-bottom: none;
    padding-left: 26px;
    margin-left: 0;
    background-image: url("assets/arrow-back.svg");
    background-repeat: no-repeat;
    background-position: 3px 5px;

    &:hover {
      color: rgba($accent-special, 0.8);
    }
  }
}

.page-body--overflow-hidden {
  overflow-y: hidden;
}

.form {
  max-width: 328px;
  width: 100%;
  display: flex;
  flex-direction: column;

  // @media(max-width: $tablet-width-only) {
  //   max-width: 328px;
  //   width: 100%;
  //   display: flex;
  //   flex-direction: column;
  // }

  @media(max-width: $mobile-width-only) {
    max-width: 100%;
    order: 1;
  }
}

.modal__form-wrapper {
  margin: 266px auto 0;
  padding: 31px 32px 32px;
  background-color: $basic-light;
  max-width: 393px;
  box-sizing: border-box;
  border-radius: 8px;
  border: 1px solid rgba($basic-darkest, 0.38);

  @media(max-width: $mobile-width-only) {
    max-width: 100vw;
    height: 100vh;
    margin: 0;
    border-radius: 0;
    border: none;
    padding: 35px 43px 43px;
  }
}

.modal__warning {
  @include font(22px, 24px, 700);
  color: $basic-dark;
  margin: 0;
  padding: 23px 0 22px;
}

.modal .form__control {
  padding-bottom: 42px;

  &:last-of-type {
    padding-top: 0;
    padding-bottom: 38px;
  }
}

.modal .form__label--checkbox {
  padding-left: 40px;
}

.form__errors {
  @include reset-list;
  @include font(12px, 18px, 700);
  color: $alert-color;
}



</style>
