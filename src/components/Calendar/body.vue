<script setup>

const emits = defineEmits(["onChangeDate"])

const props = defineProps({
  dates: {
    type: Array,
    default: () => []
  },
  weekdays: {
    type: Array,
    default: () => []
  }
})

const changeDate = (year, month, date) => {
  emits("onChangeDate", year, month, date)
}

</script>

<template>
  <div class="calendar-body">
    <div class="week" v-if="weekdays.length">
      <div class="weekday"
           v-for="day in weekdays"
           :key="day"
           v-text="day"
      />
    </div>
    <div class="dates">
      <div class="date"
           v-for="(date, idx) in dates"
           :key="`${date}+${idx}`"
           v-text="date.date"
           :class="{'--active': date.active}"
           @click="() => changeDate(date.year, date.month, date.date)"
      />
    </div>
  </div>
</template>

<style scoped>
.calendar-body{
  .week{
    margin-bottom: 6px;
    padding-bottom: 3px;
    border-bottom: 1px solid black;
    grid-template-rows: 1fr !important;
  }

  .dates, .week{
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-column-gap: 8px;
    grid-row-gap: 4px;
    text-align: center;

    .date{
      min-width: 26px;
      border-radius: 4px;
      cursor: pointer;

      &:hover{
        background: gainsboro;
      }

      &.--active{
        border: 1px solid blue;
      }
    }
  }
}
</style>