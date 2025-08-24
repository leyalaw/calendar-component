<template>
  <section v-if="currentMonthDate" class="base-calendar">
    <BaseCalendarHeader
      :date="currentMonthDate"
      :locale="locale"
      @click-previous="goToMonth('previous')"
      @click-next="goToMonth('next')"
    />
    <BaseCalendarTable
      :current-month-date="currentMonthDate"
      :selected-day-date="selectedDayDate"
      :locale="locale"
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
      selectedDayDate: null,
      currentMonthDate: null,
    };
  },
  /* --------------------------------- Mounted -------------------------------- */
  mounted() {
    this.setInitialDate();
  },
  /* --------------------------------- Methods -------------------------------- */
  methods: {
    goToMonth(direction) {
      const monthChange = direction === "previous" ? -1 : 1;
      const date = new Date(this.currentMonthDate);
      date.setMonth(date.getMonth() + monthChange);
      this.currentMonthDate = date;
    },
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
