<template>
  <div class="reservation-box" :style="boxWidth">
    <div
      :style="boxStyles"
      class="reservation-content"
      :class="customerInfo.nomination ? 'nomination-box' : 'no-nomination-box'"
    >
      <div v-if="boxBig">
        <p>{{ customerInfo.name }}</p>
        <p v-if="customerInfo.customerNumberOfPeople != 0">
          他{{ customerInfo.customerNumberOfPeople }}名
        </p>
        <p>{{ reseversionTimeText }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    rowWidth: {
      type: Number,
      default: 100,
    },
    boxPlaceX: {
      type: Number,
      default: 0,
    },
    boxPlaceY: {
      type: Number,
      default: 0,
    },
    customerInfo: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {}
  },
  computed: {
    boxStyles() {
      return {
        '---height': this.calcBoxheight(this.intDiffMinute) + 'px',
        '---boxPlaceX': this.calcBoxPlaceX() + 'px',
        '---boxPlaceY': this.boxPlaceY + 'px',
      }
    },
    boxWidth() {
      return {
        '---width': this.rowWidth + 'px',
      }
    },
    boxPlace() {
      return {
        '---boxPlaceHeight': this.calcBoxPlace() + 'px',
      }
    },
    reseversionTimeText() {
      const startMinutes = this.customerInfo.start.getMinutes()
      const endMinutes = this.customerInfo.end.getMinutes()
      const startHours = this.customerInfo.start.getHours()
      const endHours = this.customerInfo.end.getHours()
      if (startMinutes === 0 || endMinutes === 0)
        return (
          startHours +
          ':' +
          startMinutes +
          '0' +
          '~' +
          endHours +
          ':' +
          endMinutes +
          '0'
        )
      return startHours + ':' + startMinutes + '~' + endHours + ':' + endMinutes
    },
    boxBig() {
      if (this.rowWidth >= 100) return true
      return false
    },
  },
  created() {
    this.setIntDiffMinute()
  },
  methods: {
    setIntDiffMinute() {
      this.intDiffMinute = this.calcDiffTime(
        this.customerInfo.start,
        this.customerInfo.end
      )
    },
    // 時間の差分の計算(分)
    calcDiffTime(startTime, endTime) {
      const diff = startTime.getTime() - endTime.getTime() + 30
      const diffMinute = Math.abs(diff) / (60 * 1000) + startTime.getMinutes()
      return Math.round(diffMinute)
    },
    // 差分の時間いよるboxの高さの計算
    calcBoxheight(diiftime) {
      const Boxheight = Math.round(diiftime / 10) * 20
      return Boxheight
    },
    calcBoxPlaceX() {
      return this.boxPlaceX * this.rowWidth
    },
  },
}
</script>
<style lang="scss" scoped>
.reservation-box {
  position: absolute;
  width: var(---width);
  padding: 2px;
}
.reservation-content {
  position: relative;
  height: var(---height);
  left: var(---boxPlaceX);
  top: var(---boxPlaceY);
}
.nomination-box {
  background: #6e9eff;
  border: 1px solid #6e9eff;
  border-radius: 3px;
}
.no-nomination-box {
  background: #e5eeff;
  border: 2px solid #6e9eff;
  border-radius: 3px;
}
</style>
