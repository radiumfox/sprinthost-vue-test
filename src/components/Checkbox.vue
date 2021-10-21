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