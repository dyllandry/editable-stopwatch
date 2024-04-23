<script setup lang="ts">
import { ref, computed } from 'vue'

const isRunning = ref<boolean>(false);

const inputHours = ref<number | string>(0);
const inputMinutes = ref<number | string>(0);
const inputSeconds = ref<number | string>(0);

const startDate = ref<Date>(new Date());
const endDate = ref<Date>(new Date());

let now = ref<number>(Date.now());
function updateNow() {
    now.value = Date.now();
}
setInterval(updateNow, 1000);

function onStart(event: MouseEvent) {
    event.preventDefault();
    updateNow();

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

    const inputMs = (inputHours.value * 1000 * 60 * 60) + (inputMinutes.value * 1000 * 60) + (inputSeconds.value * 1000);

    startDate.value = new Date(Date.now() - inputMs);

    isRunning.value = true;
}

function onStop() {
    updateNow();
    isRunning.value = false;
    endDate.value = new Date();
    inputHours.value = durationHours.value;
    inputMinutes.value = durationMinutes.value;
    inputSeconds.value = durationSeconds.value;
}

function onReset() {
    const confirmedReset = confirm("Are you sure you want to reset the stopwatch?");
    if (!confirmedReset) {
        return;
    }
    inputHours.value = 0;
    inputMinutes.value = 0;
    inputSeconds.value = 0;
    isRunning.value = false;
}

const duration = computed(() => {
    if (isRunning) {
        return now.value - startDate.value.getTime();
    }
    return endDate.value.getTime() - startDate.value.getTime();
});

const durationHours = computed(() => {
    return clamp(Math.floor(duration.value / (1000 * 60 * 60)) % 24, 0, 23);
});

const durationMinutes = computed(() => {
    return clamp(Math.floor(duration.value / (1000 * 60)) % 60, 0, 59);
});

const durationSeconds = computed(() => {
    return clamp(Math.floor(duration.value / 1000) % 60, 0, 59);
});

function clamp(val: number, min: number, max: number) {
    return Math.min(Math.max(val, min), max);
}
</script>

<template>
    <div v-if="isRunning" :style="{ marginBottom: '8px' }">
        <span>{{ durationHours }}</span>
        <span :style="{ margin: '0px 8px' }">:</span>
        <span>{{ durationMinutes }}</span>
        <span :style="{ margin: '0px 8px' }">:</span>
        <span>{{ durationSeconds }}</span>
    </div>

    <div v-if="!isRunning" :style="{ marginBottom: '8px' }">
        <input v-model="inputHours" type="number" name="hours">
        <span :style="{ margin: '0px 8px' }">:</span>
        <input v-model="inputMinutes" type="number" name="minutes">
        <span :style="{ margin: '0px 8px' }">:</span>
        <input v-model="inputSeconds" type="number" name="seconds">
    </div>
    
    <button @click="onStart" form="timeInput" type="submit">Start</button>
    <button @click="onStop">Stop</button>
    <button @click="onReset">Reset</button>
</template>

<style scoped>

</style>
