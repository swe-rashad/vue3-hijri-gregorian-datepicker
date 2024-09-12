<template>
  <div  :class="['datepicker','dp__main', themeClass]">
    <!-- Input field to trigger the date picker -->
    <div class="datepicker-input" @click="openDatePicker">
      <input class="dp__pointer dp__input_readonly dp__input dp__input_icon_pad dp__input_focus dp__input_reg"  type="text" :value="formattedDate" :readOnly="readOnly" :placeholder="placeholder"
      :disabled="disabled" />
      <span class="datepicker-icon">
        <CalendarIcon className="dp__input_icon dp__input_icons " :size="48"  />

              </span>
    </div>

    <!-- Date picker dialog with transition -->
    <transition name="fade">
      <div  :class="['datepicker-overlay', themeClass, 'dp__menu dp__menu_index']" v-if="showPicker">
        <div class="datepicker-dialog">
          <div class="calendar">
            <div class="calendar-header">
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="prevYearsOrMonths"><span class="dp__inner_nav"><BackArrowIcon className="custom-arrow-icon" :size="40"  /></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="prevMonth"><span
                  class="dp__inner_nav">    <BackArrowIcon className="custom-arrow-icon" :size="40"  />
                </span></DatePickerButton>
              <span @click="toggleYearSelection">{{ currentMonth }} {{ currentYear }}</span>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="nextYearsOrMonths"><span class="dp__inner_nav"><ForwardArrowIcon className="custom-forward-icon" :size="40"  /></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="nextMonth"><span
                  class="dp__inner_nav">    <ForwardArrowIcon className="custom-forward-icon" :size="40"  />
                </span></DatePickerButton>
            </div>

            <!-- Year Selection -->
            <transition name="fade">
              <div v-if="isYearSelection" class="year-selection">
                <div v-for="year in visibleYears" :key="year" :class="{ selected: year === currentYear }"
                  @click="selectYear(year)">
                  {{ year }}
                </div>
              </div>
            </transition>

            <!-- Month Selection -->
            <transition name="fade">
              <div v-if="isMonthSelection" class="month-selection">
                <div v-for="(month, index) in months" :key="month" :class="{ selected: index + 1 === currentMonth }"
                  @click="selectMonth(index)">
                  {{ month }}
                </div>
              </div>
            </transition>

            <!-- Calendar Grid -->
            <transition name="fade">
              <div v-if="!isYearSelection && !isMonthSelection" class="calendar-grid">
                <div v-for="day in daysOfWeek" :key="day" class="calendar-day-header">
                  {{ day }}
                </div>
                <div v-for="day in daysInMonth" :key="day.date"
                  :class="['calendar-day', { selected: isSelected(day.date) }]" @click="selectDate(day.date)">
                  {{ day.day }}
                </div>
              </div>
            </transition>
          </div>

          <!-- Time Selection (if enabled) -->

          <div v-if="withTime">
            <button type="button" class="dp__btn dp__button" aria-label="Close time Picker" @click="showTimeArea" tabindex="0">
              <ClockIcon className="custom-clock-icon" :size="48"   />
                   
                  </button>
            <div class="dp--tp-wrap">
              
              <div v-if="withTime && showTime" role="dialog" class="dp__overlay dp--overlay-absolute" aria-label="Time picker" tabindex="0">
                <div class="dp__overlay_container dp__container_flex  dp__time_picker_overlay_container"
                  style="display: flex">
                  <!--v-if-->
                  <div class="dp__overlay_row dp__flex_row">
                    <div class="dp__time_input">
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="hours-time-inc-btn-0"
                          aria-label="Increment hours" tabindex="0" @click="incrementHours">
                          <UpArrowIcon className="custom-up-arrow" :size="40"  />
                        </button><button type="button" aria-label="19-Open hours overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="hours-toggle-overlay-btn-0">
                          <!--v-if-->{{ formattedHour }}</button><button type="button" class="dp__btn dp__inc_dec_button"
                          data-test="hours-time-dec-btn-0" aria-label="Decrement hours" tabindex="0"
                          @click="decrementHours">
                          <DownArrowIcon className="custom-down-arrow" :size="40"  />

                        </button>
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        :
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="minutes-time-inc-btn-0"
                          aria-label="Increment minutes" tabindex="0" @click="incrementMinutes">
                          <UpArrowIcon className="custom-up-arrow" :size="40" color="blue" />
                        </button><button type="button" aria-label="56-Open minutes overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="minutes-toggle-overlay-btn-0">
                          <!--v-if-->{{ formattedMinute }}</button><button type="button"
                          class="dp__btn dp__inc_dec_button" data-test="minutes-time-dec-btn-0"
                          aria-label="Decrement minutes" tabindex="0" @click="decrementMinutes">
                          <DownArrowIcon className="custom-down-arrow" :size="40" />

                        </button>
                      </div>
                      <!--v-if--><!--v-if--><!--v-if-->
                    </div>
                  </div>
                  <button type="button" class="dp__btn dp__button" aria-label="Close time Picker" @click="hideTimeArea" tabindex="0">
                    <CalendarIcon className="dp__icon" :size="8"  />
                   
                  </button>
                </div>
              </div>
            </div>
          </div>
          <!-- Actions -->
          <div class="dp__action_row">
            <div class="dp__selection_preview" title="">{{ formattedDate }}</div>
            <div class="dp__action_buttons">

              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_select" @click="confirmSelection">{{
                  translations.ok }}</DatePickerButton>
              </transition>
              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_cancel" @click="cancelSelection">{{
                  translations.cancel }}</DatePickerButton>
              </transition>
              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_select" @click="switchCalendar">{{
                  isHijri
                    ? translations.switchToGregorian
                    : translations.switchToHijri
                }}</DatePickerButton>
              </transition>
            </div>
          </div>

        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, nextTick } from "vue";
