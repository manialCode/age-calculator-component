<script setup>
  const props = defineProps({
    userDate: {
      type: Object,
      required: true,
    },
  });
</script>

<template>
  <div class="result-container">
    <p class="text-neutral-900 fs-input fw-extra-bold">
      <span class="text-primary-400">{{
        props.userDate.state ? props.userDate.year : "--"
      }}</span>
      years
    </p>
    <p class="text-neutral-900 fs-input fw-extra-bold">
      <span class="text-primary-400">{{
        props.userDate.state ? props.userDate.month : "--"
      }}</span>
      months
    </p>
    <p class="text-neutral-900 fs-input fw-extra-bold">
      <span class="text-primary-400">{{
        props.userDate.state ? props.userDate.day : "--"
      }}</span>
      days
    </p>
  </div>
</template>

<style lang="css" scoped>
  .result-container {
    font-style: italic;
    text-align: left;
    line-height: 1.2;
  }
  .fs-input {
    font-size: 2.3rem;
  }

  @media (min-width: 28.125em) {
    .fs-input {
      font-size: 4rem;
    }
  }
</style>
