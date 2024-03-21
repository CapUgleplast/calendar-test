<script setup>
import Calendar from "@/components/Calendar/index.vue"
import langugeConfig from "@/components/Calendar/langugeConfig.js";
import {ref} from "vue";


const date = ref()

const setDate = (val) => {
  date.value = val
}
</script>

<template>
    <div class="mainWrap">
      <Calendar :lang="langugeConfig.ru"
                :value="'2010-03-11'"
                @get-date="setDate"
      />
      <div class="get-date">
        {{ date || '&nbsp;' }}
      </div>
    </div>
</template>

<style scoped>
  .mainWrap{
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
  }
</style>