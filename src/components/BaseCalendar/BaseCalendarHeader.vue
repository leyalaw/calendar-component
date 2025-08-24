<template>
  <header class="base-calendar-header">
    <!-- Предыдущий месяц -->
    <button
      type="button"
      class="base-calendar-header__month-button base-calendar-header__month-button--previous"
    >
      <!-- стрелка влево -->
      <span class="base-calendar-header__month-arrow"></span>
    </button>

    <!-- Месяц и год -->
    <div class="base-calendar-header__title">
      <span class="base-calendar-header__month">Jul</span>
      <span class="base-calendar-header__year">2018</span>
    </div>

    <!-- Следующий месяц -->
    <button
      type="button"
      class="base-calendar-header__month-button base-calendar-header__month-button--next"
    >
      <!-- стрелка вправо -->
      <span class="base-calendar-header__month-arrow"></span>
    </button>
  </header>
</template>

<script>
export default {
  name: "BaseCalendarHeader",
};
</script>

<style scoped>
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
