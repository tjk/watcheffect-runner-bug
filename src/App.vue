<template>
  <div>hello world</div>
</template>

<script setup>
import { watchEffect } from 'vue'
watchEffect(onInvalidate => {
  onInvalidate(() => {})
})
</script>
