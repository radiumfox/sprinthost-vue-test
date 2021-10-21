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