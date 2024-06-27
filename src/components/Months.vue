<script setup lang="ts">
const props = defineProps<{
  month: any
  calendarType: string
  todayYear: string
  todayMonth: string
  todayDay: string
  fromDate: string
  toDate: string
}>()

const checkHighlight = (date: string) => {
  if (props.fromDate === '' || props.toDate === '') {
    return false
  }

  const [curYear, curMonth, curDay] = date.split('/').map(Number)
  const [fromYear, fromMonth, fromDay] = props.fromDate.split('/').map(Number)
  const [toYear, toMonth, toDay] = props.toDate.split('/').map(Number)

  if (
    curYear >= fromYear &&
    curYear <= toYear &&
    ((fromYear !== toYear &&
      ((curMonth === fromMonth && curDay >= fromDay) ||
        (curMonth === toMonth && curDay <= toDay) ||
        (curMonth !== fromMonth &&
          curMonth !== toMonth &&
          (curMonth > fromMonth || curMonth < toMonth)))) ||
      (fromYear === toYear &&
        fromMonth !== toMonth &&
        ((curMonth === fromMonth && curDay >= fromDay) ||
          (curMonth === toMonth && curDay <= toDay) ||
          (curMonth !== fromMonth &&
            curMonth !== toMonth &&
            curMonth > fromMonth &&
            curMonth < toMonth))) ||
      (curMonth === fromMonth &&
        curMonth === toMonth &&
        fromYear === toYear &&
        fromMonth === toMonth &&
        curDay > fromDay &&
        curDay < toDay))
  ) {
    return true
  }
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
      <span v-for="emptyIndex in +month.days[0].dayOfWeek" class="" :key="'e' + emptyIndex"></span>
      <span
        v-for="(day, dayIndex) in month.days"
        :class="
          'calendar-cell ' +
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
        @click="
          !checkPassed(month.year + '/' + month.value + '/' + day.value) &&
            $emit('selectDate', month.year, month.value, day.value)
        "
      >
        {{ day.id }}
      </span>
    </div>
  </div>
</template>
<style></style>
