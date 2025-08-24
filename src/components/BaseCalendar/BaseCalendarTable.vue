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
    <tbody>
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
    </tbody>
  </table>
</template>

<script>
export default {
  name: "BaseCalendarTable",
  data() {
    return {
      /** Наименования дней недели */
      weekdays: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      /** Количество дней в месяце */
      daysAmount: 31,
      /** Максимально возможное количество недель в месяце */
      maxWeeksAmount: 6,
      /** Выбранный день */
      selectedDay: 10,
    };
  },
  computed: {
    /** Количество дней в неделе */
    weekdaysAmount() {
      return this.weekdays.length;
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
