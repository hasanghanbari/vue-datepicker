<script setup lang="ts">
defineProps<{
  month: any
  calendarType: string
  todayYear: string
  todayMonth: string
  todayDay: string
  fromDate: string
  toDate: string
  selectDate(year: string, month: string, day: string): void
}>()
const checkHighlight = (date: string) => {
  return false
}
const checkPassed = (date: string) => {
  return false
}
// const selectDate = (year: string, month: string, day: string) => {
//   return false
// }
</script>

<template>
  <div class="month" :key="'index' + 1">
    <h5>{{ month.title }} {{ month.year }}</h5>
    <div class="calendar-grid">
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'ش' : 's' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'ی' : 's' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'د' : 'm' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'س' : 't' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'چ' : 'w' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'پ' : 't' }} </span>
      <span class="calendar-weekday"> {{ calendarType === 'isJalali' ? 'ج' : 'f' }} </span>
      <span
        v-for="(day, dayIndex) in month.days"
        :class="
          'calendar-cell' +
          (+todayYear === month.year,
          +todayMonth === +month.value && +todayDay === +day.value ? 'is_today ' : '') +
          (day.holiday ? 'is_holiday ' : '') +
          (month.year + '/' + month.value + '/' + day.value == fromDate ? 'is_selected ' : '') +
          (month.year + '/' + month.value + '/' + day.value == toDate ? 'is_selected ' : '') +
          (checkHighlight(month.year + '/' + month.value + '/' + day.value)
            ? 'is_highlight '
            : '') +
          (checkPassed(month.year + '/' + month.value + '/' + day.value) ? 'is_passed ' : '')
        "
        :key="dayIndex"
        @Click="
          !checkPassed(month.year + '/' + month.value + '/' + day.value) &&
            selectDate(month.year, month.value, day.value)
        "
      >
        {{ day.id }}
      </span>
    </div>
  </div>
</template>
<style></style>
