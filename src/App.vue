<template>
  <h1 class="app__title">Смена пароля</h1>
  <div class="app__wrapper">

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
        <form class="modal__form form" action="#" method="post" @submit="checkForm">
         
          <FormControl className="modal__form-control" labelText="Новый пароль" id="new-password" placeholder="Введите новый пароль" />
          <FormControl className="modal__form-control" labelText="Повторите новый пароль" id="new-password-repeat" placeholder="Повторите новый пароль" />

          <Checkbox checkboxText="Выйти со всех других устройств" id="end-sessions-modal" checked/>
           <div class="form__errors-wrap" v-if="errors.length">
            <ul class="form__errors">
              <li class="form__error" v-bind:key="error" v-for="error in errors">{{ error }}</li>
            </ul>
          </div>

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