import moment from "moment-hijri";
import { format, addMonths, subMonths } from "date-fns";
import DatePickerButton from "./common/DatePickerButton.vue";
import CalendarIcon from './common/CalendarIcon.vue'; // Adjust the path as needed
import ClockIcon from './common/ClockIcon.vue'; // Adjust the path as needed
import BackArrowIcon from './common/BackArrowIcon.vue'; // Adjust the path as needed
import ForwardArrowIcon  from './common/ForwardArrowIcon.vue'; // Adjust the path as needed
import DownArrowIcon  from './common/DownArrowIcon.vue'; // Adjust the path as needed
import UpArrowIcon  from './common/UpArrowIcon.vue'; // Adjust the path as needed

const props = defineProps({
  initialType: {
    type: String,
    default: "gregorian",
  },
  withTime: {
    type: Boolean,
    default: false,
  },
  modelValue: {
    type: Object, // or String, depending on your setup
    default: () => ({ date: '', type: 'gregorian' }),
    
  },
  language: {
    type: String,
    default: "en",
  },
  format: {
    type: String,
    default: "", // Allows passing a custom format
  },

    disabled: {
      type: Boolean,
      default: false,
    },
    readOnly: {
      type: Boolean,
      default: true,
    },
   
    placeholder: {
      type: String,
      default: 'Select date',
    },
    darkTheme: {
      type: Boolean,
      default: false // Default to dark theme
    }

});

const emit = defineEmits(["update:modelValue", "cancel"]);

const themeClass = computed(() => props.darkTheme ? 'dp__theme_dark' : 'dp__theme_light');
const isHijri = ref(props.initialType === "hijri");
const showPicker = ref(false);
const showTime = ref(false);
const currentDate = ref(
  isHijri.value ? moment().startOf("day").toDate() : new Date()
);
const selectedDate = ref(currentDate.value);
const selectedHour = ref(currentDate.value.getHours());
const selectedMinute = ref(currentDate.value.getMinutes());
const selectedSecond = ref(currentDate.value.getSeconds());


const isYearSelection = ref(false);
const isMonthSelection = ref(false);




