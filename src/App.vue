<script setup>
  import Welcome from './components/pages/Welcome.vue';
  import Layout from './components/layouts/Layout.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Workout from './components/pages/Workout.vue'
  
  import { computed, onMounted, ref } from 'vue';
  import { workoutProgram } from './utils';

  const defaultData = {}
  for (let workoutIdx in workoutProgram) {
    const workoutData = workoutProgram[workoutIdx]
    defaultData[workoutIdx] = {}

    for (let e of workoutData.workout) {
      defaultData[workoutIdx][e.name] = ''
    }
  }


  const selectedDisplay = ref(1)
  const data = ref(defaultData)
  const selectedWorkout = ref(-1)

  const isWorkoutComplete = computed(() => {
    const currWorkout = data.value?.[selectedWorkout.value]
    if (!currWorkout) {
      return false
    }

    const isCompleteCheck = Object.values(currWorkout).every(ex => !!ex)
    return isCompleteCheck

  })

  const firstInCompleteWorkoutIndex = computed(() => {
    const allWorkouts = data.value
    if (!allWorkouts) {
      return -1
    }

    // loop over every key value pair, and check if the workout is complete or not
    for (const [index, workout] of Object.entries(allWorkouts)) {
      const isComplete = Object.values(workout).every(ex => !!ex)
      if (!isComplete) {
        return parseInt(index)
      }
    }

    return -1 // all are complete
  })

  function handleChangeDisplay(idx) {
    selectedDisplay.value = idx
  }

  function handleSelectWorkout(idx) {
    selectedDisplay.value = 3
    selectedWorkout.value = idx
  }

  function handleSaveWorkout() {
    // save the current data snapshot to localstorage
    localStorage.setItem('workouts', JSON.stringify(data.value))
    // show the dashboard
    selectedDisplay.value = 2
    //deselect a workout
    selectedWorkout.value = -1
    
  }

  function handleResetPlan() {
    selectedDisplay.value = 2
    selectedWorkout.value = -1
    data.value = defaultData
    localStorage.removeItem('workouts')
    alert('Reset successfully')
  }

  onMounted(() => {
    if (!localStorage) {
      return
    }
    if (localStorage.getItem('workouts')) {
      const savedData = JSON.parse(localStorage.getItem('workouts'))
      data.value = savedData

      selectedDisplay.value = 2

    }

  })

</script>

<template>
  <layout>
    <welcome :handleChangeDisplay="handleChangeDisplay" v-if="selectedDisplay == 1"/>
    
    <dashboard :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex"
     :handleSelectWorkout="handleSelectWorkout" 
     :handleResetPlan="handleResetPlan"
     v-if="selectedDisplay == 2"/>  
    
     <workout :handleSaveWorkout="handleSaveWorkout" :isWorkoutComplete="isWorkoutComplete"
     :data="data" :selectedWorkout="selectedWorkout" v-if="workoutProgram?.[selectedWorkout]"/>
  </layout>
  
</template>

<style scoped></style>
