<template>
  <table class="base-calendar-table">
    <!-- Наименования дней недели -->
    <thead>
      <tr>
        <th
          v-for="weekday in weekdays"
          :key="weekday.short"
          :aria-label="weekday.long"
          class="base-calendar-table__weekday"
        >
          <span aria-hidden="true">{{ weekday.short }}</span>
        </th>
      </tr>
    </thead>

    <!-- Таблица дней -->
    <tbody>
      <tr v-for="week in weeks" :key="week">
        <td v-for="dayDate in week" :key="dayDate">
          <button
            type="button"
            :disabled="isDisabledDay(dayDate)"
            :tabindex="getTabIndex(dayDate)"
            :aria-pressed="isSelectedDay(dayDate)"
            class="base-calendar-table__day"
            :class="{
              'base-calendar-table__day--selected': isSelectedDay(dayDate),
            }"
            @click="$emit('click-day', dayDate)"
          >
            {{ dayDate.getDate() }}
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
/* -------------------------------------------------------------------------- */
/*                           Таблица дней календаря                           */
/* -------------------------------------------------------------------------- */

/** Дата понедельника, взятая за основу */
const MONDAY_DATE = Object.freeze(new Date(2025, 7, 4));
/** Количество дней в неделе */
const WEEKDAYS_AMOUNT = 7;
/** Максимальный и минимальный ваозможный индекс последней недели */
const LAST_WEEK_INDEX = Object.freeze({
  MAX: 5,
  MIN: 3,
});
/** Коды локалей, в которых неделя начинается с воскресенья */
const SUNDAY_WEEK_LOCALES = ["en-US", "en-CA", "en-GB"]; // TODO: список неполный

export default {
  name: "BaseCalendarTable",
  /* ---------------------------------- Props --------------------------------- */
  props: {
    /** Дата отображаемого месяца */
    currentMonthDate: {
      type: Date,
      required: true,
    },
    /** Дата выбранного дня */
    selectedDayDate: {
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
    /** флаг календаря, в котором неделя начинается с воскресенья */
    isSundayWeek() {
      return SUNDAY_WEEK_LOCALES.includes(this.locale);
    },
    /** Индекс отображаемого месяца */
    currentMonthIndex() {
      return this.currentMonthDate.getMonth();
    },
    selectedMonthIndex() {
      return this.selectedDayDate.getMonth();
    },
    /** Форматтер сокращённых названий дней недели */
    weekdayShortFormatter() {
      return new Intl.DateTimeFormat(this.locale, { weekday: "short" });
    },
    /** Форматтер полных названий дней недели */
    weekdayLongFormatter() {
      return new Intl.DateTimeFormat(this.locale, { weekday: "long" });
    },
    /** Сокращённые и полные наименования дней недели */
    weekdays() {
      const weekdays = [];

      const sundayWeekCorrection = +this.isSundayWeek;

      for (
        let daysFromMonday = 0;
        daysFromMonday < WEEKDAYS_AMOUNT;
        daysFromMonday++
      ) {
        const date = new Date(MONDAY_DATE);
        date.setDate(date.getDate() + daysFromMonday - sundayWeekCorrection);
        weekdays.push({
          short: this.weekdayShortFormatter.format(date),
          long: this.weekdayLongFormatter.format(date),
        });
      }

      return weekdays;
    },
    /** Дата первого дня месяца */
    firstMonthDayDate() {
      const date = new Date(this.currentMonthDate);
      date.setDate(1);
      return date;
    },
    /** Индекс первого дня месяца */
    firstMonthWeekdayIndex() {
      const mondayWeekCorrection = +!this.isSundayWeek;
      return this.firstMonthDayDate.getDay() - mondayWeekCorrection;
    },
    /** Номер последнего дня месяца */
    lastMonthDayNumber() {
      const date = new Date(this.currentMonthDate);
      date.setMonth(date.getMonth() + 1);
      date.setDate(0);
      return date.getDate();
    },
    /** Даты дней, сгруппированные по неделям */
    weeks() {
      const weeks = [];

      let daysFromFirst = 0;

      for (let weekIndex = 0; weekIndex <= LAST_WEEK_INDEX.MAX; weekIndex++) {
        const week = [];

        const isFirstWeek = weekIndex === 0;
        const isPossiblyLastWeek = weekIndex >= LAST_WEEK_INDEX.MIN;

        for (
          let weekdayIndex = 0;
          weekdayIndex < WEEKDAYS_AMOUNT;
          weekdayIndex++
        ) {
          const date = new Date(this.firstMonthDayDate);

          const day =
            isFirstWeek && weekdayIndex < this.firstMonthWeekdayIndex
              ? date.getDate() - this.firstMonthWeekdayIndex + weekdayIndex
              : date.getDate() + daysFromFirst++;

          date.setDate(day);
          week.push(date);
        }

        weeks.push(week);

        if (isPossiblyLastWeek && daysFromFirst >= this.lastMonthDayNumber)
          break;
      }

      return weeks;
    },
  },
  /* --------------------------------- Methods -------------------------------- */
  methods: {
    /** Флаг выбранного дня */
    isSelectedDay(dayDate) {
      return (
        this.selectedMonthIndex === this.currentMonthIndex &&
        dayDate.toDateString() === this.selectedDayDate.toDateString()
      );
    },
    /** Флаг дня соседнего месяца */
    isDisabledDay(dayDate) {
      return dayDate.getMonth() !== this.currentMonthIndex;
    },
    /** Таб-индекс дня (выбранный или первый) */
    getTabIndex(dayDate) {
      return this.isSelectedDay(dayDate) ||
        (this.selectedMonthIndex !== this.currentMonthIndex &&
          dayDate.getDate() === 1)
        ? 0
        : -1;
    },
  },
};
</script>

<style scoped>
/* ---------------------------------- Style --------------------------------- */
.base-calendar-table {
  width: 100%;
}

.base-calendar-table__weekday,
.base-calendar-table__day {
  padding: 0.8rem 0.2rem;
  font-weight: normal;
}

.base-calendar-table__weekday {
  font-size: 1rem;
  word-break: break-all;
}

.base-calendar-table__day {
  width: 100%;
  font-size: 1.5rem;
}

.base-calendar-table__day:disabled {
  color: #ccc;
}

.base-calendar-table__day:hover:not(:disabled):not(
    .base-calendar-table__day--selected
  ) {
  background-color: var(--active-color);
  color: var(--background-color);
}

.base-calendar-table__day--selected:not(:focus) {
  outline: 1px solid var(--active-color);
  cursor: default;
}
</style>
