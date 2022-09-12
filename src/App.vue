<script setup>
import { reactive } from 'vue';
import DualDatepicker from './Components/Datepicker.vue';
const dateTest = reactive({ value: null });
</script>
<template>
  <DualDatepicker v-model="dateTest.value"
    :disabled="{type:'EN',from:new Date(),to:new Date('2022/03/11'),dates:[new Date(),new Date('2022/09/11')]}"
    defaultDate="EN" />
</template>