<style lang="sass">
.datetime-picker {
    position: relative;
    font-family: "Segoe UI","Lucida Grande",Helvetica,Arial,"Microsoft YaHei";
    -webkit-font-smoothing: antialiased;
    color: #333;

    * {
        box-sizing: border-box;
    }
    input {
        width: 100%;
        padding: 6px 10px;
        outline: 0 none;
        border: 1px solid #ccc;
    }
    .picker-wrap {
        position: absolute;
        width: 238px;
        height: 280px;
        margin-top: 2px;
        background-color: #fff;
        box-shadow: 0 0 6px #ccc;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        border-spacing: 0;
        text-align: center;
        font-size: 13px;
    }
    tr {
        height: 34px;
    }
    th, td {
        user-select: none;
        width: 34px;
        height: 34px;
        padding: 0;
        line-height: 34px;
    }
    td {
        cursor: pointer;
        &:hover {
            background-color: #f0f0f0;
        }
        &.pass,
        &.future {
            color: #aaa;
        }
        &.today {
            background-color: #ececec;
            color: #3bb4f2;
        }
    }
    .date-head {
        background-color: #3bb4f2;
        text-align: center;
        color: #fff;
        font-size: 14px;
    }
    .date-days {
        color: #3bb4f2;
        font-size: 14px;
    }
    .show-year {
        display: inline-block;
        min-width: 62px;
        vertical-align: middle;
    }
    .show-month {
        display: inline-block;
        min-width: 28px;
        vertical-align: middle;
    }
    .btn-prev,
    .btn-next {
        cursor: pointer;
        display: inline-block;
        padding: 0 10px;
        vertical-align: middle;
        &:hover {
            background: rgba(16, 160, 234, 0.5);
        }
    }
}
</style>

<template>
    <div class="datetime-picker" :style="{ width: width }">
        <input
            type="text"
            :style="styleObj"
            :readonly="readonly"
            :value="value"
            @click="show = !show">
        <div class="picker-wrap" v-show="show">
            <table class="date-picker">
                <thead>
                    <tr class="date-head">
                        <th colspan="4">
                            <span class="btn-prev" @click="yearClick(-1)">&lt;</span>
                            <span class="show-year">{{now.getFullYear()}}</span>
                            <span class="btn-next" @click="yearClick(1)">&gt;</span>
                        </th>
                        <th colspan="3">
                            <span class="btn-prev" @click="monthClick(-1)">&lt;</span>
                            <span class="show-month">{{months[now.getMonth()]}}</span>
                            <span class="btn-next" @click="monthClick(1)">&gt;</span>
                        </th>
                    </tr>
                    <tr class="date-days">
                        <th v-for="day in days">{{day}}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="i in 6">
                        <td v-for="j in 7"
                            :class="{ pass: [0, 1].includes(i) && date[i * 7 + j] > 14, future: [4, 5].includes(i) && date[i * 7 + j] < 14, today: date[i * 7 + j] === now.getDate() }"
                            @click="pickDate">{{date[i * 7 + j]}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            width: {type: String, default: '238px' },
            readonly: { type: Boolean, default: false },
            value: { type: String, default: '2016-01-17' }
        },
        data () {
            return {
                show: false,
                days: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
                months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
                date: [],
                now: new Date()
            };
        },
        watch: {
            now () {
                this.update();
            }
        },
        methods: {
            update () {
                var arr = [];
                var setting = new Date(this.now);
                var month = setting.getMonth();
                setting.setMonth(setting.getMonth(), 1);        // the first day
                var curFirstDay = setting.getDay();
                curFirstDay === 0 && (curFirstDay = 7);
                setting.setDate(0);                             // the last day
                var lastDayCount = setting.getDate();
                setting.setMonth(month + 1, 0);                 // the last day of this month
                var curDayCount = setting.getDate();
                setting.setDate(1);                             // fix bug when month change
                for (let i = curFirstDay; i > 0; i--) {
                    arr.push(lastDayCount - i + 1);
                }
                for (let i = 0; i < curDayCount; i++) {
                    arr.push(i + 1);
                }
                var j = 1;
                while (arr.length < 42) {
                    arr.push(j);
                    j++;
                }
                this.date = arr;
            },
            yearClick (flag) {
                this.now.setFullYear(this.now.getFullYear() + flag);
                this.now = new Date(this.now);
            },
            monthClick (flag) {
                this.now.setMonth(this.now.getMonth() + flag);
                this.now = new Date(this.now);
            },
            pickDate (e) {
                this.show = false;
                var year = this.now.getFullYear();
                var month = ('0' + (this.now.getMonth() + 1)).slice(-2);
                var date = ('0' + e.target.textContent).slice(-2);
                this.value = year + '-' + month + '-' + date;
            }
        },
        ready () {
            this.update();
        }
    };
</script>