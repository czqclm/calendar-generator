<template>
  <div class="calendar-app" :style="{
    '--weekend-color': colorConfig.weekendColor,
    '--day-off-color': colorConfig.dayOffColor,
    '--day-off-bg-color': colorConfig.dayOffBgColor,
    '--cover-shift-color': colorConfig.coverShiftColor,
    '--cover-shift-bg-color': colorConfig.coverShiftBgColor
  }">
    <div class="calendar-container">
      <div class="calendar-left">
        <div id="print-box" :style="{ fontFamily: calendarConfig.selectedFont }">
          <div class="header">
            <div class="day">一</div>
            <div class="day">二</div>
            <div class="day">三</div>
            <div class="day">四</div>
            <div class="day">五</div>
            <div class="day two-day-dayoffs-color">六</div>
            <div class="day two-day-dayoffs-color">日</div>
          </div>
          <div class="days" id="days">
            <CalendarDays :total-days="calendarConfig.totalDays" :first-day-of-week="calendarConfig.firstDayOfWeek"
              :special-days="calendarConfig.specialDays" :color-config="colorConfig" />
          </div>
        </div>
      </div>
      <div class="calendar-right">
        <CalendarConfig v-model:calendar-config="calendarConfig" v-model:color-config="colorConfig" />
      </div>
    </div>
  </div>
  <div class="support-info">powered by: <el-link type="primary" href="https://blog.aicq.icu/"
      target="_blank">@无笙</el-link></div>
</template>

<script setup>
import { ref } from 'vue'
import CalendarDays from './components/CalendarDays.vue'
import CalendarConfig from './components/CalendarConfig.vue'

const calendarConfig = ref({
  totalDays: 31,
  firstDayOfWeek: 2,
  selectedFont: '寒蝉全圆体',
  specialDays: [
    { day: 1, nickname: "元旦", className: "holiday-color" },
    { day: 10, nickname: "萤火虫D1", className: "cover-shift-color" },
    { day: 28, nickname: "除夕", className: "holiday-color" },
  ]
})

const colorConfig = ref({
  weekendColor: '#9b97c0',
  dayOffColor: '#f6ebed',
  dayOffBgColor: '#9b97c0',
  coverShiftColor: '#ffffff',
  coverShiftBgColor: '#9a9a9a'
})
</script>

<style>
body {
  font-family: "寒蝉全圆体";
  font-weight: '400';
}

.calendar-app {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.calendar-container {
  display: flex;
  width: 100%;
  max-width: 1200px;
  justify-content: space-evenly;
}

.calendar-left {
  margin-right: 20px;
  width: 600px;
  min-width: 600px;
}

.calendar-right {
  width: 500px;
}

.header {
  display: flex;
  justify-content: space-between;
  padding: 20px 0;
}

.day {
  flex: 1;
  text-align: center;
}

.day-itme {
  font-size: 1em;
  line-height: 1.5em;
  font-weight: bold;
}

.day-box {
  width: calc(100% / 7);
  height: 70px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.nikename {
  height: 17px;
  font-size: 0.5em;
  line-height: 1.5em;
  width: 80%;
  text-align: center;
}

.two-day-dayoffs-color {
  color: var(--weekend-color);
}

.holiday-color {
  color: var(--day-off-color);
  background-color: var(--day-off-bg-color);
  border-radius: 18px;
}

.cover-shift-color {
  color: var(--cover-shift-color);
  background-color: var(--cover-shift-bg-color);
  border-radius: 18px;
}

.day-border-box {
  width: 60px;
  height: 60px;
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  align-items: center;
}

.support-info {
  width: 100%;
  text-align: center;
  padding: 10px 0;
  color: #666;
}

@media (max-width: 768px) {
  .calendar-container {
    flex-direction: column;
  }

  .calendar-right {
    width: 100%;
    margin-top: 20px;
  }
}
</style>
