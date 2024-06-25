<script setup lang="ts">
import moment from 'jalali-moment'
import { ref } from 'vue'

import Months from './Months.vue'

defineProps<{
  msg: string
}>()
const fromDate = ref('')
const toDate = ref('')
const calendarType = ref('isJalali')
const isRoundtrip = ref(false)

const todayDay = ref(moment().locale('fa').format('DD'))
const todayMonth = ref(moment().locale('fa').format('MM'))
const todayYear = ref(moment().locale('fa').format('YYYY'))

const mainYear = ref(todayYear)
const mainMonth = ref(todayMonth)

const monthElement = ref([])
const emptyDayElement = ref([])

const showDropdown = ref(false)
const jalaliMonths = [
  {
    id: 1,
    value: '01',
    title: 'فروردین'
  },
  {
    id: 2,
    value: '02',
    title: 'اردیبهشت'
  },
  {
    id: 3,
    value: '03',
    title: 'خرداد'
  },
  {
    id: 4,
    value: '04',
    title: 'تیر'
  },
  {
    id: 5,
    value: '05',
    title: 'مرداد'
  },
  {
    id: 6,
    value: '06',
    title: 'شهریور'
  },
  {
    id: 7,
    value: '07',
    title: 'مهر'
  },
  {
    id: 8,
    value: '08',
    title: 'آبان'
  },
  {
    id: 9,
    value: '09',
    title: 'آذر'
  },
  {
    id: 10,
    value: '10',
    title: 'دی'
  },
  {
    id: 11,
    value: '11',
    title: 'بهمن'
  },
  {
    id: 12,
    value: '12',
    title: 'اسفند'
  }
]
const gregorianMonths = [
  {
    id: 1,
    value: '01',
    title: 'January'
  },
  {
    id: 2,
    value: '02',
    title: 'February'
  },
  {
    id: 3,
    value: '03',
    title: 'March'
  },
  {
    id: 4,
    value: '04',
    title: 'April'
  },
  {
    id: 5,
    value: '05',
    title: 'May'
  },
  {
    id: 6,
    value: '06',
    title: 'June'
  },
  {
    id: 7,
    value: '07',
    title: 'July'
  },
  {
    id: 8,
    value: '08',
    title: 'August'
  },
  {
    id: 9,
    value: '09',
    title: 'September'
  },
  {
    id: 10,
    value: '10',
    title: 'October'
  },
  {
    id: 11,
    value: '11',
    title: 'November'
  },
  {
    id: 12,
    value: '12',
    title: 'December'
  }
]

const datepicker = ref({
  monthCount: 2,
  activeMonth: 2,
  months: []
})

const setMonths = () => {
  let dateMonth = +mainMonth
  let dateYear = +mainYear
  datepicker.months.length = 0
  for (let index = 0; index < datepicker.monthCount; index++) {
    let month = jalaliMonths.find((x) => x.id == dateMonth)
    if (calendarType.value === 'isGregorian') {
      month = gregorianMonths.find((x) => x.id == dateMonth)
    }

    console.log('month', month, dateMonth)
    datepicker.months.push({
      id: month.id,
      value: month.value,
      title: month.title,
      year: dateYear,
      days: []
    })

    dateYear = dateMonth === 12 ? dateYear + 1 : dateYear
    dateMonth = dateMonth === 12 ? 1 : dateMonth + 1
  }
  console.log('datepicker', datepicker)
}

const setDays = () => {
  datepicker.value.months.forEach((month) => {
    month.days.length = 0
    const startMonth = moment(month.year + '/' + month.id + '/01', 'jYYYY/jM/jD')
    const dayCount = startMonth.jDaysInMonth()
    for (let day = 0; day < dayCount; day++) {
      const date = moment(month.year + '/' + month.id + '/' + (day + 1), 'jYYYY/jM/jD')
      const dayOfWeek = (date.day() + 1) % 7
      const holiday = dayOfWeek === 6 ? true : false
      // console.log("date2", date.jDaysInMonth());
      if (date.isValid()) {
        month.days.push({
          id: day + 1,
          value: (day + 1 < 10 ? '0' : '') + (day + 1),
          price: null,
          state: 'active',
          holiday: holiday,
          dayOfWeek: dayOfWeek
        })
      }
    }
  })
}

