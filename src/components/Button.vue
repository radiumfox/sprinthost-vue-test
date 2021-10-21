<template>
  <button class="form__submit" type="submit">{{ submitButtonName }}</button>
</template>

<script>

export default {
  name: 'Button',
  props: {
    submitButtonName: String,
  }
}

</script>

<style lang="scss">

@import '../sass/style.scss';

.form__submit {
  @include font(16px, 24px, 700);
  background-color: $accent;
  border: none;
  border-radius: 4px;
  padding: 12px 0;
  color: $basic-light;
  cursor: pointer;

  &:hover {
    background-color: $accent-special;
  }
}
</style>