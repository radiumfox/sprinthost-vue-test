<template>
  <div class="form__control" :class="className">
    <input class="form__input form__input--checkbox visually-hidden" 
      :id="id"
      :checked="checked"
      type="checkbox"
    />
    <label :for="id" class="form__label form__label--checkbox">{{ checkboxText }}</label>
  </div>
</template>

<script>

export default {
  name: 'Checkbox',
  props: {
    id: String,
    checkboxText: String,
    checked: Boolean,
    className: String,
  }
}

</script>
