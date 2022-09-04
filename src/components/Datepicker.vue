<style scoped>
.datepicker-main {
  width: 250px;
}
</style>
<script>
export default {
  name: 'DualDatepicker'
}
</script>
<script setup>
import { reactive, onMounted, ref, watch } from 'vue';
import nepalidatepicker from './NepaliDatePicker.vue';
import englishdatepicker from './EnglishDatepicker.vue';

onMounted(() => {
  updateDates();
  main.dateType = 'EN';
  let container = document.getElementById('datePicker109usojfd');
  document.addEventListener('click', function (e) {
    if (container != e.target && !container.contains(e.target)) {
      main.showDatePicker = false;
    }
  });
});

watch(props,(newVal)=>{
  console.log("date Changed");
  if(newVal.modelValue){
    main.dateEN = newVal.dateEN;
    main.dateNP = newVal.dateNP;
  }
});

const props = defineProps(['modelValue','defaultDate']);

const _nepaliDates = [
  { year: 2026, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2027, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2028, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2029, value: [31, 31, 32, 31, 32, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2030, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2031, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2032, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2033, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2034, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2035, value: [30, 32, 31, 32, 31, 31, 29, 30, 30, 29, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2036, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2037, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2038, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2039, value: [31, 31, 31, 32, 31, 31, 29, 30, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2040, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2041, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2042, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2043, value: [31, 31, 31, 32, 31, 31, 29, 30, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2044, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2045, value: [31, 32, 31, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2046, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2047, value: [31, 31, 31, 32, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2048, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2049, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2050, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2051, value: [31, 31, 31, 32, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2052, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2053, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2054, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2055, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2056, value: [31, 31, 32, 31, 32, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2057, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2058, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2059, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2060, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2061, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2062, value: [30, 32, 31, 32, 31, 31, 29, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2063, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2064, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2065, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2066, value: [31, 31, 31, 32, 31, 31, 29, 30, 30, 29, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2067, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2068, value: [31, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2069, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2070, value: [31, 31, 31, 32, 31, 31, 29, 30, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2071, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2072, value: [31, 32, 31, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2073, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2074, value: [31, 31, 31, 32, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2075, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2076, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2077, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2078, value: [31, 31, 31, 32, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2079, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2080, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2081, value: [31, 31, 32, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2082, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2083, value: [31, 31, 32, 31, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2084, value: [31, 31, 32, 31, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2085, value: [31, 32, 31, 32, 30, 31, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2086, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2087, value: [31, 31, 32, 31, 31, 31, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2088, value: [30, 31, 32, 32, 30, 31, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2089, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2090, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2091, value: [31, 31, 32, 31, 31, 31, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2092, value: [30, 31, 32, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2093, value: [30, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2094, value: [31, 31, 32, 31, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2095, value: [31, 31, 32, 31, 31, 31, 30, 29, 30, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2096, value: [30, 31, 32, 32, 31, 30, 30, 29, 30, 29, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2097, value: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2098, value: [31, 31, 32, 31, 31, 31, 29, 30, 29, 30, 29, 31], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null },
  { year: 2099, value: [31, 31, 32, 31, 31, 31, 30, 29, 29, 30, 30, 30], days: function () { return this.value.reduce((s, x) => s + x, 0) }, startDay: null }
];
const dateTypeList = ['EN', 'NP'];
const _startDay = 0;
const main = reactive({
  input: {
    placeholder: "placeholder"
  },
  dateType: null,
  showDatePicker: false,
  originNP: '2026/08/17',
  dateEN: { text: null, value: null },
  dateNP: { text: null, value: null },
});
class EnglishDate extends Date{

}
class NepaliDate {
  constructor(date){

  }
  constructor(year,month,date){

  }
  constructor(time){

  }
  format(){

  }
}

function todaysDate(){
  var _enDate = new Date();
}

function duoDigitMaker(val) {
  if (!val && val != 0) console.log("Got empty parameter for duo digit maker");
  else {
    if ((parseFloat(val) < 10)) return '0' + val;
    else return val.toString();
  }
}

function updateEnglishDate(obj) {
  obj.date = duoDigitMaker(obj.date);
  obj.month.value = duoDigitMaker(obj.month.value + 1);
  main.dateEN.text = obj.date + '/' + obj.month.value + '/' + obj.year;
  main.dateEN.value = obj;
  toggleDatePicker(false);
  main.dateNP.value = ConvertEnglishToNepali(obj.month.value + '/' + obj.date + '/' + obj.year);
  main.dateNP.text = main.dateNP.value.year + '/' + duoDigitMaker(main.dateNP.value.month.value + 1) + '/' + duoDigitMaker(main.dateNP.value.date)
  $emit('update:modelValue',{dateEN:main.dateEN,dateNP:main.dateNP});
}

function toggleDatePicker(val) {
  main.showDatePicker = val;
}

function updateDates() {
  var _prev = null;
  _nepaliDates.map((x, i) => {
    if (i == 0) x.startDay = _startDay;
    else {
      x.startDay = (_prev.startDay + _prev.days()) % 7;
    }
    _prev = x;
  });
};

function originObject() {
  let _splited = main.originNP.split('/');
  return { year: _splited[0], month: { text: null, value: _splited[1] }, date: _splited[2] }
}

function getNPSpan(dateEN) {
  return parseInt((dateEN.getTime()) / 1000 / 60 / 60 / 24);
}
//find two nepali date differences

function addNepaliDaysToOrigin(days) {
  var _origin = originObject();
  let _startYear = _nepaliDates.findIndex(x => x.year == _origin.year);
  if (_startYear >= 0) {
    let _addedDays = -_origin.date;
    let x = null;
    for (let date in _nepaliDates) {
      x = _nepaliDates[date];
      let y = null;
      for (var i = 0; i < x.value.length; i++) {
        y = x.value[i];
        if (x.year > _origin.year || (x.year == _origin.year && _origin.month.value <= i)) {
          if ((_addedDays + y) <= days) {
            _addedDays += y;
          } else {
            _origin.year = x.year;
            _origin.month.value = i;
            _origin.date = days + 1 - _addedDays;
            return _origin;
          }
        }
      }
    };
  }
  else console.error('Date got out of range.');
  console.log(_origin);
}

defineExpose({ toggleDatePicker, _nepaliDates, updateEnglishDate });

</script>
<template>
  <div class="datepicker-main" id="datePicker109usojfd">
    <div class="datepicker-input">
      <input type="text" :placeholder="main.input.placeholder"
        :value="main.dateType == 'EN' ? main.dateEN.text : main.dateNP.text" :maxlength="0"
        @focus="main.showDatePicker = true" />
      <select v-model="main.dateType">
        <option v-for="item in dateTypeList">{{ item }}</option>
      </select>
    </div>
    <div class="datepicker-body">
      <nepalidatepicker :value="main.dateNP" :options="_nepaliDates"
        v-if="main.dateType == 'NP' && main.showDatePicker" />
      <englishdatepicker :value="main.dateEN" v-else-if="main.dateType == 'EN' && main.showDatePicker" />
    </div>
  </div>
</template>
<style scoped>
.datepicker-main {}
</style>