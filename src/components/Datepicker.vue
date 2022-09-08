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
import { computed } from '@vue/reactivity';

onMounted(() => {
  updateDates();
  main.disableDates.en = []
  main.today = todaysDate();
  main.dateType = 'EN';
  let container = document.getElementById('datePicker109usojfd');
  document.addEventListener('click', function (e) {
    if (container != e.target && !container.contains(e.target)) {
      main.showDatePicker = false;
    }
  });
});

watch(props, (newVal) => {
  if (newVal.modelValue) {
    main.dateEN = newVal.dateEN;
    main.dateNP = newVal.dateNP;
  }
});

const props = defineProps(['modelValue', 'defaultDate', 'disabled']);

const emit = defineEmits(['update:modalValue']);

const disableTo = computed(() => {
  if (props.disabled.to) {
    if (props.disabled.type == 'EN') {
      return { en: dateToObject(props.disabled.to), np: ConvertEnglishToNepali(props.disabled.to) }
    } else {
      return { en: ConvertNepaliToEnglish(props.disabled.to), np: stringToObject(props.disabled.to, 'NP') }
    }
  }
});
const disableFrom = computed(() => {
  if (props.disabled.from) {
    if (props.disabled.type == 'EN') {
      return { en: dateToObject(props.disabled.from), np: ConvertEnglishToNepali(props.disabled.from) }
    } else {
      return { en: ConvertNepaliToEnglish(props.disabled.from), np: stringToObject(props.disabled.from, 'NP') }
    }
  }
});
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
const today = null;
const main = reactive({
  input: {
    placeholder: "placeholder"
  },
  today: null,
  dateType: null,
  showDatePicker: false,
  disableTo: { en: null, np: null },
  disableFrom: { en: null, np: null },
  disableDates: { en: [], np: [] },
  originNP: '2026/08/17',
  dateEN: { text: null, value: null },
  dateNP: { text: null, value: null },
});

//converts date to string
function dateToString(date, type = 'EN', format = 'DD/MM/YYYY') {
  return duoDigitMaker(date.getDate()) + '/' + duoDigitMaker(date.getMonth() + 1) + '/' + date.getFullYear();
}

//converts date to object
function dateToObject(date) {
  return { year: date.getFullYear(), month: { value: date.getMonth(), text: null }, date: date.getDate() }
}

//converts string to date object
function stringToObject(date, type = 'EN') {
  if (date) {
    var _date = _date = date.split('/');
    if (type == 'EN') {
      return { year: _nepaliDates.find(x => x.year == _date[0]), month: { text: null, value: parseInt(_date[1]) - 1 }, date: _date[2] }
    } else {
      return { year: _date[2], month: { value: parseInt(_date[1]) - 1, text: null }, date: _date[0] }
    }
  }
}

//convert date object to string
function objectToString(obj, type = 'EN') {
  if (type == 'EN') {
    return obj.date + '/' + (parseInt(obj.month.value) + 1) + '/' + obj.year;
  } else {
    return obj.year.year + '/' + (parseInt(obj.month.value) + 1) + '/' + obj.date;
  }
}
/**
 * Get today's date
 * @return {object} object with english and nepali date object
 */
///returns todays english and nepali date
function todaysDate() {
  var _today = new Date();
  let _todaysDate = { nepali: { value: null, text: null }, english: { value: null, text: null } };
  _todaysDate.english.value = dateToObject(_today);
  _todaysDate.english.text = objectToString(_todaysDate.english.value);
  _todaysDate.nepali.value = ConvertEnglishToNepali(duoDigitMaker(parseFloat(_todaysDate.english.value.month.value) + 1) + '/' + _todaysDate.english.value.date + '/' + _todaysDate.english.value.year)
  _todaysDate.nepali.text = objectToString(_todaysDate.nepali.value);
  return _todaysDate;
}

/**
 * Returns total days from 2026 push 17 provided nepali date object
 * format object(object:{date:int,month:{text:string,value:string/int},year:{year:int,value:[int],days:f,startDay:int}})
 * @param {object} dateNP custom nepali date object
 * @return {int} total days
 */
function NPDateSpan(dateNP) {
  let _defaultDate = main.originNP.split('/').map(x => parseInt(x || 0));
  console.log("before", _nepaliDates);
  if (dateNP.year >= _defaultDate[0] && _nepaliDates[_nepaliDates.length - 1].year >= dateNP.year) {
    let _totalDays = _nepaliDates.filter(x => x.year > _defaultDate[0] && x.year < dateNP.year).reduce((s, x) => s + x.days(), 0) || 0;
    _totalDays += _nepaliDates.find(x => x.year == _defaultDate[0]).days() - _nepaliDates.find(x => x.year == _defaultDate[0]).value.slice(0, _defaultDate[1]).reduce((s, x) => s + parseInt(x || 0), 0) - _defaultDate[2];
    _totalDays += _nepaliDates.find(x => x.year == parseInt(dateNP.year))?.value.slice(0, parseInt(dateNP.month)).reduce((s, x) =>
      s + parseInt(x), 0) || 0;
    _totalDays += dateNP.date || 0;
    return _totalDays;
  } else console.error('Date out of range');
}

/**
 * Makes two digit :requires at least one digit.
 * @param {string,int} val 
 * @return {string} tow digit number
 */
function duoDigitMaker(val) {
  if (!val && val != 0) console.log("Got empty parameter for duo digit maker");
  else {
    if ((parseFloat(val) < 10)) return '0' + val;
    else return val.toString();
  }

}
/**
 * Updates the english date object to the english as well as nepali date string
 * obj:{object:{date:int,month:{text:string,value:string/int},year:{year:int,value:[int],days:f,startDay:int}}}
 * @param {Object} obj custom english date object
 * @return {void} void
 */
