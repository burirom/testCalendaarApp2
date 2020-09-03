<template>
  <div class="box" style="width: 100%">
    <!-- header -->
    <div class="d-flex header">
      <div class="first-content" style="min-width: 200px">
        <div class="second-item first-row"><slot name="first" /></div>
        <div class="second-item second-row"></div>
      </div>
      <div
        v-for="(firstList, firstIndex) in firstLists"
        :key="firstIndex"
        class="second-content col-Width"
        :style="colWidth"
      >
        <div v-if="firstList.unitName" class="second-item first-row">
          {{ firstList.unitName }}
        </div>
        <div v-else-if="firstList.personInCharge" class="second-item first-row">
          {{ firstList.personInCharge }}
        </div>
        <div class="d-flex icon-box second-item second-row">
          <p>ユニット表示</p>
          <slot name="iconbtn" />
        </div>
      </div>
    </div>
    <!-- body -->
    <div class="d-flex body">
      <!-- time col -->
      <div class="time-content" style="min-width: 200px">
        <div
          v-for="(time, timeIndex) in times"
          :key="timeIndex"
          class="time-row"
        >
          <slot name="summaryItem" />
          <p class="calender-time-table ma-0 pl-2 pr-2">{{ time }}</p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        </div>
      </div>
      <!-- table -->
      <div class="d-flex posion-rl">
        <slot name="reservationbox" />
        <!-- table row -->
        <div
          v-for="(firstList, firstIndex) in firstLists"
          :key="firstIndex"
          class="table-col col-Width"
          :style="colWidth"
        >
          <!-- table col -->
          <div
            v-for="(time, timeIndex) in times"
            :key="timeIndex"
            class="table-row"
          >
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
            <p class="calender-time-table ma-0 pl-2 pr-2"></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    firstLists: {
      type: Array,
      default: null,
    },
    secondLists: {
      type: Array,
      default: null,
    },
    times: {
      type: Array,
      default: null,
    },
    width: {
      type: Number,
      default: 100,
    },
  },
  data() {
    return {
      activeNumber: null,
    }
  },
  computed: {
    colWidth() {
      return { '---width': this.width + 'px' }
    },
    secondColWidth() {
      return {
        '---width': this.getColWidth() / this.secondActiveList().length + 'px',
      }
    },
    secondActiveList() {
      return function (index) {
        if (index === this.activeNumber) return this.secondLists
        return this.secondLists.slice(0, 5)
      }
    },
  },
  methods: {
    initActiveNumber() {
      this.activeNumber = null
    },
    getColWidth() {
      return this.secondActiveList().length * 40
    },
  },
}
</script>

<style lang="scss" scoped>
.box {
  overflow: auto;
  border: 1px solid #c3c3c3;
}
.header {
  .first-content {
    border-right: 1px solid #c3c3c3;
  }
  .second-content {
    border-right: 1px solid #c3c3c3;
    &:last-child {
      border: none;
    }
  }
  .second-item {
    border-bottom: 1px solid #c3c3c3;
  }
  .first-row {
    height: 60px;
    text-align: center;
  }
  .second-row {
    height: 40px;
  }
}

.body {
  .time-content {
    border-right: 1px solid #c3c3c3;
  }
  .time-row {
    border-bottom: 1px solid #c3c3c3;
    &:last-child {
      border: none;
    }
  }
  .table-col {
    border-right: 1px solid #c3c3c3;
    &:last-child {
      border: none;
    }
  }
  .table-row {
    border-bottom: 1px solid #c3c3c3;
    &:last-child {
      border: none;
    }
  }
  .calender-time-table {
    height: 20px;
    border-bottom: 1px dotted #e2e2e2;
    &:last-child {
      border-bottom: none;
    }
  }
}

.icon-box {
  position: relative;
  .icon-btn {
    position: absolute;
    background: #f5f6f8;
    width: 22px;
    height: 22px;
    bottom: 0;
    right: 0;
    transform: translateY(-50%) translateX(50%);
    z-index: 0;
    border-radius: 50%;
    border: 1px solid #c3c3c3;
  }
}
.posion-rl {
  position: relative;
}
.border-right {
  border-right: 1px solid #0000001a;
  text-align: center;
  &:last-child {
    border: none;
  }
}
.col-Width {
  width: var(---width);
  min-width: var(---width);
}
.second-col-Width {
  width: var(---width);
  min-width: var(---width);
}
</style>
