<template>
  <table class="base-calendar-table">
    <!-- Наименования дней недели -->
    <thead>
      <tr>
        <th
          v-for="weekday in weekdays"
          :key="weekday"
          class="base-calendar-table__weekday"
        >
          {{ weekday }}
        </th>
      </tr>
    </thead>

    <!-- Таблица дней -->
    <!-- <tbody>
      <tr v-for="week in weeks" :key="week">
        <td v-for="day in week" :key="day">
          <button
            type="button"
            class="base-calendar-table__day"
            :class="{
              'base-calendar-table__day--selected': day === selectedDay,
            }"
          >
            {{ day }}
          </button>
        </td>
      </tr>
    </tbody> -->
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
  /* ---------------------------------- Data ---------------------------------- */
  data() {
    return {
      /** Количество дней в месяце */
      daysAmount: 31,
      /** Максимально возможное количество недель в месяце */
      maxWeeksAmount: 6,
      /** Выбранный день */
      selectedDay: 10,
    };
  },
  /* -------------------------------- Computed -------------------------------- */
  computed: {
    /** флаг календаря, в котором неделя начинается с воскресенья */
    isSundayWeek() {
      return SUNDAY_WEEK_LOCALES.includes(this.locale);
    },
    /** Форматтер сокращённых названий дней недели */
    weekdayFormatter() {
      return new Intl.DateTimeFormat(this.locale, { weekday: "short" });
    },
    /** Сокращённые наименования дней недели */
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
        weekdays.push(this.weekdayFormatter.format(date));
      }

      return weekdays;
    },
    /** Дни, сгруппированные по неделям */
    weeks() {
      const weeks = [];

      let dayNumber = 1;

      for (let weekIndex = 0; weekIndex < this.maxWeeksAmount; weekIndex++) {
        if (dayNumber > this.daysAmount) break;

        const week = [];

        for (
          let weekdayIndex = 0;
          weekdayIndex < this.weekdaysAmount;
          weekdayIndex++
        ) {
          week.push(dayNumber++);
          if (dayNumber > this.daysAmount) break;
        }

        weeks.push(week);
      }

      return weeks;
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

.base-calendar-table__day--selected {
  outline: 1px solid #44bbdd;
}
</style>