const yearRangeStart = ref(isHijri.value ? 1400 : 2000);
const formattedHour = computed(() => pad(selectedHour.value));
const formattedMinute = computed(() => pad(selectedMinute.value));
const translations = computed(() => {
  return props.language === "ar"
    ? {
      ok: "موافق",
      cancel: "إلغاء",
      switchToGregorian: "التبديل إلى الميلادي",
      switchToHijri: "التبديل إلى الهجري",
      hh: "ساعة",
      mm: "دقيقة",
      ss: "ثانية",
      monthsGregorian: [
        "يناير",
        "فبراير",
        "مارس",
        "أبريل",
        "مايو",
        "يونيو",
        "يوليو",
        "أغسطس",
        "سبتمبر",
        "أكتوبر",
        "نوفمبر",
        "ديسمبر",
      ],
      monthsHijri: [
        "محرم",
        "صفر",
        "ربيع الأول",
        "ربيع الآخر",
        "جمادى الأولى",
        "جمادى الآخرة",
        "رجب",
        "شعبان",
        "رمضان",
        "شوال",
        "ذو القعدة",
        "ذو الحجة",
      ],
      daysOfWeek: [
        "الإثنين",
        "الثلاثاء",
        "الأربعاء",
        "الخميس",
        "الجمعة",
        "السبت",
        "الأحد",
      ],
    }
    : {
      ok: "Select",
      cancel: "Cancel",
      switchToGregorian: "Switch to Gregorian",
      switchToHijri: "Switch to Hijri",
      hh: "HH",
      mm: "MM",
      ss: "SS",
      monthsGregorian: [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ],
      monthsHijri: [
        "Muharram",
        "Safar",
        "Rabi I",
        "Rabi II",
        "Jumada I",
        "Jumada II",
        "Rajab",
        "Shaaban",
        "Ramadan",
        "Shawwal",
        "Dhu al-Qidah",
        "Dhu al-Hijjah",
      ],
      daysOfWeek: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    };
});

const currentMonth = computed(() => {
  return isHijri.value
    ? translations.value.monthsHijri[moment(selectedDate.value).iMonth()]
    : translations.value.monthsGregorian[selectedDate.value.getMonth()];
});

const currentYear = computed(() => {
  return isHijri.value
    ? moment(selectedDate.value).format("iYYYY")
    : format(selectedDate.value, "yyyy");
});

const visibleYears = ref([]);

const months = computed(() => {
  return isHijri.value
    ? translations.value.monthsHijri
    : translations.value.monthsGregorian;
});

const daysOfWeek = computed(() => {
  return translations.value.daysOfWeek;
});

const daysInMonth = computed(() => {
  const days = [];
  const startOfMonth = isHijri.value
    ? moment(selectedDate.value).startOf("iMonth")
    : new Date(
      selectedDate.value.getFullYear(),
      selectedDate.value.getMonth(),
      1
    );

  const startDay = isHijri.value
    ? startOfMonth.isoWeekday() % 7
    : startOfMonth.getDay() === 0
      ? 7
      : startOfMonth.getDay();

  for (let i = 0; i < startDay; i++) {
    days.push({ day: "", date: null });
  }

  const totalDays = isHijri.value
    ? moment(selectedDate.value).iDaysInMonth()
    : new Date(
      selectedDate.value.getFullYear(),
      selectedDate.value.getMonth() + 1,
      0
    ).getDate();

  for (let day = 1; day <= totalDays; day++) {
    const date = isHijri.value
      ? moment(startOfMonth).iDate(day).toDate()
      : new Date(startOfMonth.getFullYear(), startOfMonth.getMonth(), day);
    days.push({ day, date });
  }

  return days;
});

const pad = (value) => String(value).padStart(2, "0");

const prevMonth = () => {
  selectedDate.value = isHijri.value
    ? moment(selectedDate.value).subtract(1, "iMonth").toDate()
    : subMonths(selectedDate.value, 1);
  updateDate();
};

