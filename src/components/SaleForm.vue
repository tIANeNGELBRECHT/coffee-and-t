<script lang="ts" setup>
import { ref } from 'vue';

const myForm = ref();

const name = ref('');
const age = ref<number | null>(null);
const accept = ref(false);

function onSubmit() {
  myForm.value.validate().then((success: boolean) => {
    if (success) {
      alert(`Form submitted:\nName: ${name.value}\nAge: ${age.value}\nAccepted: ${accept.value}`);
      reset();
    } else {
      alert('Please fix the errors in the form.');
    }
  });
}

function onReset() {
  reset();
}

function reset() {
  myForm.value.resetValidation();
  name.value = '';
  age.value = null;
  accept.value = false;
}
</script>

<template>
  <div class="q-pa-md" style="max-width: 400px;">
    <q-form
      ref="myForm"
      @submit.prevent="onSubmit"
      @reset.prevent="onReset"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="name"
        label="Your name *"
        hint="Name and surname"
        lazy-rules
        :rules="[val => val && val.length > 0 || 'Please type something']"
      />

      <q-input
        filled
        type="number"
        v-model="age"
        label="Your age *"
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Please type your age',
          val => val > 0 && val < 100 || 'Please type a real age'
        ]"
      />

      <div class="row gap">
        <q-btn label="Reset" type="reset" color="primary" outline class="q-ml-sm" />
        <q-btn label="Submit" type="submit" color="primary" />
      </div>
    </q-form>
  </div>
</template>
