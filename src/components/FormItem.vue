<template>
  <div>
    Index {{index}}<br/>
    City #{{index + 1}}
    <br/>
    <input
      :id="`city-${index}`"
      :value="city"
      @blur="validateField(v$.city)"
      @input="$emit('update:city', $event.target.value)"
    />
    <br/>
    <div
      v-if="v$.city.$dirty && v$.city.required.$invalid"
    >This field is required</div>
    <br/>
  </div>
</template>

<script>
import useVuelidate from '@vuelidate/core';
import { required } from '@vuelidate/validators';

export default {
  name: 'FormView',
  props: {
    city: {
      type: String,
      default: '',
    },
    addrLine: {
      type: String,
      default: '',
    },
    index: {
      type: Number,
    }
  },
  setup() {
    return {
      v$: useVuelidate(),
    };
  },
  validations() {
    const validation = {
      city: {
        required,
      },
    };
    return validation;
  },
  methods: {
    validateField(validation) {
      if (validation) {
        validation.$touch();
      }
    }
  }
};
</script>
