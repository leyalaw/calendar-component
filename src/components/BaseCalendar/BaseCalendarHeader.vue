<template>
  <header :aria-label="currentFullName" class="base-calendar-header">
    <!-- Предыдущий месяц -->
    <button
      type="button"
      :aria-label="previousLongMonth"
      @click="$emit('click-previous', previousMonthDate)"
      class="base-calendar-header__month-button base-calendar-header__month-button--previous"
    >
      <!-- стрелка влево -->
      <span class="base-calendar-header__month-arrow"></span>
    </button>

    <!-- Месяц и год -->
    <div
      tabindex="0"
      :aria-label="currentFullName"
      aria-live="assertive"
      class="base-calendar-header__title"
    >
      <span aria-hidden="true" class="base-calendar-header__month">
        {{ currentMonth.short }}
      </span>
      <span aria-hidden="true" class="base-calendar-header__year">
        {{ currentYear }}
      </span>
    </div>

    <!-- Следующий месяц -->
    <button
      type="button"
      :aria-label="nextLongMonth"
      @click="$emit('click-next', nextMonthDate)"
      class="base-calendar-header__month-button base-calendar-header__month-button--next"
    >
      <!-- стрелка вправо -->
      <span class="base-calendar-header__month-arrow"></span>
    </button>
  </header>
</template>

<script>
/* -------------------------------------------------------------------------- */
/*                               Хедер календаря                              */
/* -------------------------------------------------------------------------- */

export default {
  name: "BaseCalendarHeader",
  /* ---------------------------------- Emits --------------------------------- */
  emits: ["click-previous", "click-next"],
  /* ---------------------------------- Props --------------------------------- */
  props: {
    /** Дата отображаемого месяца */
    currentMonthDate: {
      type: Date,
      required: true,
    },
    /** Код локали (по умолчанию en-US) */
    locale: {
      type: String,
      default: "en-US",
    },
  },
  /* -------------------------------- Computed -------------------------------- */
  computed: {
    /** Форматтер сокращённых названий месяцев */
    monthShortFormatter() {
      return new Intl.DateTimeFormat(this.locale, { month: "short" });
    },
    /** Форматтер полных названий месяцев */
    monthLongFormatter() {
      return new Intl.DateTimeFormat(this.locale, { month: "long" });
    },
    /** Дата предыдущего месяца */
    previousMonthDate() {
      return this.getNearMonth("previous");
    },
    /** Дата следующего месяца */
    nextMonthDate() {
      return this.getNearMonth("next");
    },
    /** Названия отображаемого месяца */
    currentMonth() {
      return {
        short: this.monthShortFormatter.format(this.currentMonthDate),
        long: this.monthLongFormatter.format(this.currentMonthDate),
      };
    },
    /** Полное название предыдущего месяцев */
    previousLongMonth() {
      return this.monthLongFormatter.format(this.previousMonthDate);
    },
    /** Полное название следующего месяцев */
    nextLongMonth() {
      return this.monthLongFormatter.format(this.nextMonthDate);
    },
    /** Год в формате YYYY */
    currentYear() {
      return this.currentMonthDate.getFullYear();
    },
    /** Полное название отображаемых месяца и года */
    currentFullName() {
      return `${this.currentMonth.long} ${this.currentYear}`;
    },
  },
  methods: {
    getNearMonth(direction) {
      const monthChange = direction === "previous" ? -1 : 1;
      const date = new Date(this.currentMonthDate);
      date.setMonth(date.getMonth() + monthChange);
      return date;
    },
  },
};
</script>

<style scoped>
/* ---------------------------------- Style --------------------------------- */
.base-calendar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.8rem;
  font-size: 1.2rem;
}

.base-calendar-header__month-arrow {
  display: inline-block;
  border-top: 0.5rem solid transparent;
  border-bottom: 0.5rem solid transparent;
  border-right: 0.8rem solid currentColor;
}

.base-calendar-header__month-button {
  padding: 0.2rem;
}

.base-calendar-header__month-button:hover {
  color: var(--active-color);
}

.base-calendar-header__month-button--next > * {
  transform: rotate(180deg);
}

.base-calendar-header__title {
  display: flex;
  gap: 0.8rem;
}
</style>
