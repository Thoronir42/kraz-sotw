<template>
  <form class="auth-form" @submit.prevent="submitForm">
    <h1>{{formSpec.title}} <small>{{formSpec.id}}</small></h1>
    <div class="form-group">
      <label for="login" class="input-label">Jméno</label>
      <input name="login" id="login" v-model="values.login"/>

      <label for="password" class="input-label">Heslo</label>
      <input
          name="password" id="password" type="password" v-model="values.password"
          autocomplete="new-password"
      />
    </div>

    <div class="form-group">
      <label class="check-input">
        <input type="checkbox" checked readonly>
        <span>Jsem obeznámen s vedlejšími účinky kúry</span>
      </label>
      <label class="check-input">
        <input type="checkbox" checked readonly>
        <span>Vzdávám se nároku na vrácení poplatku</span>
      </label>
    </div>

    <div class="form-group">
      <button type="submit">Předložit</button>
    </div>
  </form>
</template>

<script lang="ts">

import {defineComponent, PropType, ref} from "vue";
export default defineComponent({
  props: {
    formSpec: {type: Object as PropType<AuthForm>, required: true},
  },
  setup(props, {emit}) {
    const values = ref({
      login: '',
      password: '',
    });

    return {
      values,
      submitForm() {
        emit('submitted', values.value);
      },
    };
  },
});
</script>

<style lang="scss">

</style>
