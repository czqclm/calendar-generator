<template>
    <div class="days">
      <template v-for="(day, index) in allDays" :key="index">
        <div class="day-box" v-if="day !== ''">
          <div 
            class="day-border-box" 
            :class="getDayClass(day, index)"
          >
            <div class="day-itme">{{ day }}</div>
            <div class="nikename">{{ getNickname(day) }}</div>
          </div>
        </div>
        <div v-else class="day-box empty"></div>
      </template>
    </div>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  
  const props = defineProps({
    totalDays: { type: Number, default: 31 },
    firstDayOfWeek: { type: Number, default: 2 },
    specialDays: { type: Array, default: () => [] },
    colorConfig: { type: Object, required: true }
  })
  
  const allDays = computed(() => {
    const emptyDays = Array(props.firstDayOfWeek).fill('')
    const days = Array.from({ length: props.totalDays }, (_, i) => i + 1)
    const allDays = [...emptyDays, ...days]
    
    const remainingDays = 7 - (allDays.length % 7)
    if (remainingDays < 7) {
      allDays.push(...Array(remainingDays).fill(''))
    }
    
    return allDays
  })
  
  const getDayClass = (day, index) => {
    const specialDay = props.specialDays.find(s => s.day === day)
    const classes = []
    
    if (index % 7 === 5 || index % 7 === 6) {
      classes.push('two-day-dayoffs-color')
    }
    
    if (specialDay) {
      classes.push(specialDay.className)
    }
    
    return classes
  }
  
  const getNickname = (day) => {
    const specialDay = props.specialDays.find(s => s.day === day)
    return specialDay ? specialDay.nickname : ''
  }
  </script>
  
  <style scoped>
  .days {
    display: flex;
    flex-wrap: wrap;
  }
  </style>
  