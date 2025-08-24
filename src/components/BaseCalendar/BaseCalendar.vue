<template>
  <section class="base-calendar">
    <BaseCalendarHeader />
    <BaseCalendarTable />
  </section>
</template>

<script>
import BaseCalendarHeader from "./BaseCalendarHeader.vue";
import BaseCalendarTable from "./BaseCalendarTable.vue";

export default {
  name: "BaseCalendar",
  components: {
    BaseCalendarHeader,
    BaseCalendarTable,
  },
};
</script>

<style scoped>
.base-calendar {
  width: 24rem;
  border: 1px solid #ccc;
  text-align: center;
  color: #333;
}
</style>
