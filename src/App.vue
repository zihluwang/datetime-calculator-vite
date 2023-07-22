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
          :max="timePeriod.get('hour')"
        />
        时
        <el-input-number class="input-break-time" v-model="breakMinutes" :min="0" :max="60" />
        分
      </el-form-item>

      <el-form-item label="共娱乐">
        {{ `${timePeriod.get("hour")} 时 ${timePeriod.get("minute")} 分` }}
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue"
import moment, { type Moment } from "moment"

const initTime = ref<Moment>(moment({ hour: 0, minute: 0, second: 0 }))
const startAt = ref<Date>(initTime.value.toDate())
const endAt = ref<Date>(initTime.value.toDate())
const breakHours = ref(0)
const breakMinutes = ref(0)

const timePeriod = computed(() =>
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
