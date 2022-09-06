<style scoped>
.nepalicalender-main {
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
    grid-template-columns: 35px 35px 35px 35px 35px 35px 35px;
}

.week-grid div,
.nepalicalender-head div {
    height: 35px;
    display: inline-flex;
    justify-content: center;
    align-items: center;
}

.nepalicalender-head {
    display: grid;
    grid-template-columns: 35px auto 35px;
    justify-content: space-between;
}

.date-item {
    cursor: pointer;
}

.date-item:hover {
    border: 1px solid skyblue;
}

.selected-date {
    border: 1px solid skyblue;
}
</style>
<script>
export default {
    name: 'nepalidatepicker',
    inheritAttrs: false,
    data() {
        return {
            calender: {
                month: null,
                year: null,
            },
            _dates: null,
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
                { text: 'Baisakh', value: 0 },
                { text: 'Jestha', value: 1 },
                { text: 'Ashar', value: 2 },
                { text: 'Shrawan', value: 3 },
                { text: 'Bhadra', value: 4 },
                { text: 'Aswin', value: 5 },
                { text: 'Kartik', value: 6 },
                { text: 'Mangsir', value: 7 },
                { text: 'Poush', value: 8 },
                { text: 'Magh', value: 9 },
                { text: 'Falgun', value: 10 },
                { text: 'Chaitra', value: 11 },
            ],
        }
    },
    props: ['value', 'options', 'today'],
    created() {
        this._dates = this.options;
        if (!this.value.value) {
            this.calender.year = this._dates.find(x => x.year == this.today.year.year);
            this.calender.month = this.month.find(x => x.value == (parseFloat(this.today.month.value)));
            this.calender.date = this.today.date;
        } else {
            console.log(this.value.value, "value date")
            this.calender.month = this.month.find(x => x.value == this.value.value.month?.value);
            this.calender.year = this._dates.find(x => x.year == this.value.value.year.year);
            this.calender.date = this.value.value.date;
        }
    },
    mounted() {

    },
    computed: {
    },
    methods: {
        getMonthStartDay() {
            return (this.calender.year?.startDay + this.calender.year.value.slice(0, this.calender.month.value).reduce((s, i) => s + i, 0)) % 7;
        },

        nextMonth() {
            let _month = (this.calender.month.value + 1) % 12;
            if (_month == 0) {
                var _year = this._dates.find(x => x.year == (this.calender.year.year + 1));
                if (_year) {
                    this.calender.year = _year;
                }
            }
            this.calender.month = this.month.find(x => x.value == _month);
        },
        prevMonth() {
            let _month = (12 + this.calender.month.value) % 12;
            if (_month == 0) {
                var _year = this._dates.find(x => x.year == (this.calender.year.year - 1));
                if (_year) {
                    this.calender.year = _year;
                }
            }
            this.calender.month = this.month.find(x => x.value == _month);;
        },
        updateDate(date) {
            // this.$emit('change', );
            this.calender.date = date;
            this.$parent.updateNepaliDate(this.calender);
        },
    }
}
</script>
<script setup>
</script>
<template>
    <div class="nepalicalender-main">
        <div class="nepalicalender-head">
            <div class="head-left at-center">
                <button type="button" @click="prevMonth()">{{ '<' }}</button>
            </div>
            <div class="head-center space-betn">
                <select v-model="calender.year">
                    <option v-for="year in _dates" :value="year">{{ year.year }}</option>
                </select>
                <select v-model="calender.month">
                    <option v-for="mon in month" :value="mon">{{ mon.text }}</option>
                </select>
            </div>
            <div class="head-right at-center">
                <button type="button" @click="nextMonth()">{{ '>' }}</button>
            </div>
        </div>
        <div class="nepalicalender-body">
            <div class="week-grid week-days">
                <div v-for="day in weekdays" :value="day.value">{{ day.text }}</div>
            </div>
            <div class="week-grid">
                <div class="date-item" v-for="nd in getMonthStartDay()"></div>
                <div :class="[{ 'selected-date': calender.date == date && calender.month.value == value.value?.month.value }, 'date-item']"
                    v-for="date in calender.year.value[calender.month.value]" @click="updateDate(date)">{{ date }}</div>
            </div>
        </div>
    </div>
</template>
    