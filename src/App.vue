<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  data: {
    currentMonth: 0,
    currentYear: 2023,
    days: [],
    monthNames: [
      'January',
      'February',
      'March',
      'April',
      'May',
      'June',
      'July',
      'August',
      'September',
      'October',
      'November',
      'December',
    ],
  },
  mounted: function () {    
    // TODO Wrong days from concat in getGrid - fix 
    this.runTests()
    const now = new Date();
    this.currentMonth = now.getMonth();
    this.currentYear = now.getFullYear();
    this.days = this.getGrid()
  },
  methods: {
    prevMonth: function() {

    },
    nextMonth: function() {
      const next = new Date(this.currentYear, this.currentMonth + 1, 1);
      this.currentMonth = next.getMonth();
      this.currentYear = next.getFullYear();
      this.days = this.getGrid();
    },
    runTests: function () {
      //Test May 2023 - 5 rows x 7
      let expected = [
        30, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29,
        30, 31, 1, 2, 3,
      ]
      this.currentMonth = 4;
      this.compare(expected, this.getGrid())
      //Test July 2023 - 6 rows x 7
      expected = [
        25, 26, 27, 28, 29, 30, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24,
        25, 26, 27, 28, 29, 30, 31, 1, 2, 3, 4, 5,
      ]
      this.currentMonth = 6;
      this.compare(expected, this.getGrid())
    },
    compare: function (expected, actual) {
      console.log('expected', expected)
      console.log('actual', actual)
      console.assert(expected.toString() === actual.toString())
    },
    getGrid: function () {
      console.log('START HERE...');

      const firstDayCurrMonth = new Date(this.currentYear, this.currentMonth, 1);
      const copyDate = new Date(this.currentYear, this.currentMonth, 1);
      const lastDayThisMonth = new Date(firstDayCurrMonth.getFullYear(), firstDayCurrMonth.getMonth() + 1, 0)
      const lastDayPrevMonth = new Date(copyDate.setDate(0))      
      const daysPrevMonth = this.getDaysInMonth(lastDayPrevMonth)
      const daysSelectedMonth = this.getDaysInMonth(firstDayCurrMonth)
      console.log('daysPrevMonth', daysPrevMonth);
      console.log('daysSelectedMonth', daysSelectedMonth);
      console.log('lastDayThisMonth', lastDayThisMonth);
      //console.log('lastDayThisMonth.getDay()',  lastDayThisMonth.getDay());
      const fragmentLastMonth = firstDayCurrMonth.getDay() === 0 ? [] : daysPrevMonth.slice(-firstDayCurrMonth.getDay())
      console.log('firstDayCurrMonth.getDay()', firstDayCurrMonth.getDay());
      console.log('days I need from prev month', fragmentLastMonth);
      const numDaysNeededNextMonth = 7 - (lastDayThisMonth.getDay() + 1);
      console.log('numDaysNeededNextMonth', numDaysNeededNextMonth);
      const fragmentNextMonth = [1, 2, 3, 4, 5, 6, 7].slice(0, numDaysNeededNextMonth)
      console.log('days I need from next month', fragmentNextMonth);
      return [].concat(fragmentLastMonth, daysSelectedMonth, fragmentNextMonth)
    },
    getDaysInMonth: function (aDate) {
      var date = new Date(aDate.getFullYear(), aDate.getMonth(), 1)
      var days = []
      while (date.getMonth() === aDate.getMonth()) {
        days.push(new Date(date).getDate())
        date.setDate(date.getDate() + 1)
      }
      return days
    },
  },
})
</script>

