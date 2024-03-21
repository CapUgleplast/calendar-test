<script setup>

import {computed, onMounted, reactive, ref} from "vue";
import Body from "@/components/Calendar/body.vue";


const emits = defineEmits(['getDate'])

const props = defineProps({
  value: {
    type: String,
    default: ''
  },
  lang: {
    type: Object
  }
})

const checkPropValue = (val) => {
  const cd = new Date(val)
  if(val && !isNaN(cd.getTime())) {
    return cd
  }
  return new Date()
}

const propDate = reactive({ value: checkPropValue(props.value)})

const dates = reactive({ value: [] })



onMounted(()=>{
  dates.value = new Array(35).fill(null)

  monthDeclare()
})


const titleFormatter = computed(() => {
    if(props.lang?.months.length){
      return `${props.lang.months[propDate.value.getMonth()]} ${propDate.value.getFullYear()}`
    }
    return propDate.value.toDateString()
})


const monthDeclare = () => {
  const cd = new Date(propDate.value.getTime())

  cd.setDate(1)

  const firstDateDay = cd.getDay()

  dates.value = dates.value.map((val, id) => {
    const newCD = new Date(cd.getTime())
    newCD.setDate(id - firstDateDay + 2)

    const dateObj = {
      date: newCD.getDate(),
      month: newCD.getMonth(),
      year: newCD.getFullYear(),
    }

    return {
      ...dateObj,
      active: checkActive(dateObj.year, dateObj.month, dateObj.date)
    }

  })
}

const changeMonth = (value) => {
  propDate.value.setMonth(propDate.value.getMonth() + value)
  propDate.value = new Date(propDate.value.getTime())

  monthDeclare()
  changeDate(propDate.value.getFullYear(), propDate.value.getMonth(), propDate.value.getDate())
}

const changeDate = (year, month, date) => {
  const prevActive = dates.value.findIndex((val) => val?.active) || 0
  dates.value[prevActive].active = false

  propDate.value = new Date(year, month, date)
  const newActive = dates.value.findIndex((val) => checkActive(val.year, val.month, val.date))
  dates.value[newActive].active = true

  emitDate()
}


const checkActive = (year, month, date) => {
  return propDate.value.getFullYear() == year && propDate.value.getMonth() == month && propDate.value.getDate() == date
}

const emitDate = () => {
  emits('getDate', propDate.value)
}


</script>

<template>
  <div class="wrapper">
    <div class="header">
      <button class="back"
              v-text="'<'"
              @click="() => changeMonth(-1)"
      />
      <div class="month-title">
        {{ titleFormatter }}
      </div>
      <button class="forward"
              v-text="'>'"
              @click="() => changeMonth(1)"
      />

    </div>
    <div class="body">
        <Body :dates="dates.value"
              @onChangeDate="changeDate"
              :weekdays="lang.weekdays"
        />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.wrapper{
  border: 4px solid aquamarine;

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: nowrap;
    padding: 10px;
    background: aquamarine;
  }
  .body{
    padding: 6px;
  }
}

</style>