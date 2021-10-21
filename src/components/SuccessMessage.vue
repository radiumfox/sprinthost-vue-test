<template>
  <div class="success">
    <p class="success__message">Пароль успешно изменен!</p>
    <button @click="makeEvent" class="success__close">Закрыть</button>
  </div>
</template>

<script>

export default {
  name: 'SuccessMessage',
  methods: {
    makeEvent() {
      this.$emit('success-close')
    }
  }
}

</script>