const nextMonth = () => {
  selectedDate.value = isHijri.value
    ? moment(selectedDate.value).add(1, "iMonth").toDate()
    : addMonths(selectedDate.value, 1);
  updateDate();
};

const selectYear = (year) => {
  if (isHijri.value) {
    selectedDate.value = moment(selectedDate.value).iYear(year).toDate();
  } else {
    selectedDate.value.setFullYear(year);
  }
  isYearSelection.value = false;
  isMonthSelection.value = true;
  updateDate();
};

const selectMonth = (monthIndex) => {
  if (isHijri.value) {
    selectedDate.value = moment(selectedDate.value)
      .iMonth(monthIndex)
      .startOf("iMonth")
      .toDate();
  } else {
    selectedDate.value.setMonth(monthIndex);
  }
  isMonthSelection.value = false;
  updateDate();
};

const selectDate = (date) => {
  if (date) {
    selectedDate.value = date;
 
    confirmSelection();
    
    
  }
};

const showTimeArea = () => {
  showTime.value = true;
  
};
const hideTimeArea = () => {
  showTime.value = false;
  
};
const incrementHours = () => {
  var hour = (selectedHour.value + 1) % 24;
  selectedHour.value = hour;
  selectedDate.value.setHours(hour);

};
const decrementHours = () => {
  var hour = (selectedHour.value - 1 + 24) % 24;
  selectedHour.value = hour;
  selectedDate.value.setHours(hour);

};
const incrementMinutes = () => {
  var minute = (selectedMinute.value + 1) % 60;
  selectedMinute.value = minute;
  selectedDate.value.setMinutes(minute);

};
const decrementMinutes = () => {
  var minute = (selectedMinute.value - 1 + 60) % 60;
  selectedMinute.value = minute;
  selectedDate.value.setMinutes(minute);

};
const selectMinute = (minute) => {
  selectedMinute.value = minute;
  selectedDate.value.setMinutes(minute);
};

const selectSecond = (second) => {
  selectedSecond.value = second;
  selectedDate.value.setSeconds(second);
};

const switchCalendar = () => {
  isHijri.value = !isHijri.value;

  let convertedDate;
  if (isHijri.value) {
    convertedDate = moment(selectedDate.value).format("iYYYY/iMM/iDD");
    selectedDate.value = moment(convertedDate, "iYYYY/iMM/iDD").toDate();
  } else {
    try {
      convertedDate = moment(selectedDate.value).format("DD-MM-YYYY");
      selectedDate.value = new Date(convertedDate);
    } catch (error) {
      console.error("Error converting Hijri date to Gregorian:", error);
    }
  }

  yearRangeStart.value = isHijri.value
    ? moment(selectedDate.value).iYear() -
    (moment(selectedDate.value).iYear() % 10)
    : selectedDate.value.getFullYear() -
    (selectedDate.value.getFullYear() % 10);

  updateVisibleYears();

  nextTick(() => {
    if (isYearSelection.value) {
      isYearSelection.value = true;
    } else if (isMonthSelection.value) {
      isMonthSelection.value = true;
    }
  });
};

const isSelected = (date) => {
  return date && date.toDateString() === selectedDate.value.toDateString();
};

const confirmSelection = async () => {
  // Create a new date object with selected time
  const finalDate = new Date(selectedDate.value);
  finalDate.setHours(selectedHour.value, selectedMinute.value, selectedSecond.value, 0);

  // Convert the date to UTC for consistency
  const adjustedDate = new Date(
    Date.UTC(
      finalDate.getFullYear(),
      finalDate.getMonth(),
      finalDate.getDate(),
      finalDate.getHours(),
      finalDate.getMinutes(),
      finalDate.getSeconds()
    )
  );

  // Determine the calendar type and format accordingly
  const calendarType = isHijri.value ? "hijri" : "gregorian";

  let formattedDate;
  if (calendarType === "hijri") {
    formattedDate = moment(adjustedDate).format("iDD-iMM-iYYYY HH:mm:ss"); // Hijri format
  } else {
    formattedDate = format(adjustedDate, "dd-MM-yyyy HH:mm:ss"); // Gregorian format
  }

  // Emit the formatted date along with calendar type
  emit("update:modelValue", {
    date: formattedDate,
    type: calendarType,
  });

  // Close the date picker
  showPicker.value = false;
};

