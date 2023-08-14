<script setup>
import TestOne from './components/TestOne.vue';
import TestTwo from './components/TestTwo.vue';
</script>

<template>
    <div class="row">
        <div class="center">
            <TestOne />
        </div>
        <div class="spacer"></div>
        <TestTwo />
    </div>
</template>

<style scoped>
.row {
    display: flex;
}
.center {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
}

.spacer {
    width: 100px; /* Adjust spacer width as needed */
}
</style>
