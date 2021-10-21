<template>
  <div class="success">
    <p class="success__message">Пароль успешно изменен!</p>
  </div>
</template>

<script>
export default {
  name: 'SuccessMessage'
}
</script>
  
<style lang="scss">
  @import '../sass/global.scss';

  .success {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    border-radius: 8px;
    background-color: $basic-light;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .success__message {
    @include font(28px, 35px, 700);
    text-align: center;
    max-width: 300px;
  }

</style>