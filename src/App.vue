<script setup>
import { reactive } from 'vue';
import DualDatepicker from './Components/Datepicker.vue';
const dateTest = reactive({ value: null });
</script>
<template>
  <DualDatepicker v-model="dateTest.value" :disabled="{type:'EN',from:null,to:new Date(),dates:[]}" defaultDate="EN" />
</template>