<style scoped>
.englishcalender-main {
    border: 1px solid skyblue;
    border-radius: 5px;
}

.at-center {
    display: inline-flex;
    justify-content: center;
    align-items: center;
}

.space-betn {
    display: inline-grid;
    grid-template-columns: auto auto;
    justify-content: space-between;
    align-items: center;
}

.week-grid {
    display: grid;
    grid-template-columns: 33px 33px 33px 33px 33px 33px 33px;
}

.week-grid div,
.englishcalender-head div {
    height: 33px;
    display: inline-flex;
    justify-content: center;
    align-items: center;
}

.englishcalender-head {
    display: grid;
    grid-template-columns: 33px auto 33px;
    justify-content: space-between;
}

.date-item {
    border: 1px solid rgba(0, 0, 0, 0);
    cursor: pointer;
}

.date-item:hover {
    border: 1px solid skyblue;
}

e {
    border: 1px solid skyblue;
}

.disabled-item {
    pointer-events: none;
    opacity: 0.3;
}
</style>
<script>
export default {
    name: 'englishdatepicker',
    inheritAttrs: false,
    data() {
        return {
            _startDay: 4,
            _yearRange: [1970, (new Date()).getFullYear()],
            calender: {
                month: null,
                year: null,
                date: null,
            },
            monthDates: 30,
            weekdays: [
                { text: 'Sun', value: 0 },
                { text: 'Mon', value: 1 },
                { text: 'Tue', value: 2 },
                { text: 'Wed', value: 3 },
                { text: 'Thu', value: 4 },
                { text: 'Fri', value: 5 },
                { text: 'Sat', value: 6 },
            ],
            month: [
                { text: 'Jan', value: 0 },
                { text: 'Feb', value: 1 },
                { text: 'Mar', value: 2 },
                { text: 'Apr', value: 3 },
                { text: 'May', value: 4 },
                { text: 'Jun', value: 5 },
                { text: 'Jul', value: 6 },
                { text: 'Aug', value: 7 },
                { text: 'Sep', value: 8 },
                { text: 'Oct', value: 9 },
                { text: 'Nov', value: 10 },
                { text: 'Dec', value: 11 },
            ],
        }
    },
    props: ['value', 'today', 'disableTo', 'disableFrom', 'disableDates'],
    created() {
        if (!this.value.value) {
            this.calender.year = this.today.year;
            this.calender.month = this.month.find(x => x.value == (parseFloat(this.today.month.value)));
            this.calender.date = this.today.date;
        } else {
            this.calender.year = this.value.value.year;
            this.calender.month = this.month.find(x => x.value == (parseFloat(this.value.value.month.value)));
            this.calender.date = this.value.value.date;
        }
    },
    mounted() {
        console.log("disabeldares", this.disableDates);
    },
    computed: {
        yearStartDay() {
            let _leapYearGap = 4 - this._yearRange[0] % 4;
            let _yearGap = this.calender.year - this._yearRange[0];
            let _leapDays = parseInt((_leapYearGap + _yearGap) / 4);
            return (this._startDay + _yearGap * 365 + _leapDays) % 7

        },
        monthStartDay() {
            if (this.calender.month == 0) return this.yearStartDay;
            else return (this.yearStartDay + this.currentYearMonths.slice(0, this.calender.month.value).reduce((s, x) => s + x, 0)) % 7;
        },
        currentYearMonths() {
            let _months = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
            if (this.calender.year % 4 == 0) _months[1] = 29;
            return _months;
        },
    },
    methods: {
        nextMonth() {
            let _month = (this.calender.month.value + 1) % 12;
            if (_month == 0) {
                var _year = (this.calender.year + 1);
                if (_year <= this._yearRange[1]) {
                    this.calender.year = _year;
                }
            }
            this.calender.month = this.month.find(x => x.value == _month);
        },
        prevMonth() {
            let _month = (12 + this.calender.month.value) % 12;
            if (_month == 0) {
                var _year = (this.calender.year - 1);
                _month = 11;
                if (_year >= this._yearRange[0]) {
                    this.calender.year = _year;
                }
            } else _month -= 1;
            this.calender.month = this.month.find(x => x.value == _month);;
        },
        updateDate(date) {
            // this.$emit('change', );
            this.calender.date = date;
            this.$parent.updateEnglishDate(this.calender);
        },
        disableYear(year = this.calender.year) {
            return this.disableDates?.includes(x => x.year == year) || year < this.disableTo?.year || year > this.disableFrom?.year;
        },
        affectingCurrentYear(year = this.calender.year) {
            return year == this.disableTo?.year || year == this.disableFrom?.year;
        },
        affectingCurrentMonth(month = this.calender.month) {
            return month.value == this.disableTo?.month.value || month.value == this.disableFrom?.month.value;
        },
        affectingCurrentDate(date) {
            let _isAffected = this.disableDates.findIndex(x => x.year == this.calender.year && this.calender.month.value == x.month.value && x.date == date);
            return _isAffected >= 0 ? true : false;
        },
        disableMonth(month = this.calender.month.value) {
            if (this.disableYear()) return true;
            if (this.affectingCurrentYear()) {
                return (this.disableTo?.month.value > month || this.disableFrom?.month.value < month)
            }
            return false;
        },
        disableDate(date,) {
            console.log(date, this.disableMonth(), this.disableYear(), this.disableFrom);
            if (this.disableYear() || this.disableMonth()) return true;
            if (this.affectingCurrentDate(date)) return true;
            else if (this.affectingCurrentYear() && this.affectingCurrentMonth())
                return (this.disableTo?.date > date || this.disableFrom?.date < date);
            return false;
        },
        disableNextMonth() {
            if (this.calender.month.value == 0) {
                if (this.calender.year == _yearRange[1]) {
                    return true;
                }
            }
            if (this.calender.month.value < this.disableTo?.month.value) return false;
            else return true;
        },
        disablePrevMonth() {
            if (this.calender.month.value == 0) {
                if (this.calender.year == _yearRange[0]) {
                    return true;
                }
            }
            if (this.calender.month.value > this.disableTo?.month.value) return false;
            else return true; //check year  todo
        },
    }
}
</script>
<template>
    <div class="englishcalender-main">
        <div class="englishcalender-head">
            <div class="head-left at-center">
                <button type="button" @click="prevMonth()" :class="{'disabled-item': disablePrevMonth()}">{{ '<'
                }}</button>
            </div>
            <div class="head-center space-betn">
                <select v-model="calender.year">
                    <option :disabled="disableYear(year + _yearRange[0]-1)"
                        v-for="year in  (_yearRange[1]-_yearRange[0]+1)" :value="year + _yearRange[0]-1">{{
                        year+_yearRange[0]-1 }}</option>
                </select>
                <select v-model="calender.month">
                    <option v-for="mon in month" :disabled="disableMonth(mon.value)" :value="mon">{{
                    mon.text }}</option>
                </select>
            </div>
            <div class="head-right at-center">
                <button type="button" @click="nextMonth()" :class="{'disabled-item': disableNextMonth()}">{{ '>'
                }}</button>
            </div>
        </div>
        <div class="englishcalender-body">
            <div class="week-grid week-days">
                <div v-for="day in weekdays" :value="day.value">{{ day.text }}</div>
            </div>
            <div class="week-grid">
                <div v-for="nd in monthStartDay"></div>
                <div :class="[{ 'selected-date': calender.date == date && calender.month.value == value.value?.month.value }, 
                {'disabled-item':disableDate(date)},
                'date-item']" v-for="date in currentYearMonths[this.calender.month.value]" @click="updateDate(date)">{{
                date }}
                </div>
            </div>
        </div>
    </div>
</template>
    