const cancelSelection = () => {
  emit("cancel");
  showPicker.value = false;
};

const toggleYearSelection = () => {
  isYearSelection.value = !isYearSelection.value;
  isMonthSelection.value = false;
  if (isYearSelection.value) {
    updateVisibleYears();
  }
};

const prevYearsOrMonths = () => {
  yearRangeStart.value -= 10;
  updateVisibleYears();
};

const nextYearsOrMonths = () => {
  yearRangeStart.value += 10;
  updateVisibleYears();
};

const updateDate = () => {
  selectedDate.value = new Date(selectedDate.value);
  calculateDaysInMonth();
};

const updateVisibleYears = () => {
  visibleYears.value = Array.from(
    { length: 10 },
    (_, i) => yearRangeStart.value + i
  );
};

const calculateDaysInMonth = () => {
  const days = [];
  const startOfMonth = isHijri.value
    ? moment(selectedDate.value).startOf("iMonth")
    : moment(selectedDate.value).startOf("month");

  const startDay = isHijri.value
    ? startOfMonth.isoWeekday() % 7
    : startOfMonth.day();

  for (let i = 0; i < startDay; i++) {
    days.push({ day: "", date: null });
  }

  const totalDays = isHijri.value
    ? moment(selectedDate.value).iDaysInMonth()
    : moment(selectedDate.value).daysInMonth();

  for (let day = 1; day <= totalDays; day++) {
    const date = isHijri.value
      ? moment(startOfMonth).iDate(day).toDate()
      : moment(startOfMonth).date(day).toDate();
    days.push({ day, date });
  }

  daysInMonth.value = days;
};

const openDatePicker = () => {
  showPicker.value = true;
};
const formattedDate = computed(() => {
  // Set default formats based on Hijri or Gregorian, and with or without time
  const defaultFormat = isHijri.value ? "iDD-iMM-iYYYY" : "dd-MM-yyyy";
  const timeFormat = isHijri.value
    ? "iDD-iMM-iYYYY  HH:mm:ss"
    : "dd-MM-yyyy  HH:mm:ss";

  // If a custom format is provided, use it; otherwise, use default formats
  const formatString = props.format
    ? props.format
    : props.withTime
      ? timeFormat
      : defaultFormat;

  // Update the selected date with the correct hours, minutes, and seconds
  selectedDate.value.setHours(
    selectedHour.value,
    selectedMinute.value,
    selectedSecond.value,
    0
  );

  // Format the date using Moment.js for Hijri and date-fns for Gregorian
  return isHijri.value
    ? moment(selectedDate.value).format(formatString)
    : format(selectedDate.value, formatString);
});


onMounted(() => {
  if (props.modelValue && props.modelValue.date && props.modelValue.type) {
    // Set calendar type
    isHijri.value = props.modelValue.type === "hijri";

    // Convert the date based on the calendar type
    if (isHijri.value) {
      // Convert Hijri date to Gregorian date
      selectedDate.value = moment(props.modelValue.date, "iDD-iMM-iYYYY").toDate();
    } else {
      // Use the Gregorian date directly
      selectedDate.value = new Date(props.modelValue.date);
    }

    // Extract time components
    selectedHour.value = selectedDate.value.getHours();
    selectedMinute.value = selectedDate.value.getMinutes();
    selectedSecond.value = selectedDate.value.getSeconds();
  } else {
    // Default to the current date and time
    selectedDate.value = isHijri.value
      ? moment().startOf("day").toDate()  // Hijri default
      : new Date();                     // Gregorian default
    selectedHour.value = selectedDate.value.getHours();
    selectedMinute.value = selectedDate.value.getMinutes();
    selectedSecond.value = selectedDate.value.getSeconds();
  }

  // Update visible years in the calendar
  updateVisibleYears();
});













</script>
