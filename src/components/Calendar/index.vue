<template>
  <div class="calendar">
    <el-row type="flex" justify="end">
      <el-col :span="5">
        <el-select @change="updateCalendar" v-model="currentYear">
          <el-option
            v-for="item in years"
            :key="item"
            :label="item"
            :value="item"
          >
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="3">
        <el-select @change="updateCalendar" v-model="currentMonth">
          <el-option v-for="item in 12" :key="item" :label="item" :value="item">
          </el-option>
        </el-select>
      </el-col>
    </el-row>
    <el-calendar v-model="currentDate">
      <template v-slot:dateCell="{ date }">
        <div class="cell-box">
          {{ date.getDate() }}
          <span v-if="isWeek(date)">休</span>
        </div>
      </template>
    </el-calendar>
  </div>
</template>

<script>
export default {
  name: 'Calendar',
  data() {
    return {
      currentYear: '',
      currentMonth: '',
      currentDate: '',
      years: [], // 当前年份的 + 前5年 + 后5年 11
    }
  },

  created() {
    this.initCaldndar()

    // const arr = [1, 2, 3, 4, 5, 5, 6]
    // const newSetArr = new Set(arr)
    // const newArr = Array.from(newSetArr, (item) => {
    //   // mdn mapFn
    //   return item + 1
    // })
    // newArr.push(8)
    // console.log(newArr)
  },

  methods: {
    initCaldndar() {
      const date = new Date()
      this.currentYear = date.getFullYear()
      this.currentMonth = date.getMonth() + 1
      this.years = Array.from(
        Array(11).fill(this.currentYear - 5),
        (item, i) => item + i
      )

      this.currentDate = date
    },
    isWeek(date) {
      // console.log(date.getDay())
      return date.getDay() === 6 || date.getDay() === 0
    },
    // 用于更新日历
    updateCalendar() {
      this.currentDate = this.currentYear + '-' + this.currentMonth
      // console.log(this.currentDate)
    },
  },
}
</script>

<style scoped lang="scss">
::v-deep .el-calendar {
  .el-calendar__header {
    display: none;
  }
  .el-calendar-table td {
    border: none !important;
  }
  .cell-box {
    color: green;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }
}
</style>
