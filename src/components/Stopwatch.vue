<script setup lang="ts">
import { ref, getCurrentInstance } from 'vue'

const isRunning = ref<boolean>(false);

const inputHours = ref<number | string>(0);
const inputMinutes = ref<number | string>(0);
const inputSeconds = ref<number | string>(0);

const startDate = ref<Date | null>(null);
const endDate = ref<Date | null>(null);

const instance = getCurrentInstance();
setInterval(() => instance.proxy.$forceUpdate(), 1000);

function handleStart(event) {
    event.preventDefault();
    // Coerce type
    if (typeof inputHours.value != 'number') {
        inputHours.value = 0;
    }
    if (typeof inputMinutes.value != 'number') {
        inputMinutes.value = 0;
    }
    if (typeof inputSeconds.value != 'number') {
        inputSeconds.value = 0;
    }

    // Coerce size
    inputHours.value = clamp(inputHours.value, 0, 23);
    inputMinutes.value = clamp(inputMinutes.value, 0, 59);
    inputSeconds.value = clamp(inputSeconds.value, 0, 59);

    // TODO: set start date

    isRunning.value = true;
}

function handleStop() {
    isRunning.value = false;
    // TODO: set end date
}

function handleReset() {
    const confirmedReset = confirm("Are you sure you want to reset the stopwatch?");
    if (!confirmedReset) {
        return;
    }
    inputHours.value = 0;
    inputMinutes.value = 0;
    inputSeconds.value = 0;
}

function clamp(val, min, max) {
    return Math.min(Math.max(val, min), max);
}
</script>

<template>
    <div class="time-input">
        <input v-model="inputHours" type="number" name="hours">
        <span class="time-delimiter">:</span>
        <input v-model="inputMinutes" type="number" name="minutes">
        <span class="time-delimiter">:</span>
        <input v-model="inputSeconds" type="number" name="seconds">
    </div>
    
    <button @click="handleStart" form="timeInput" type="submit">Start</button>
    <button @click="handleStop">Stop</button>
    <button @click="handleReset">Reset</button>
</template>

<style scoped>
.time-input {
    margin-bottom: 8px;
}

.time-delimiter {
    margin: 0 8px;
}
</style>
