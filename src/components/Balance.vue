<script setup>
const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <h4>Your Balance</h4>
  <h1>${{ total }}</h1>
</template>

<style>
h4 {
  margin: 0;
  text-transform: uppercase;
}
</style>
