<template>
  <div class="form__control">
    <label class="form__label" v-bind:for="id">{{labelText}}</label>
    <input 
      class="form__input" 
      v-bind:id="id" 
      type="text" 
      v-bind:placeholder="placeholder"
      />
      </div>
</template>

<script>

export default {
  name: 'FormControl',
  props: {
    labelText: String,
    placeholder: String,
    id: String,
  },
}

</script>

<style lang="scss">
@import '../sass/style.scss';

.form__control {
  padding-bottom: 26px;

  &:last-of-type {
    padding-top: 28px;
    padding-bottom: 33px;

    @media(max-width: $mobile-width-only) {
      padding-bottom: 34px;
    }
  }
}

.form__input {
  @include placeholder;
  display: flex;
  flex-shrink: 0;
  width: 100%;
  margin-top: 2px;
  padding: 6px 8px 7px;
  border: none;
  border-bottom: 2px solid rgba($basic-darkest, 0.24);
  box-sizing: border-box;

  &:focus {
    outline: none;
    border-color: rgba($basic-darkest, 0.5);
  }
}

.form__label {
  @include font(12px, 16px, 700);
  letter-spacing: 0.4px;
}

</style>