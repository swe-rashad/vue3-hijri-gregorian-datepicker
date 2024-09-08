<template>
  <div id="app">
    
    <DatePicker 
      :initialType="calendarType" 
      :withTime="true" 
      v-model="selectedDate" 
      :language="'en'" 
    />
    <!-- <NewDatePicker/> -->
     <!-- <Custom/> -->
     <br>
    <!-- <DatePicker/> -->

  </div>
</template>

<script>

 import DatePicker from '../src/components/DatePicker.vue';
// import NewDatePicker from './components/NewDatePicker.vue';

export default {
  components: {
   // LocalizedDatepicker
   DatePicker,
  // NewDatePicker
  
  },
  data() {
    return {
      calendarType: 'gregorian',
      selectedDate: null,
    };
  },
};
</script>
