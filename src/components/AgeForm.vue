<script setup>
  import { ref } from "vue";

  const inputDate = ref({
    day: null,
    month: null,
    year: null,
    isValid: true,
    isEmpty: false,
    exist: true,
  });

  const currentYear = new Date().getFullYear();

  const validation = (day, month, year) => {
    inputDate.value.isValid = false;
    inputDate.value.isEmpty = day === null && month === null && year === null;

    if (!inputDate.value.isEmpty) {
      if (year < currentYear && month >= 1 && month <= 12) {
        const lastDayOfMonth = new Date(year, month, 0).getDate();
        inputDate.value.exist = day <= lastDayOfMonth;
        inputDate.value.isValid = inputDate.value.exist;
      } else {
        inputDate.value.isValid = false;
      }
    }
    console.log(inputDate.value.isValid);
    return inputDate.value.isValid;
  };

  const emit = defineEmits(["save-date"]);
  const send = (day_, month_, year_, valid) => {
    valid = validation(day_, month_, year_);
    emit("save-date", day_, month_, year_, valid);
    setTimeout(() => {
      inputDate.value.isEmpty = false;
      inputDate.value.exist = true;
    }, 2000);
  };

  function handleKeyDown(event) {
    if (event.key === "e" || event.keyCode === 69) {
      event.preventDefault();
    }
  }
</script>

<template>
  <form>
    <div
      class="grid-layout fs-500"
      :class="inputDate.isEmpty || !inputDate.exist ? 'err' : ''">
      <div
        class="input-box"
        :class="inputDate.day > 31 || inputDate.day < 0 ? 'err' : ''">
        <label for="day" class="fs-300 text-neutral-700 fw-extra-bold"
          >DAY</label
        >
        <input
          class="fw-extra-bold bg-neutral-100 text-neutral-900"
          @keydown="handleKeyDown"
          v-model="inputDate.day"
          placeholder="DD"
          type="number"
          name="day"
          id="day" />

        <span
          v-if="inputDate.day > 31 || inputDate.day < 0"
          class="feedback text-accent-100"
          >Must be a valid day</span
        >

        <span v-if="inputDate.isEmpty" class="feedback text-accent-100"
          >This field is required</span
        >
        <span v-if="!inputDate.exist" class="feedback text-accent-100"
          >Must be a valid date</span
        >
      </div>
      <div
        class="input-box"
        :class="inputDate.month > 12 || inputDate.month < 0 ? 'err' : ''">
        <label for="month" class="fs-300 text-neutral-700 fw-extra-bold"
          >MONTH</label
        >
        <input
          class="fw-extra-bold bg-neutral-100 text-neutral-900"
          @keydown="handleKeyDown"
          v-model="inputDate.month"
          placeholder="MM"
          type="number"
          name="month"
          id="month" />
        <span
          v-if="inputDate.month > 12 || inputDate.month < 0"
          class="feedback text-accent-100"
          >Must be a valid month</span
        >
        <span v-if="inputDate.isEmpty" class="feedback text-accent-100"
          >This field is required</span
        >
      </div>
      <div
        class="input-box"
        :class="inputDate.year >= currentYear ? 'err' : ''">
        <label for="year" class="fs-300 text-neutral-700 fw-extra-bold"
          >YEAR</label
        >
        <input
          class="fw-extra-bold bg-neutral-100 text-neutral-900"
          @keydown="handleKeyDown"
          v-model="inputDate.year"
          placeholder="YYYY"
          type="number"
          name="year"
          id="year" />

        <span
          v-if="inputDate.year >= currentYear"
          class="feedback text-accent-100"
          >Must be in the past</span
        >
        <span v-if="inputDate.isEmpty" class="feedback text-accent-100"
          >This field is required</span
        >
      </div>
    </div>
    <div class="button">
      <button
        type="submit"
        @click.prevent="
          send(
            inputDate.day,
            inputDate.month,
            inputDate.year,
            inputDate.isValid
          )
        "
        @keypress.enter.prevent="
          send(
            inputDate.day,
            inputDate.month,
            inputDate.year,
            inputDate.isValid
          )
        "
        class="bg-primary-400">
        <img src="./icons/icon-arrow.svg" alt="row" />
      </button>
    </div>
  </form>
</template>

<style lang="css" scoped>
  .grid-layout {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
  }
  form .input-box {
    display: flex;
    flex-direction: column;
    /* border: 2px solid #000; */
  }
  form .input-box > input {
    border: 1px solid var(--clr-neutral-500);
    border-radius: 0.5rem;
    padding: 0.45rem;
    width: 100%;
  }
  form .input-box > input {
    text-indent: 0.4rem;
  }
  form .input-box > input:focus-visible,
  form .input-box > input:focus,
  form .input-box > input:focus-within {
    border: 1px solid var(--clr-primary-400);
    cursor: pointer;
    outline: none;
  }

  form .input-box > input::placeholder {
    letter-spacing: 0.5px;
  }

  form .input-box > label {
    text-align: left;
    letter-spacing: 0.1rem;
    font-size: 0.55rem;
    margin-bottom: 0.5em;
  }

  .button {
    width: 100%;
    position: relative;
    z-index: 1;
  }

  .button button[type="submit"] {
    border: none;
    padding: 1rem;
    margin: 1rem 0;
    border-radius: 50%;
    z-index: 2;
    transition: all 0.5s ease;
  }

  .button button[type="submit"]::before {
    content: "";
    position: absolute;
    top: 50%;
    left: 0;
    width: 100%;
    border: 1px solid var(--clr-neutral-500);
    z-index: -1;
  }
  .button button[type="submit"]:hover {
    cursor: pointer;
    background-color: var(--clr-neutral-900);
  }
  .button button[type="submit"] img {
    width: 1.25rem;
  }

  @media (min-width: 30em) {
    .grid-layout {
      grid-template-columns: repeat(4, 1fr);
    }

    .button button[type="submit"] {
      margin: 0.55rem 0;
    }
    .button {
      text-align: right;
    }
  }

  .err .input-box > label,
  .err .input-box > .feedback {
    color: var(--clr-accent-100);
  }
  .err .input-box.input-box input {
    border: 1px solid var(--clr-accent-100);
  }

  .err > label,
  .err > .feedback {
    color: var(--clr-accent-100);
  }
  .err.input-box input {
    border: 1px solid var(--clr-accent-100);
  }
  .feedback {
    position: relative;
    left: -0.45rem;
    margin-top: 0.3rem;
    font-size: 0.5rem;
    font-style: italic;
  }

  form .err.input-box > input:focus-visible,
  form .err.input-box > input:focus,
  form .err.input-box > input:focus-within {
    border: 1px solid var(--clr-accent-100);
  }
</style>
