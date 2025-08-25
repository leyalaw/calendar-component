<template>
  <main class="container">
    <!-- Простой информационный блок -->
    <header class="info">
      <!-- ввод даты -->
      <form @submit.prevent="initialDateString = dateString">
        <input v-model="dateString" type="text" placeholder="YYYY-MM-DD" />
        <button type="submit">></button>
      </form>
      <!-- выбор локали -->
      <select v-model="currentLocaleCode">
        <option
          v-for="locale in LOCALES"
          :key="locale.code"
          :value="locale.code"
        >
          {{ locale.name }}
        </option>
      </select>
      <!-- отображение выбранной даты -->
      <p tabindex="0">{{ formattedSelectedDate }}</p>
    </header>
    <!-- Календарь -->
    <BaseCalendarComponent
      :locale="currentLocaleCode"
      :initial-date-string="initialDateString"
      @select="selectedDate = $event"
    />
  </main>
</template>

<script>
import BaseCalendarComponent from "./components/BaseCalendar/BaseCalendar.vue";

/** Локали */
const LOCALE = Object.freeze({
  US: { code: "en-US", name: "English" },
  RU: { code: "ru-RU", name: "Русский" },
});

export default {
  name: "App",
  components: {
    BaseCalendarComponent,
  },
  /* ---------------------------------- Data ---------------------------------- */
  data() {
    return {
      /** Локали */
      LOCALES: Object.freeze(Object.values(LOCALE)),
      /** Выбранная локаль */
      currentLocaleCode: LOCALE.US.code,
      /** Выбранная дата */
      selectedDate: null,
      /** Строка даты */
      dateString: "",
      /** Исходная строка даты */
      initialDateString: "",
    };
  },
  /* -------------------------------- Computed -------------------------------- */
  computed: {
    /** Дата для отображения */
    formattedSelectedDate() {
      return this.selectedDate
        ? this.selectedDate.toLocaleDateString(this.currentLocaleCode, {
            year: "numeric",
            month: "long",
            day: "numeric",
          })
        : "";
    },
  },
};
</script>

<style scoped>
/* ---------------------------------- Style --------------------------------- */
.container {
  margin: 2rem;
  font-size: 1.6rem;
}

.info {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2rem;
}

.info form {
  width: 100%;
}

.info form input {
  border-bottom: 1px solid black;
}

.info form input::placeholder {
  color: #ccc;
  font-size: 1.2rem;
}

.info form button {
  font-weight: bold;
  padding: 0.8rem;
}
</style>