<template>
  <div id="app">
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@3.4.1/dist/css/bootstrap.min.css"
      integrity="sha384-HSMxcRTRxnN+Bdg0JdbxYKrThecOKuH5zCYotlSAcp1+c8xmyTe9GYg1l9a69psu"
      crossorigin="anonymous"
    />
  	<span>{{ currentMonth }}/{{ currentYear }}</span>
    <div style="background-color: beige; padding: 10px; display: inline-block">
      <span>{{ monthNames[currentMonth] }}</span><button v-on:click="prevMonth"> < </button> | <button v-on:click="nextMonth">></button>
      <table class="pcccal">
        <thead>
          <tr>
            <th>Sun</th>
            <th>Mon</th>
            <th>Tue</th>
            <th>Wed</th>
            <th>Thu</th>
            <th>Fri</th>
            <th>Sat</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="row in days.length / 7">
            <td :row="row" :day="day" v-for="day in days.slice((row - 1) * 7, row * 7)" class="selectable">
              {{ days[day - 1] }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style>
.pcccal {
  border-collapse: collapse;
  width: 100%;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  background: beige;
  width: 224px;
  font-size: 12px;
}

.pcccal thead {
  margin: 0;
}

.pcccal th,
.pcccal td {
  width: 14.2857142%;
  border: 1px solid #ddd;
  color: #666;
  text-align: right;
  vertical-align: middle;
  padding: 0;
  border-collapse: collapse;
  cursor: pointer;
  height: 25px;
}

.pcccal td {
  padding-right: 5px;
}

.pcccal .selectable {
  padding-right: 5px;
  background: #f5f5f5;
}

.pcccal th {
  background: #f5f5f5;
  font-weight: 700;
  text-align: center;
  color: #999;
  cursor: default;
}

.pcccal .selectable {
  background: #f5f5f5;
}

.ajg-time {
  background: #f5f5f5;
  border-top: 1px solid #ddd;
  color: #666;
  font-size: 12px;
  text-align: center;
  border-collapse: collapse;
  cursor: pointer;
  border-bottom-width: 0;
  height: 25px;
  line-height: 25px;
}

/* Top level */
.xdsoft_datetimepicker {
  box-shadow: 0 5px 15px -5px rgba(0, 0, 0, 0.506);
  background: beige;
  border-bottom: 1px solid #bbb;
  border-left: 1px solid #ccc;
  border-right: 1px solid #ccc;
  border-top: 1px solid #ccc;
  color: #333;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  padding: 8px;
  padding-left: 0;
  padding-top: 2px;
  position: absolute;
  z-index: 9999;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
.xdsoft_datetimepicker.xdsoft_rtl {
  padding: 8px 0 8px 8px;
}

.xdsoft_datetimepicker iframe {
  position: absolute;
  left: 0;
  top: 0;
  width: 75px;
  height: 210px;
  background: transparent;
  border: none;
}

/*For IE8 or lower*/
.xdsoft_datetimepicker button {
  border: none !important;
}

.xdsoft_noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.xdsoft_noselect::selection {
  background: transparent;
}
.xdsoft_noselect::-moz-selection {
  background: transparent;
}

.xdsoft_datetimepicker.xdsoft_inline {
  display: inline-block;
  position: static;
  box-shadow: none;
}

.xdsoft_datetimepicker * {
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

.xdsoft_datetimepicker .xdsoft_datepicker,
.xdsoft_datetimepicker .xdsoft_timepicker {
  display: none;
}

.xdsoft_datetimepicker .xdsoft_datepicker.active,
.xdsoft_datetimepicker .xdsoft_timepicker.active {
  display: block;
}

.xdsoft_datetimepicker .xdsoft_datepicker {
  width: 224px;
  float: left;
  margin-left: 8px;
}
.xdsoft_datetimepicker.xdsoft_rtl .xdsoft_datepicker {
  float: right;
  margin-right: 8px;
  margin-left: 0;
}

.xdsoft_datetimepicker.xdsoft_showweeks .xdsoft_datepicker {
  width: 256px;
}

.xdsoft_datetimepicker .xdsoft_timepicker {
  width: 58px;
  float: left;
  text-align: center;
  margin-left: 8px;
  margin-top: 0;
}
.xdsoft_datetimepicker.xdsoft_rtl .xdsoft_timepicker {
  float: right;
  margin-right: 8px;
  margin-left: 0;
}

.xdsoft_datetimepicker .xdsoft_datepicker.active + .xdsoft_timepicker {
  margin-top: 8px;
  margin-bottom: 3px;
}

.xdsoft_datetimepicker .xdsoft_monthpicker {
  position: relative;
  text-align: center;
}

.xdsoft_datetimepicker .xdsoft_label i,
.xdsoft_datetimepicker .xdsoft_prev,
.xdsoft_datetimepicker .xdsoft_next,
.xdsoft_datetimepicker .xdsoft_today_button {
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAeCAYAAADaW7vzAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6Q0NBRjI1NjM0M0UwMTFFNDk4NkFGMzJFQkQzQjEwRUIiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6Q0NBRjI1NjQ0M0UwMTFFNDk4NkFGMzJFQkQzQjEwRUIiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpDQ0FGMjU2MTQzRTAxMUU0OTg2QUYzMkVCRDNCMTBFQiIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpDQ0FGMjU2MjQzRTAxMUU0OTg2QUYzMkVCRDNCMTBFQiIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PoNEP54AAAIOSURBVHja7Jq9TsMwEMcxrZD4WpBYeKUCe+kTMCACHZh4BFfHO/AAIHZGFhYkBBsSEqxsLCAgXKhbXYOTxh9pfJVP+qutnZ5s/5Lz2Y5I03QhWji2GIcgAokWgfCxNvcOCCGKqiSqhUp0laHOne05vdEyGMfkdxJDVjgwDlEQgYQBgx+ULJaWSXXS6r/ER5FBVR8VfGftTKcITNs+a1XpcFoExREIDF14AVIFxgQUS+h520cdud6wNkC0UBw6BCO/HoCYwBhD8QCkQ/x1mwDyD4plh4D6DDV0TAGyo4HcawLIBBSLDkHeH0Mg2yVP3l4TQMZQDDsEOl/MgHQqhMNuE0D+oBh0CIr8MAKyazBH9WyBuKxDWgbXfjNf32TZ1KWm/Ap1oSk/R53UtQ5xTh3LUlMmT8gt6g51Q9p+SobxgJQ/qmsfZhWywGFSl0yBjCLJCMgXail3b7+rumdVJ2YRss4cN+r6qAHDkPWjPjdJCF4n9RmAD/V9A/Wp4NQassDjwlB6XBiCxcJQWmZZb8THFilfy/lfrTvLghq2TqTHrRMTKNJ0sIhdo15RT+RpyWwFdY96UZ/LdQKBGjcXpcc1AlSFEfLmouD+1knuxBDUVrvOBmoOC/rEcN7OQxKVeJTCiAdUzUJhA2Oez9QTkp72OTVcxDcXY8iKNkxGAJXmJCOQwOa6dhyXsOa6XwEGAKdeb5ET3rQdAAAAAElFTkSuQmCC);
}

.xdsoft_datetimepicker .xdsoft_label i {
  opacity: 0.5;
  background-position: -92px -19px;
  display: inline-block;
  width: 9px;
  height: 20px;
  vertical-align: middle;
}

.xdsoft_datetimepicker .xdsoft_prev {
  float: left;
  background-position: -20px 0;
}
.xdsoft_datetimepicker .xdsoft_today_button {
  float: left;
  background-position: -70px 0;
  margin-left: 5px;
}

.xdsoft_datetimepicker .xdsoft_next {
  float: right;
  background-position: 0 0;
}

.xdsoft_datetimepicker .xdsoft_next,
.xdsoft_datetimepicker .xdsoft_prev,
.xdsoft_datetimepicker .xdsoft_today_button {
  background-color: transparent;
  background-repeat: no-repeat;
  border: 0 none;
  cursor: pointer;
  display: block;
  height: 30px;
  opacity: 0.5;
  -ms-filter: 'progid:DXImageTransform.Microsoft.Alpha(Opacity=50)';
  outline: medium none;
  overflow: hidden;
  padding: 0;
  position: relative;
  text-indent: 100%;
  white-space: nowrap;
  width: 20px;
  min-width: 0;
}

.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_prev,
.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_next {
  float: none;
  background-position: -40px -15px;
  height: 15px;
  width: 30px;
  display: block;
  margin-left: 14px;
  margin-top: 7px;
}
.xdsoft_datetimepicker.xdsoft_rtl .xdsoft_timepicker .xdsoft_prev,
.xdsoft_datetimepicker.xdsoft_rtl .xdsoft_timepicker .xdsoft_next {
  float: none;
  margin-left: 0;
  margin-right: 14px;
}

.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_prev {
  background-position: -40px 0;
  margin-bottom: 7px;
  margin-top: 0;
}

.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box {
  height: 151px;
  overflow: hidden;
  border-bottom: 1px solid #ddd;
}

.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div {
  background: #f5f5f5;
  border-top: 1px solid #ddd;
  color: #666;
  font-size: 12px;
  text-align: center;
  border-collapse: collapse;
  cursor: pointer;
  border-bottom-width: 0;
  height: 25px;
  line-height: 25px;
}

.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div:first-child {
  border-top-width: 0;
}

.xdsoft_datetimepicker .xdsoft_today_button:hover,
.xdsoft_datetimepicker .xdsoft_next:hover,
.xdsoft_datetimepicker .xdsoft_prev:hover {
  opacity: 1;
  -ms-filter: 'progid:DXImageTransform.Microsoft.Alpha(Opacity=100)';
}

.xdsoft_datetimepicker .xdsoft_label {
  display: inline;
  position: relative;
  z-index: 9999;
  margin: 0;
  padding: 5px 3px;
  font-size: 14px;
  line-height: 20px;
  font-weight: bold;
  background-color: #fff;
  float: left;
  width: 182px;
  text-align: center;
  cursor: pointer;
}

/*
.xdsoft_datetimepicker .xdsoft_label:hover>span {
	text-decoration: underline;
}
*/

.xdsoft_datetimepicker .xdsoft_label:hover i {
  opacity: 1;
}

.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select {
  border: 1px solid #ccc;
  position: absolute;
  right: 0;
  top: 30px;
  z-index: 101;
  display: none;
  background: #fff;
  max-height: 160px;
  overflow-y: hidden;
}

.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select.xdsoft_monthselect {
  right: -7px;
}
.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select.xdsoft_yearselect {
  right: 2px;
}
.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select > div > .xdsoft_option:hover {
  color: #fff;
  background: #ff8000;
}

.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select > div > .xdsoft_option {
  padding: 2px 10px 2px 5px;
  text-decoration: none !important;
}

.xdsoft_datetimepicker .xdsoft_label > .xdsoft_select > div > .xdsoft_option.xdsoft_current {
  background: #33aaff;
  box-shadow: #178fe5 0 1px 3px 0 inset;
  color: #fff;
  font-weight: 700;
}

.xdsoft_datetimepicker .xdsoft_month {
  width: 100px;
  text-align: right;
}

.xdsoft_datetimepicker .xdsoft_calendar {
  clear: both;
}

.xdsoft_datetimepicker .xdsoft_year {
  width: 48px;
  margin-left: 5px;
}

.xdsoft_datetimepicker .xdsoft_calendar table {
  border-collapse: collapse;
  width: 100%;
}

.xdsoft_datetimepicker .xdsoft_calendar td > div {
  padding-right: 5px;
}

.xdsoft_datetimepicker .xdsoft_calendar th {
  height: 25px;
}

.xdsoft_datetimepicker .xdsoft_calendar td,
.xdsoft_datetimepicker .xdsoft_calendar th {
  width: 14.2857142%;
  background: #f5f5f5;
  border: 1px solid #ddd;
  color: #666;
  font-size: 12px;
  text-align: right;
  vertical-align: middle;
  padding: 0;
  border-collapse: collapse;
  cursor: pointer;
  height: 25px;
}
.xdsoft_datetimepicker.xdsoft_showweeks .xdsoft_calendar td,
.xdsoft_datetimepicker.xdsoft_showweeks .xdsoft_calendar th {
  width: 12.5%;
}

.xdsoft_datetimepicker .xdsoft_calendar th {
  background: #f1f1f1;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_today {
  color: #33aaff;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_highlighted_default {
  background: #ffe9d2;
  box-shadow: #ffb871 0 1px 4px 0 inset;
  color: #000;
}
.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_highlighted_mint {
  background: #c1ffc9;
  box-shadow: #00dd1c 0 1px 4px 0 inset;
  color: #000;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_default,
.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_current,
.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div.xdsoft_current {
  background: #33aaff;
  box-shadow: #178fe5 0 1px 3px 0 inset;
  color: #fff;
  font-weight: 700;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_other_month,
.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_disabled,
.xdsoft_datetimepicker .xdsoft_time_box > div > div.xdsoft_disabled {
  opacity: 0.5;
  -ms-filter: 'progid:DXImageTransform.Microsoft.Alpha(Opacity=50)';
  cursor: default;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_other_month.xdsoft_disabled {
  opacity: 0.2;
  -ms-filter: 'progid:DXImageTransform.Microsoft.Alpha(Opacity=20)';
}

.xdsoft_datetimepicker .xdsoft_calendar td:hover,
.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div:hover {
  color: #fff !important;
  background: #ff8000 !important;
  box-shadow: none !important;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_current.xdsoft_disabled:hover,
.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div.xdsoft_current.xdsoft_disabled:hover {
  background: #33aaff !important;
  box-shadow: #178fe5 0 1px 3px 0 inset !important;
  color: #fff !important;
}

.xdsoft_datetimepicker .xdsoft_calendar td.xdsoft_disabled:hover,
.xdsoft_datetimepicker .xdsoft_timepicker .xdsoft_time_box > div > div.xdsoft_disabled:hover {
  color: inherit !important;
  background: inherit !important;
  box-shadow: inherit !important;
}

.xdsoft_datetimepicker .xdsoft_calendar th {
  font-weight: 700;
  text-align: center;
  color: #999;
  cursor: default;
}

.xdsoft_datetimepicker .xdsoft_copyright {
  color: #ccc !important;
  font-size: 10px;
  clear: both;
  float: none;
  margin-left: 8px;
}

.xdsoft_datetimepicker .xdsoft_copyright a {
  color: #eee !important;
}
.xdsoft_datetimepicker .xdsoft_copyright a:hover {
  color: #aaa !important;
}

.xdsoft_time_box {
  position: relative;
  border: 1px solid #ccc;
}
.xdsoft_scrollbar > .xdsoft_scroller {
  background: #ccc !important;
  height: 20px;
  border-radius: 3px;
}
.xdsoft_scrollbar {
  position: absolute;
  width: 7px;
  right: 0;
  top: 0;
  bottom: 0;
  cursor: pointer;
}
.xdsoft_datetimepicker.xdsoft_rtl .xdsoft_scrollbar {
  left: 0;
  right: auto;
}
.xdsoft_scroller_box {
  position: relative;
}

.xdsoft_datetimepicker.xdsoft_dark {
  box-shadow: 0 5px 15px -5px rgba(255, 255, 255, 0.506);
  background: #000;
  border-bottom: 1px solid #444;
  border-left: 1px solid #333;
  border-right: 1px solid #333;
  border-top: 1px solid #333;
  color: #ccc;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_timepicker .xdsoft_time_box {
  border-bottom: 1px solid #222;
}
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_timepicker .xdsoft_time_box > div > div {
  background: #0a0a0a;
  border-top: 1px solid #222;
  color: #999;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_label {
  background-color: #000;
}
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_label > .xdsoft_select {
  border: 1px solid #333;
  background: #000;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_label > .xdsoft_select > div > .xdsoft_option:hover {
  color: #000;
  background: #007fff;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_label > .xdsoft_select > div > .xdsoft_option.xdsoft_current {
  background: #cc5500;
  box-shadow: #b03e00 0 1px 3px 0 inset;
  color: #000;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_label i,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_prev,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_next,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_today_button {
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAeCAYAAADaW7vzAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6QUExQUUzOTA0M0UyMTFFNDlBM0FFQTJENTExRDVBODYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6QUExQUUzOTE0M0UyMTFFNDlBM0FFQTJENTExRDVBODYiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpBQTFBRTM4RTQzRTIxMUU0OUEzQUVBMkQ1MTFENUE4NiIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpBQTFBRTM4RjQzRTIxMUU0OUEzQUVBMkQ1MTFENUE4NiIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/Pp0VxGEAAAIASURBVHja7JrNSgMxEMebtgh+3MSLr1T1Xn2CHoSKB08+QmR8Bx9A8e7RixdB9CKCoNdexIugxFlJa7rNZneTbLIpM/CnNLsdMvNjM8l0mRCiQ9Ye61IKCAgZAUnH+mU3MMZaHYChBnJUDzWOFZdVfc5+ZFLbrWDeXPwbxIqrLLfaeS0hEBVGIRQCEiZoHQwtlGSByCCdYBl8g8egTTAWoKQMRBRBcZxYlhzhKegqMOageErsCHVkk3hXIFooDgHB1KkHIHVgzKB4ADJQ/A1jAFmAYhkQqA5TOBtocrKrgXwQA8gcFIuAIO8sQSA7hidvPwaQGZSaAYHOUWJABhWWw2EMIH9QagQERU4SArJXo0ZZL18uvaxejXt/Em8xjVBXmvFr1KVm/AJ10tRe2XnraNqaJvKE3KHuUbfK1E+VHB0q40/y3sdQSxY4FHWeKJCunP8UyDdqJZenT3ntVV5jIYCAh20vT7ioP8tpf6E2lfEMwERe+whV1MHjwZB7PBiCxcGQWwKZKD62lfGNnP/1poFAA60T7rF1UgcKd2id3KDeUS+oLWV8DfWAepOfq00CgQabi9zjcgJVYVD7PVzQUAUGAQkbNJTBICDhgwYTjDYD6XeW08ZKh+A4pYkzenOxXUbvZcWz7E8ykRMnIHGX1XPl+1m2vPYpL+2qdb8CDAARlKFEz/ZVkAAAAABJRU5ErkJggg==);
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar th {
  background: #0a0a0a;
  border: 1px solid #222;
  color: #999;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar th {
  background: #0e0e0e;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td.xdsoft_today {
  color: #cc5500;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td.xdsoft_highlighted_default {
  background: #ffe9d2;
  box-shadow: #ffb871 0 1px 4px 0 inset;
  color: #000;
}
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td.xdsoft_highlighted_mint {
  background: #c1ffc9;
  box-shadow: #00dd1c 0 1px 4px 0 inset;
  color: #000;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td.xdsoft_default,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td.xdsoft_current,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_timepicker .xdsoft_time_box > div > div.xdsoft_current {
  background: #cc5500;
  box-shadow: #b03e00 0 1px 3px 0 inset;
  color: #000;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar td:hover,
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_timepicker .xdsoft_time_box > div > div:hover {
  color: #000 !important;
  background: #007fff !important;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_calendar th {
  color: #666;
}

.xdsoft_datetimepicker.xdsoft_dark .xdsoft_copyright {
  color: #333 !important;
}
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_copyright a {
  color: #111 !important;
}
.xdsoft_datetimepicker.xdsoft_dark .xdsoft_copyright a:hover {
  color: #555 !important;
}

.xdsoft_dark .xdsoft_time_box {
  border: 1px solid #333;
}

.xdsoft_dark .xdsoft_scrollbar > .xdsoft_scroller {
  background: #333 !important;
}
/*
.xdsoft_datetimepicker .xdsoft_save_selected {
    display: block;
    border: 1px solid #dddddd !important;
    margin-top: 5px;
    width: 100%;
    color: #454551;
    font-size: 13px;
}
*/
</style>
