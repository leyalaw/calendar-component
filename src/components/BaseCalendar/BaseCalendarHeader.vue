<template>
  <header class="base-calendar-header">
    <!-- Предыдущий месяц -->
    <button
      type="button"
      @click="$emit('click-previous')"
      class="base-calendar-header__month-button base-calendar-header__month-button--previous"
    >
      <!-- стрелка влево -->
      <span class="base-calendar-header__month-arrow"></span>
    </button>

    <!-- Месяц и год -->
    <div class="base-calendar-header__title">
      <span class="base-calendar-header__month">
        {{ month }}
      </span>
      <span class="base-calendar-header__year">
        {{ year }}
      </span>
    </div>

    <!-- Следующий месяц -->
    <button
      type="button"
      @click="$emit('click-next')"
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
    date: {
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
    /** Сокращённое название месяца */
    month() {
      return this.date.toLocaleString(this.locale, { month: "short" });
    },
    /** Год в формате YYYY */
    year() {
      return this.date.getFullYear();
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
  padding: 1rem;
  font-size: 1.2rem;
}

.base-calendar-header__month-arrow {
  display: inline-block;
  border-top: 0.5rem solid transparent;
  border-bottom: 0.5rem solid transparent;
  border-right: 0.8rem solid currentColor;
}

.base-calendar-header__month-button--next > * {
  transform: rotate(180deg);
}

.base-calendar-header__title {
  display: flex;
  gap: 0.8rem;
}
</style>
