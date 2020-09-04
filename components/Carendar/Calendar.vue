<template>
  <v-card>
    <CalendarHeadar
      :raios="raiosData"
      @sendDate="setDate"
      @sendRaiosData="setRaiosData"
      @sendCarendarList="setCarendarActiveList"
    />
    <CalendarDayContent
      v-if="activeCarendarList === 0"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
    <CalendarWeekContent
      v-if="activeCarendarList === 1"
      :date-item="dateItem"
      :radios="raiosData === 'unit' ? true : false"
      :api-data="damyData"
      :customer-lists="damyData.customerLists"
      :width="100"
      @sendRaiosData="setRaiosData"
    />
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      date: new Date(),
      dateItem: {
        year: null,
        month: null,
        day: null,
      },
      raiosData: 'unit',
      activeCarendarList: 0,
      damyData: {
        Business: {
          openDate: new Date('2020/9/2/7:50'),
          closeDate: new Date('2020/9/2/17:0'),
        },
        times: [],
        personInChargeLists: [
          {
            id: 1,
            personInCharge: 'あ担当者1',
          },
          {
            id: 2,
            personInCharge: 'い担当者2',
          },
          {
            id: 3,
            personInCharge: 'う担当者３',
          },
          {
            id: 4,
            personInCharge: 'え担当者４',
          },
          {
            id: 5,
            personInCharge: 'お担当者5',
          },
          {
            id: 6,
            personInCharge: 'お担当者5',
          },
          {
            id: 7,
            personInCharge: 'お担当者5',
          },
        ],
        unitLists: [
          {
            id: 1,
            unitName: 'ユニット1',
          },
          {
            id: 2,
            unitName: 'ユニット2',
          },
          {
            id: 3,
            unitName: 'ユニット３',
          },
          {
            id: 4,
            unitName: 'ユニット４',
          },
          {
            id: 5,
            unitName: 'ユニット5',
          },
          {
            id: 6,
            unitName: 'ユニット４',
          },
          {
            id: 7,
            unitName: 'ユニット5',
          },
        ],
        customerLists: [
          {
            start: new Date('2020/8/31/10:10'),
            end: new Date('2020/8/31/13:0'),
            name: '野村',
            customerNumberOfPeople: 0,
            nomination: false,
            PersonInChargeId: 7,
            unitId: 1,
          },
          {
            start: new Date('2020/9/2/10:0'),
            end: new Date('2020/9/2/14:0'),
            name: '野村',
            customerNumberOfPeople: 0,
            nomination: true,
            PersonInChargeId: 1,
            unitId: 2,
          },
        ],
      },
    }
  },
  methods: {
    setDate(data) {
      this.dateItem.year = data.getFullYear()
      this.dateItem.month = data.getMonth() + 1
      this.dateItem.day = data.getDate()
    },
    setRaiosData(data) {
      console.log('sssssss', data)
      this.raiosData = data
    },
    setCarendarActiveList(data) {
      this.activeCarendarList = data
    },
  },
}
</script>

<style></style>
