<template>
  <div>
    <CalendarCommon
      v-if="radios"
      :times="times"
      :first-lists="apiData.unitLists"
      :second-lists="apiData.personInChargeLists"
    >
      <template #reservationbox>
        <div
          v-for="(customerList, index) in customerLists"
          :key="`first-${index}`"
          class="item-handle"
        >
          <ReservationBox
            v-if="DateComparison(dateItem, customerList.start)"
            :row-width="40"
            :customer-info="customerList"
            :box-place-x="
              calcReservationSmallBoxPlaceX(
                customerList.unitId,
                apiData.unitLists,
                customerList.PersonInChargeId,
                apiData.personInChargeLists
              )
            "
            :box-place-y="
              calcReservationBoxPlaceY(
                apiData.Business.openDate,
                customerList.start
              )
            "
          />
        </div>
      </template>
    </CalendarCommon>
    <CalendarDayCommon
      v-else
      :times="times"
      :first-lists="apiData.personInChargeLists"
      :second-lists="apiData.unitLists"
    >
      <template #reservationbox>
        <div
          v-for="(customerList, index) in customerLists"
          :key="`first-${index}`"
          class="item-handle"
        >
          <ReservationBox
            v-if="DateComparison(dateItem, customerList.start)"
            :row-width="100"
            :customer-info="customerList"
            :box-place-x="
              calcReservationBigBoxPlaceX(
                customerList.PersonInChargeId,
                apiData.personInChargeLists
              )
            "
            :box-place-y="
              calcReservationBoxPlaceY(
                apiData.Business.openDate,
                customerList.start
              )
            "
          />
        </div>
      </template>
      <template #iconbtn>
        <v-icon class="icon-btn" @click="radiosEvent()">
          mdi-chevron-right
        </v-icon>
      </template>
    </CalendarDayCommon>
  </div>
</template>

<script>
export default {
  props: {
    dateItem: {
      type: Object,
      default: null,
    },
    apiData: {
      type: Object,
      default: null,
    },
    customerLists: {
      type: Array,
      default: null,
    },
    radios: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      times: [],
    }
  },
  created() {
    if (this.apiData) this.setTimes()
  },
  methods: {
    setTimes() {
      this.times = []
      const businesstTime = this.calcBusinessTime(
        this.apiData.Business.openDate,
        this.apiData.Business.closeDate
      )
      const startHour = this.apiData.Business.openDate.getHours()
      for (let i = 0; i <= businesstTime; i++) {
        this.times.push(startHour + i + ':00')
      }
    },
    radiosEvent() {
      this.$emit('sendRaiosData', 'person')
    },
    // calc
    // calc Business Time diff
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getHours() - startDate.getHours()
      return difftime
    },
    DateComparison(dateItem, date) {
      if (!dateItem) return false
      if (dateItem.year !== date.getFullYear()) return false
      if (dateItem.month !== date.getMonth() + 1) return false
      if (dateItem.day !== date.getDate()) return false
      return true
    },

    arryFindIdIndex(Id, Lists) {
      const resault = Lists.findIndex((object) => {
        return object.id === Id
      })
      return resault
    },
    calcReservationBigBoxPlaceX(Id, Lists) {
      const result = this.arryFindIdIndex(Id, Lists)
      if (result >= 0) return result
      return null
    },
    calcReservationBoxPlaceY(openDate, reservationDate) {
      const diffHours = Math.abs(
        openDate.getHours() - reservationDate.getHours()
      )
      const diffMinitu =
        ((diffHours * 60 + reservationDate.getMinutes()) / 10) * 20
      return diffMinitu
    },
    calcReservationSmallBoxPlaceX(unitId, unitLists, personId, personLists) {
      const unitIdIndex = this.arryFindIdIndex(unitId, unitLists)
      const personIdIndex = this.arryFindIdIndex(personId, personLists)
      const unitPlaceX = unitIdIndex * this.apiData.personInChargeLists.length
      const personPlaceX = personIdIndex
      return unitPlaceX + personPlaceX
    },
  },
}
</script>

<style></style>