function updateEnglishDate(obj) {
  console.log("englisht to nepali");
  obj.date = duoDigitMaker(obj.date);
  obj.month.value = duoDigitMaker(obj.month.value + 1);
  main.dateEN.text = obj.date + '/' + obj.month.value + '/' + obj.year;
  main.dateEN.value = obj;
  main.dateEN.value.month.value = parseFloat(main.dateEN.value.month.value) - 1;
  toggleDatePicker(false);
  main.dateNP.value = ConvertEnglishToNepali((parseInt(obj.month.value) + 1) + '/' + obj.date + '/' + obj.year);
  main.dateNP.text = main.dateNP.value.year.year + '/' + duoDigitMaker(main.dateNP.value.month.value + 1) + '/' + duoDigitMaker(main.dateNP.value.date)
  // emit('update:modelValue',{dateEN:main.dateEN,dateNP:main.dateNP});
}


/**
 * Updates the english date object to the english as well as nepali date string
 * obj:{object:{date:int,month:{text:string,value:string/int},year:{year:int,value:[int],days:f,startDay:int}}}
 * @param {Object} obj custom english date object
 * @return {void} void
 */
function updateNepaliDate(obj) {
  obj.date = duoDigitMaker(obj.date);
  obj.month.value = duoDigitMaker(obj.month.value + 1);
  main.dateNP.text = obj.year.year + '/' + obj.month.value + '/' + obj.date;
  main.dateNP.value = obj;
  main.dateNP.value.month.value = parseFloat(main.dateNP.value.month.value) - 1;
  toggleDatePicker(false);
  main.dateEN.value = ConvertNepaliToEnglish(obj.year.year + '/' + obj.month.value + '/' + obj.date);
  main.dateEN.text = duoDigitMaker(main.dateEN.value.date) + '/' + duoDigitMaker(main.dateEN.value.month.value + 1) + '/' + main.dateEN.value.year;
  console.log(main.dateEN, main.dateNP);
  // emit('update:modelValue',{dateEN:main.dateEN,dateNP:main.dateNP});
}


/**
 * void: set boolean for toggeling date picker
 * @param {bool} val triggers whether to show the datepicker or not
 * @return {void}
 */
function toggleDatePicker(val) {
  main.showDatePicker = val;
}

///void: updates the _nepaliDates array with start day(0,7) of the year
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


///object: provides the simple nepali origin date in object :the base of the nepali date
function originObject() {
  let _splited = main.originNP.split('/');
  return { year: _splited[0], month: { text: null, value: _splited[1] }, date: _splited[2] }
}

///int: Takes english date
function getNPSpan(dateEN) {
  return parseInt((dateEN.getTime()) / 1000 / 60 / 60 / 24);
}
//calculates days to deduct to start date from 2026 baisakh 1
function initialDeduction() {
  return _nepaliDates.find(x => x.year == 2026).value.slice(0, 8).reduce((s, x) => s + parseInt(x), 0) + 17;
}
///object:void, adds days to nepali origin date
///int
function addNepaliDaysToOrigin(days) {
  var _origin = originObject();
  let _startYear = _nepaliDates.findIndex(x => x.year == _origin.year);
  if (_startYear >= 0) {
    let _addedDays = -initialDeduction();
    let x = null;
    for (let date in _nepaliDates) {
      x = _nepaliDates[date];
      if ((days - _addedDays) < x.days()) {
        for (var i = 0; i < x.value.length; i++) {
          if ((_addedDays + x.value[i]) > days) {
            _origin.year = _nepaliDates[date];
            _origin.month.value = i;
            _origin.date = days - _addedDays + 1
            return _origin;
          } else {
            _addedDays += x.value[i];
          }
        }
      } else {
        _addedDays += x.days();
      }
    };
  }
  else console.error('Date got out of range.');
}
//Converts Nepali Date to english date
function ConvertNepaliToEnglish(dateNP) {
  let _date = dateNP.split('/');
  if (_date.length == 3) {
    let _addingDays = NPDateSpan({ year: parseInt(_date[0]), month: parseInt(_date[1]), date: parseInt(_date[2]) })
    let _newDate = new Date(_addingDays * 24 * 60 * 60 * 1000);
    return { year: _newDate.getFullYear(), month: { text: null, value: _newDate.getMonth() }, date: _newDate.getDate() }
  }

}
///object: converts and returns nepali date object from english date string
///string:(mm/dd/yy)
function ConvertEnglishToNepali(dateEN) {
  console.log(dateEN, "English date selected");
  let _addingDays = getNPSpan(new Date(dateEN));
  return addNepaliDaysToOrigin(_addingDays);

};

defineExpose({ toggleDatePicker, _nepaliDates, updateEnglishDate, updateNepaliDate });

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
      <nepalidatepicker :value="main.dateNP" :disableTo="disableTo?.np" :disableDates="main.disableDates.np"
        :disableFrom="disableFrom?.np" :today="main.today?.nepali.value" :options="_nepaliDates"
        v-if="main.dateType == 'NP' && main.showDatePicker" />
      <englishdatepicker :value="main.dateEN" :disableTo="disableTo?.en" :disableDates="main.disableDates.en"
        :disableFrom="disableFrom?.en" :today="main.today?.english.value"
        v-else-if="main.dateType == 'EN' && main.showDatePicker" />
    </div>
  </div>
</template>
<style scoped>
.datepicker-main {}
</style>