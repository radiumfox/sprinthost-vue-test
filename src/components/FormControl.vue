<template>
  <div class="form__control" :class="className">
    <label class="form__label" :for="id">{{labelText}}</label>
    <input 
      class="form__input"
      type="text" 
      :id="id" 
      :placeholder="placeholder"
      :value="value"
      @input="change"
    />
    <small class="form__control-alert" v-if="error">{{ error }}</small>
  </div>
</template>

<script>

export default {
  name: 'FormControl',
  emits: ['update:value'],
  props: {
    value: String,
    labelText: String,
    placeholder: String,
    id: String,
    className: String,
    error: String
  },

  methods: {
    change(event) {
      this.$emit('update:value', event.target.value)
    }
  },
}

</script>
