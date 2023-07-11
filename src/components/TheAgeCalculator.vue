<script setup>
  import { ref } from "vue";
  import AgeForm from "./AgeForm.vue";
  import CalculatorResult from "./CalculatorResult.vue";

  const userDate = ref({
    day: null,
    month: null,
    year: null,
    state: false,
  });

  const saveDate = (day_, month_, year_, isValid) => {
    const currentDate = new Date();
    const birthDate = new Date(year_, month_ - 1, day_);

    let ageYears = currentDate.getUTCFullYear() - birthDate.getUTCFullYear();
    let ageMonths = currentDate.getUTCMonth() - birthDate.getUTCMonth();
    let ageDays = currentDate.getUTCDate() - birthDate.getUTCDate();

    if (ageDays <= 0) {
      ageMonths--;
      const daysInLastMonth = new Date(
        currentDate.getUTCFullYear(),
        currentDate.getUTCMonth(),
        0
      ).getUTCDate();
      ageDays += daysInLastMonth;
    }

    if (ageMonths <= 0) {
      ageYears--;
      ageMonths += 12;
    }

    userDate.value.day = ageDays;
    userDate.value.month = ageMonths;
    userDate.value.year = ageYears;
    userDate.value.state = isValid;
  };
</script>

<template>
  <div class="age-container bg-neutral-100">
    <AgeForm :userDate="userDate" @save-date="saveDate" />
    <CalculatorResult :userDate="userDate" />
  </div>
</template>

<style lang="css" scoped>
  .age-container {
    padding: 2rem 1rem;
    width: 100%;
    margin: auto;
    border-radius: 5%;
    border-bottom-right-radius: 30%;
  }

  @media (min-width: 28.125em) {
    .age-container {
      width: 490px;
      padding: 2.1rem 2rem;
    }
  }
</style>
