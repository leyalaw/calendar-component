<template>
  <main class="container">
    <!-- Простой информационный блок -->
    <header class="header">
      <select v-model="currentLocaleCode">
        <option
          v-for="locale in LOCALES"
          :key="locale.code"
          :value="locale.code"
        >
          {{ locale.name }}
        </option>
      </select>
      <p tabindex="0">{{ formattedSelectedDate }}</p>
    </header>
    <!-- Календарь -->
    <BaseCalendarComponent
      @select="selectedDate = $event"
      :locale="currentLocaleCode"
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

.header {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}
</style>
