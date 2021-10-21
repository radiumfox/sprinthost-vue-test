<template>
  <div class="form__control">
    <input class="form__input form__input--checkbox visually-hidden" 
      v-bind:id="id"
      v-bind:checked="checked"
      type="checkbox"
      />
    <label v-bind:for="id" class="form__label form__label--checkbox">{{ checkboxText }}</label>
  </div>
</template>

<script>

export default {
  name: 'Checkbox',
  props: {
    id: String,
    checkboxText: String,
    checked: Boolean,
  }
}

</script>

<style lang="scss">
@import '../sass/style.scss';

.form__label--checkbox {
  @include font(14px, 20px, 400);
  color: $basic-grey;
  position: relative;
  padding-left: 36px;
  cursor: pointer;
  letter-spacing: 0;

  &::before {
    content: "";
    position: absolute;
    width: 16px;
    height: 16px;
    border: 2px solid $basic-darkest;
    border-radius: 2px;
    left: 2px;
    top: 0;
  }

  &:hover::before {
    border-color: rgba($basic-darkest, 0.8);
  }

  @media(max-width: $mobile-width-only) {
    padding-left: 40px;
  }
}

.form__input--checkbox:checked + .form__label--checkbox {
  &::before {
    content: "";
    position: absolute;
    background-image: url('../assets/checked.svg');
    background-repeat: no-repeat;
    background-position: center;
    border-color: $special-grey-light;
  }
}

</style>