const selectDate = (year: string, month: string, day: string) => {
  const date = year + '/' + month + '/' + day
  console.log('select date', date)
  fromDate.value = date
}

const arrowMonth = (type: string) => {
  const date = mainYear + '/' + mainMonth + '/01'

  let newYear = moment(date).add(1, 'month').format('YYYY')
  let newMonth = moment(date).add(1, 'month').format('MM')

  if (type === 'right') {
    newYear = moment(date).add(-1, 'month').format('YYYY')
    newMonth = moment(date).add(-1, 'month').format('MM')
  }

  mainYear.value = newYear
  mainMonth.value = newMonth
}

const changeCalendar = () => {
  const mainDate = mainYear + '/' + mainMonth + '/15'
  console.log('mainDate', mainDate, moment.from(mainDate, 'fa', 'YYYY/MM/DD').format('YYYY'))
  if (calendarType.value === 'isJalali') {
    calendarType.value = 'isGregorian'

    // set main date for gregorean
    mainYear.value = moment.from(mainDate, 'fa', 'YYYY/MM/DD').format('YYYY')
    mainMonth.value = moment.from(mainDate, 'fa', 'YYYY/MM/DD').format('MM')

    // set today for gregorean
    todayYear.value = moment().format('YYYY')
    todayMonth.value = moment().format('MM')
    todayDay.value = moment().format('DD')
    return
  }
  calendarType.value = 'isJalali'

  // set main date for jalali
  mainYear.value = moment(mainDate).locale('fa').format('YYYY')
  mainMonth.value = moment(mainDate).locale('fa').format('MM')

  // set today for jalali
  todayYear.value = moment().locale('fa').format('YYYY')
  todayMonth.value = moment().locale('fa').format('MM')
  todayDay.value = moment().locale('fa').format('DD')
}

const goToToday = () => {
  const mainDate = todayYear + '/' + todayMonth + '/' + todayDay

  mainYear.value = moment(mainDate).format('YYYY')
  mainMonth.value = moment(mainDate).format('MM')
}
</script>

<template>
  <h1>react Datepicker</h1>
  <div class="date-picker">
    <div class="inputs">
      <div class="from-date">
        <label htmlFor="exampleDataList" class="form-label">
          {{ isRoundtrip ? 'از' : '' }}
          تاریخ:
        </label>
        <input
          class="form-control"
          list="datalistOptions"
          id="exampleDataList"
          placeholder=""
          :value="fromDate"
          @click="showDropdown = true"
        />
      </div>
      <div class="to-date" v-if="isRoundtrip">
        <label htmlFor="exampleDataList" class="form-label"> تا تاریخ: </label>
        <input
          class="form-control"
          list="datalistOptions"
          id="exampleDataList"
          placeholder=""
          :value="toDate"
        />
      </div>
    </div>
    <div class="dropdown box">
      <div :class="'dropdown-menu p-4 text-body-secondary ' + (showDropdown ? 'show' : '')">
        <div class="top-action"></div>
        <div class="arrow">
          <div class="arrow-action" @click="arrowMonth('right')">
            {{ '<' }}
          </div>
          <div class="arrow-action" @click="arrowMonth('left')">
            {{ '>' }}
          </div>
        </div>
        <div :class="'dates ' + calendarType">
          <Months />
        </div>
        <div class="bottom-action d-flex justify-content-between">
          <button class="btn btn-link" @click="goToToday()">برو امروز</button>
          <button class="btn btn-link" @click="changeCalendar()">
            {{ calendarType === 'isJalali' ? 'تقویم میلادی' : 'تقویم شمسی' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
