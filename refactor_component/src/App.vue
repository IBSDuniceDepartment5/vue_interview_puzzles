<template>
  <CustomForm
      id="form"
      @submit="onFormSubmit"
      :class="['form', layout || 'vertical']"
  >
    {{ getTransformedValue(variable) }}

    <template #values>
      <ul>
        <li
            v-for="value in formValues"
            :key="generateRandomString()"
        >
          {{ value }}
        </li>
      </ul>
    </template>

    <CustomInput v-model="formValues.first" ref="input" />
    <CustomInput v-model.lazy="formValues.third" />
  </CustomForm>
</template>

<script>
import generateRandomString from 'generator'; // always generates unique string
import expensiveComputations from 'computation';
const initialFormValues = {
  first: null,
  second: null,
};
export default {
  props: {
    layout: String // Required. Possible values: vertical | horizontal
  },

  data () {
    return { variable: 'example' }
  },
  computed: {
    formValues () {
      return this.$store.state.formValues;
    }
  },
  watch: {
    formValues (value, prevValue) {
      if (value.first !== prevValue.first) { console.log('changed') }
    },

    '$refs.input.innerValue' (value) { console.log('changed') }
  },
  created () {
    document.querySelector('#form').classList.add(this.layout);
    this.$store.state.formValues = initialFormValues;
  },
  mounted () {
    this.$root.$on('form:data:set', () => { console.log('changed'); });
  },
  methods: {
    onFormSubmit (event) { event.preventDefault(); },
    getTransformedValue (value) { return expensiveComputations(value); }
  }
};
</script>

<style scoped>
.vertical >>> .input {...}
</style>

<style>
.form {...}
</style>

<docs>
Component docs: ...
</docs>
