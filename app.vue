<template>
  <div>
    <NuxtRouteAnnouncer />
    <Quiz />
  </div>
</template>

<script setup>
import Quiz from './components/Quiz.vue';
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f0f0;
}
</style>