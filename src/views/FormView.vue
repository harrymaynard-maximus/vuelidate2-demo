<template>
  <div>
    <h1>This is a form page</h1>
    <input
      v-model="name"
      @blur="validateField(v$.name)"
    />
    <br/>
    <div
      v-if="v$.name.$dirty && v$.name.required.$invalid"
    >This field is required</div>
    <br/>
    <div
      v-for="(address, index) in addresses"
      :key="index"
    >
      <FormItem
        :index="index"
        v-model:city="address.city"
      />
    </div>
    <button
      type="button"
      @click="submit()"
    >Submit</button>
  </div>
</template>

<script>
import useVuelidate from '@vuelidate/core';
import { required } from '@vuelidate/validators';
import FormItem from '../components/FormItem.vue';

export default {
  name: 'FormView',
  components: {
    FormItem,
  },
  data() {
    return {
      name: null,
      addresses: [],
    };
  },
  setup() {
    return {
      v$: useVuelidate(),
    };
  },
  mounted() {
    this.addresses = [
      {
        city: null,
      },
      {
        city: null,
      }
    ]
  },
  validations() {
    return {
      name: {
        required,
      }
    }
  },
  methods: {
    submit() {
      this.v$.$touch();
      if (this.v$.$invalid) {
        console.log('invalid submission')
        return;
      }
      console.log('valid submission');
      console.log('addresses:', this.addresses)
    },
    validateField(validation) {
      if (validation) {
        validation.$touch();
      }
    }
  }
};
</script>
