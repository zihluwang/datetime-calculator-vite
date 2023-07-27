<template>
  <div class="wrapper">
    <el-form label-width="80">
      <el-form-item label="开始时间">
        <el-date-picker v-model="startAt" type="datetime" placeholder="开始时间" />
      </el-form-item>

      <el-form-item label="结束时间">
        <el-date-picker v-model="endAt" type="datetime" placeholder="结束时间" />
      </el-form-item>

      <el-form-item label="休息时间">
        <el-input-number
          class="input-break-time"
          v-model="breakHours"
          :min="0"
          :max="maxBreakHour"
        />
        时
        <el-input-number
          class="input-break-time"
          v-model="breakMinutes"
          :min="0"
          :max="maxBreakMinute"
        />
        分
      </el-form-item>

      <el-form-item label="共娱乐">
        {{ `${timePlayed.asHours().toFixed(2)} 时` }}
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue"
import moment from "moment"

const initTime = moment({ hour: 0, minute: 0, second: 0 })
const startAt = ref<Date>(initTime.toDate())
const endAt = ref<Date>(initTime.toDate())
const breakHours = ref(0)
const breakMinutes = ref(0)

const timeDiff = computed(() => moment.duration(moment(endAt.value).diff(moment(startAt.value))))

const maxBreakHour = computed(() => Math.floor(timeDiff.value.asHours()))
const maxBreakMinute = computed(() => {
  if (breakHours.value == maxBreakHour.value) {
    return timeDiff.value.asMinutes() - breakHours.value * 60
  } else {
    return 59
  }
})

const timePlayed = computed(() =>
  moment
    .duration(moment(endAt.value).diff(moment(startAt.value)))
    .subtract(breakHours.value, "hour")
    .subtract(breakMinutes.value, "minute")
)
</script>

<style scoped>
.wrapper {
  width: 440px;
  height: 240px;
  /* border: 1px solid #000000; */
  padding: 20px;
}

.input-break-time {
  width: 100px;
}
</style>
