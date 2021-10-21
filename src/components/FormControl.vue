<template>
  <div class="form__control" v-bind:class="className">
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
    className: String,
  },
}

</script>

<style lang="scss">
// @import '../sass/style.scss';
// @import '../sass/form.scss';

</style>