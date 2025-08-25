<template>
  <section v-if="currentMonthDate" class="base-calendar">
    <!-- Заголовок календаря -->
    <BaseCalendarHeader
      :current-month-date="currentMonthDate"
      :locale="locale"
      @click-previous="currentMonthDate = $event"
      @click-next="currentMonthDate = $event"
    />
    <!-- Таблица дней месяца -->
    <BaseCalendarTable
      :current-month-date="currentMonthDate"
      :selected-day-date="selectedDayDate"
      :locale="locale"
      @click-day="selectDay"
    />
  </section>
</template>

<script>
/* -------------------------------------------------------------------------- */
/*                                  Календарь                                 */
/* -------------------------------------------------------------------------- */

import BaseCalendarHeader from "./BaseCalendarHeader.vue";
import BaseCalendarTable from "./BaseCalendarTable.vue";

export default {
  name: "BaseCalendar",
  components: {
    BaseCalendarHeader,
    BaseCalendarTable,
  },
  /* ---------------------------------- Emits --------------------------------- */
  emits: ["select"],
  /* ---------------------------------- Props --------------------------------- */
  props: {
    /**
     * Строка с датой формата YYYY-MM-DD,
     * которая будет использоваться в качестве начальной
     * (по умолчанию текущая дата)
     */
    initialDateString: {
      type: String,
      validator(dateString) {
        return /(\d{4})-(\d{2})-(\d{2})/.test(dateString);
      },
    },
    /** Код локали (по умолчанию en-US) */
    locale: {
      type: String,
      default: "en-US",
      validator(locale) {
        try {
          new Intl.DateTimeFormat(locale);
          return true;
        } catch (error) {
          return false;
        }
      },
    },
  },
  /* ---------------------------------- Data ---------------------------------- */
  data() {
    return {
      /** Дата выбранного дня */
      selectedDayDate: null,
      /** Дата отображаемого месяца */
      currentMonthDate: null,
    };
  },
  /* --------------------------------- Mounted -------------------------------- */
  mounted() {
    this.setInitialDate();
  },
  /* ---------------------------------- Watch --------------------------------- */
  watch: {
    initialDateString() {
      this.setInitialDate();
    },
    selectedDayDate() {
      this.$emit("select", this.selectedDayDate);
    },
  },
  /* --------------------------------- Methods -------------------------------- */
  methods: {
    /** Выбор дня */
    selectDay(date) {
      this.selectedDayDate = date;
    },
    /**
     * Установка начальных значений дат выбранного дня
     * и отображаемого месяца
     */
    setInitialDate() {
      const initialDate = this.initialDateString
        ? new Date(this.initialDateString)
        : new Date();

      this.selectedDayDate = initialDate;
      this.currentMonthDate = initialDate;
    },
  },
};
</script>

<style scoped>
/* ---------------------------------- Style --------------------------------- */
.base-calendar {
  --background-color: #fff;
  --active-color: #44bbdd;

  width: 24rem;
  border: 1px solid #ccc;
  background-color: var(--background-color);
  text-align: center;
  color: #333;
}
</style>
