<template>
  <div id="app">
    <DatePicker 
      :initialType="calendarType" 
      :withTime="true" 
      v-model="selectedDate" 
      :language="'en'" 
      :darkTheme="true"
    />
    <!-- :format="'yyyy-MM-dd HH:mm:ss '" -->
    <!-- Display the selected date and type     format="dd-mm-yyyy HH:mm" -->
    <p>Selected Date: {{ selectedDate.date }}</p>
    <p>Calendar Type: {{ selectedDate.type }}</p>
  </div>
</template>


<script>
 import DatePicker from '../src/components/DatePicker.vue';
 // import NewDatePicker from './components/NewDatePicker.vue';

 export default {
  components: {
    DatePicker,
  },
  data() {
    return {

      calendarType: 'gregorian',
      selectedDate: {
        date: this.getCurrentDateTime(),
        type: 'gregorian',
      },
    };
  },
  methods: {
    getCurrentDateTime() {
      const now = new Date();
      const year = now.getFullYear();
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const day = String(now.getDate()).padStart(2, '0');
      const hours = String(now.getHours()).padStart(2, '0');
      const minutes = String(now.getMinutes()).padStart(2, '0');
      const seconds = String(now.getSeconds()).padStart(2, '0');
      
      // Format: YYYY-MM-DDTHH:mm:ss
      return `${year}-${month}-${day}T${hours}:${minutes}:${seconds}`;
    }
  },
  watch: {
    selectedDate: {
      handler(newValue) {
        // Whenever selectedDate changes, you can handle it here
        console.log('Updated selectedDate:', newValue);
      },
      deep: true, // To detect changes in the nested object (date, type)
    },
  },
};
</script>