<template>
  <div class="cal-top pa-8">
    <div class="nominate-mark">
      <div class="free">
        <span class="symbol"></span><span class="text">指名なし</span>
      </div>
      <div class="nominate">
        <span class="symbol"></span><span class="text">指名</span>
      </div>
    </div>
    <div class="next-before">
      <v-icon
        class="second-row-btn"
        color="#c3c3c3"
        fab
        @click="calendarLeftEvent()"
        >mdi-chevron-left</v-icon
      >

      <v-toolbar-title v-if="activeListId === 0">{{
        isDayDate
      }}</v-toolbar-title>
      <v-toolbar-title v-else-if="activeListId === 1">{{
        isDayDate
      }}</v-toolbar-title>
      <v-toolbar-title v-else-if="activeListId === 2">{{
        isDayDate
      }}</v-toolbar-title>
      <v-toolbar-title v-else-if="activeListId === 3">{{
        isDayDate
      }}</v-toolbar-title>

      <v-icon
        class="second-row-btn"
        fab
        color="#c3c3c3"
        @click="calendarRightEvent()"
        >mdi-chevron-right</v-icon
      >
    </div>
    <v-radio-group v-model="radios" :mandatory="false" row>
      <v-radio label="ユニット別" value="unit" @click="radiosEvent()"></v-radio>
      <v-radio label="担当者別" value="person" @click="radiosEvent()"></v-radio>
    </v-radio-group>
    <v-btn-toggle v-model="type" mandatory class="button-group">
      <v-btn
        v-for="item in list"
        :key="item.id"
        :value="item.value"
        small
        depressed
        :outlined="item.outlined"
        @click="carendarListBtnEvent(item.id)"
        >{{ item.label }}</v-btn
      >
    </v-btn-toggle>
  </div>
</template>
<script>
export default {
  props: {
    radios: {
      type: String,
      default: 'unit',
    },
  },
  data() {
    return {
      type: 0,
      list: [
        { id: 0, label: '日', outlined: false, value: 'day' },
        { id: 1, label: '週', outlined: true, value: 'week' },
        { id: 2, label: '半月', outlined: true, value: 'custom-weekly' },
        { id: 3, label: '月', outlined: true, value: 'month' },
      ],
      activeListId: 0,
      weeks: ['日', '月', '火', '水', '木', '金', '土'],

      date: new Date(),
      date2: new Date(),
      dateToString: '',
      dateWeekString: '',
      dateCustomWeekString: '',
      dateMonthString: '',
      weekAgoDateToString: '',
    }
  },
  computed: {
    isDayDate() {
      if (this.activeListId === 1)
        return this.dateToString + '.' + this.dateToString
      return this.dateToString
    },
    isRadios() {
      const test = this.radios
      return test
    },
  },
  created() {
    this.setDateToString(this.date, this.dateToString)
    this.$emit('sendDate', this.date)
  },
  methods: {
    calendarLeftEvent() {
      if (this.activeListId === 0) this.changeDay(-1, this.date)
      if (this.activeListId === 1) this.changeDay(-7, this.date)
      if (this.activeListId === 2) this.changeDay(-14, this.date)
      if (this.activeListId === 3) this.changeMonth(-1, this.date)
      this.setDateToString(this.date)
      this.$emit('sendDate', this.date)
    },
    calendarRightEvent() {
      if (this.activeListId === 0) this.changeDay(1, this.date)
      if (this.activeListId === 1) this.changeDay(7, this.date)
      if (this.activeListId === 2) this.changeDay(14, this.date)
      if (this.activeListId === 3) this.changeMonth(1, this.date)
      this.setDateToString(this.date)
      this.$emit('sendDate', this.date)
    },
    setDateToString(date) {
      this.dateToString = this.getDateToFromat(date)
    },
    setWeekAgoDateToString(date) {
      const weekagoDate = new Date(
        date.getFullYear(),
        date.getMonth() + 1,
        date.getDate() + 6
      )
      weekagoDate.setDate(weekagoDate.getDate())
      this.weekAgoDateToString = this.getDateToFromat(weekagoDate)
    },
    getDateToFromat(date) {
      const year = date.getFullYear()
      const month = date.getMonth() + 1
      const day = date.getDate()
      const week = this.weeks[date.getDay()]
      return year + '/' + month + '/' + day + '(' + week + ')'
    },
    changeDay(day, date) {
      date.setDate(date.getDate() + day)
    },
    changeMonth(month, date) {
      date.setMonth(date.getDate() + month)
    },
    changeYear(year, date) {
      date.setFullYear(date.getDate() + year)
    },
    radiosEvent() {
      console.log(this.radios + 'aaaaaaaaaaaa')
      this.$emit('sendRaiosData', this.radios)
    },
    carendarListBtnEvent(type) {
      this.activeListId = type
      this.$emit('sendCarendarList', type)
    },
  },
}
</script>
<style lang="scss" scoped>
.cal-top {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 33px;
  .next-before {
    position: relative;
    width: 230px;
    text-align: center;
    button {
      top: 0;
      bottom: 0;
      margin: auto;
      &:nth-child(1) {
        left: 0;
      }
      &:nth-child(2) {
        right: 0;
      }
    }
  }
  .nominate-mark {
    display: flex;
    & > div {
      display: flex;
      align-items: center;
      margin-right: 19px;
      .symbol {
        display: block;
        width: 66px;
        height: 20px;
        margin-right: 6px;
        border: 2px solid #6e9eff;
        border-radius: 3px;
        background: #e5eeff;
      }
      .text {
        color: #888;
      }
      &.nominate {
        .symbol {
          background: #6e9eff;
        }
      }
    }
  }
  .second-row-btn {
    box-shadow: none !important;
    width: 22px !important;
    height: 22px !important;
    border: 1px solid #c3c3c3;
  }
}
</style>
