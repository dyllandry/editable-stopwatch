<script setup lang="ts">
</script>

<template>
    <form id="timeInput" class="time-input">
        <input type="number" name="hours" max="23" min="0">
        <span class="time-delimiter">:</span>
        <input type="number" name="minutes" max="59" min="0">
        <span class="time-delimiter">:</span>
        <input type="number" name="seconds" max="59" min="0">
    </form>
    <button form="timeInput" type="submit">Start</button>
    <button>Stop</button>
    <button>Reset</button>
</template>

<style scoped>
.time-input {
    margin-bottom: 8px;
}

.time-delimiter {
    margin: 0 8px;
}
</style>
