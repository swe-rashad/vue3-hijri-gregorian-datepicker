<template>
  <div class="datepicker dp__main dp__theme_light">
    <!-- Input field to trigger the date picker -->
    <div class="datepicker-input" @click="openDatePicker">
      <input class="dp__pointer dp__input_readonly dp__input dp__input_icon_pad dp__input_focus dp__input_reg"  type="text" :value="formattedDate" placeholder="Select date" readonly />
      <span class="datepicker-icon"> <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor" aria-hidden="true" class="dp__input_icon dp__input_icons" role="img" aria-label="Calendar icon">
                   <path d="M29.333 8c0-2.208-1.792-4-4-4h-18.667c-2.208 0-4 1.792-4 4v18.667c0 2.208 1.792 4 4 4h18.667c2.208 0 4-1.792 4-4v-18.667zM26.667 8v18.667c0 0.736-0.597 1.333-1.333 1.333 0 0-18.667 0-18.667 0-0.736 0-1.333-0.597-1.333-1.333 0 0 0-18.667 0-18.667 0-0.736 0.597-1.333 1.333-1.333 0 0 18.667 0 18.667 0 0.736 0 1.333 0.597 1.333 1.333z"></path>
                   <path d="M20 2.667v5.333c0 0.736 0.597 1.333 1.333 1.333s1.333-0.597 1.333-1.333v-5.333c0-0.736-0.597-1.333-1.333-1.333s-1.333 0.597-1.333 1.333z"></path>
                   <path d="M9.333 2.667v5.333c0 0.736 0.597 1.333 1.333 1.333s1.333-0.597 1.333-1.333v-5.333c0-0.736-0.597-1.333-1.333-1.333s-1.333 0.597-1.333 1.333z"></path>
                   <path d="M4 14.667h24c0.736 0 1.333-0.597 1.333-1.333s-0.597-1.333-1.333-1.333h-24c-0.736 0-1.333 0.597-1.333 1.333s0.597 1.333 1.333 1.333z"></path>
                </svg></span>
    </div>

    <!-- Date picker dialog with transition -->
    <transition name="fade">
      <div class="datepicker-overlay dp__menu dp__menu_index dp__theme_light" v-if="showPicker">
        <div class="datepicker-dialog">
          <div class="calendar">
            <div class="calendar-header">
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="prevYearsOrMonths"><span class="dp__inner_nav"><!--v-if--><svg
                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor" aria-hidden="true"
                    class="dp__icon" role="img">
                    <path
                      d="M20.943 23.057l-7.057-7.057c0 0 7.057-7.057 7.057-7.057 0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-8 8c-0.521 0.521-0.521 1.365 0 1.885l8 8c0.52 0.52 1.365 0.52 1.885 0s0.52-1.365 0-1.885z">
                    </path>
                  </svg></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="prevMonth"><span
                  class="dp__inner_nav"><!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32"
                    fill="currentColor" aria-hidden="true" class="dp__icon" role="img">
                    <path
                      d="M20.943 23.057l-7.057-7.057c0 0 7.057-7.057 7.057-7.057 0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-8 8c-0.521 0.521-0.521 1.365 0 1.885l8 8c0.52 0.52 1.365 0.52 1.885 0s0.52-1.365 0-1.885z">
                    </path>
                  </svg></span></DatePickerButton>
              <span @click="toggleYearSelection">{{ currentMonth }} {{ currentYear }}</span>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="nextYearsOrMonths"><span class="dp__inner_nav"><!--v-if--><svg
                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor" aria-hidden="true"
                    class="dp__icon" role="img">
                    <path
                      d="M12.943 24.943l8-8c0.521-0.521 0.521-1.365 0-1.885l-8-8c-0.52-0.52-1.365-0.52-1.885 0s-0.52 1.365 0 1.885l7.057 7.057c0 0-7.057 7.057-7.057 7.057-0.52 0.52-0.52 1.365 0 1.885s1.365 0.52 1.885 0z">
                    </path>
                  </svg></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="nextMonth"><span
                  class="dp__inner_nav"><!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32"
                    fill="currentColor" aria-hidden="true" class="dp__icon" role="img">
                    <path
                      d="M12.943 24.943l8-8c0.521-0.521 0.521-1.365 0-1.885l-8-8c-0.52-0.52-1.365-0.52-1.885 0s-0.52 1.365 0 1.885l7.057 7.057c0 0-7.057 7.057-7.057 7.057-0.52 0.52-0.52 1.365 0 1.885s1.365 0.52 1.885 0z">
                    </path>
                  </svg></span></DatePickerButton>
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
            <div class="dp--tp-wrap">
              <button type="button" class="dp__btn dp__button " aria-label="Open time picker" tabindex="0"
                data-test="open-time-picker-btn">
                <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                  aria-hidden="true" class="dp__icon" role="img" v-if="withTime"  @click="showTimeArea">
                  <path
                    d="M16 1.333c-8.095 0-14.667 6.572-14.667 14.667s6.572 14.667 14.667 14.667c8.095 0 14.667-6.572 14.667-14.667s-6.572-14.667-14.667-14.667zM16 4c6.623 0 12 5.377 12 12s-5.377 12-12 12c-6.623 0-12-5.377-12-12s5.377-12 12-12z">
                  </path>
                  <path
                    d="M14.667 8v8c0 0.505 0.285 0.967 0.737 1.193l5.333 2.667c0.658 0.329 1.46 0.062 1.789-0.596s0.062-1.46-0.596-1.789l-4.596-2.298c0 0 0-7.176 0-7.176 0-0.736-0.597-1.333-1.333-1.333s-1.333 0.597-1.333 1.333z">
                  </path>
                </svg>
              </button>
              <div v-if="withTime && showTime" role="dialog" class="dp__overlay dp--overlay-absolute" aria-label="Time picker" tabindex="0">
                <div class="dp__overlay_container dp__container_flex  dp__time_picker_overlay_container"
                  style="display: flex">
                  <!--v-if-->
                  <div class="dp__overlay_row dp__flex_row">
                    <div class="dp__time_input">
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="hours-time-inc-btn-0"
                          aria-label="Increment hours" tabindex="0" @click="incrementHours">
                          <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                            aria-hidden="true" class="dp__icon" role="img">
                            <path
                              d="M24.943 19.057l-8-8c-0.521-0.521-1.365-0.521-1.885 0l-8 8c-0.52 0.52-0.52 1.365 0 1.885s1.365 0.52 1.885 0l7.057-7.057c0 0 7.057 7.057 7.057 7.057 0.52 0.52 1.365 0.52 1.885 0s0.52-1.365 0-1.885z">
                            </path>
                          </svg></button><button type="button" aria-label="19-Open hours overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="hours-toggle-overlay-btn-0">
                          <!--v-if-->{{ selectedHour }}</button><button type="button" class="dp__btn dp__inc_dec_button"
                          data-test="hours-time-dec-btn-0" aria-label="Decrement hours" tabindex="0"
                          @click="decrementHours">
                          <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                            aria-hidden="true" class="dp__icon" role="img">
                            <path
                              d="M7.057 12.943l8 8c0.521 0.521 1.365 0.521 1.885 0l8-8c0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-7.057 7.057c0 0-7.057-7.057-7.057-7.057-0.52-0.52-1.365-0.52-1.885 0s-0.52 1.365 0 1.885z">
                            </path>
                          </svg>
                        </button>
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        :
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="minutes-time-inc-btn-0"
                          aria-label="Increment minutes" tabindex="0" @click="incrementMinutes">
                          <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                            aria-hidden="true" class="dp__icon" role="img">
                            <path
                              d="M24.943 19.057l-8-8c-0.521-0.521-1.365-0.521-1.885 0l-8 8c-0.52 0.52-0.52 1.365 0 1.885s1.365 0.52 1.885 0l7.057-7.057c0 0 7.057 7.057 7.057 7.057 0.52 0.52 1.365 0.52 1.885 0s0.52-1.365 0-1.885z">
                            </path>
                          </svg></button><button type="button" aria-label="56-Open minutes overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="minutes-toggle-overlay-btn-0">
                          <!--v-if-->{{ selectedMinute }}</button><button type="button"
                          class="dp__btn dp__inc_dec_button" data-test="minutes-time-dec-btn-0"
                          aria-label="Decrement minutes" tabindex="0" @click="decrementMinutes">
                          <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                            aria-hidden="true" class="dp__icon" role="img">
                            <path
                              d="M7.057 12.943l8 8c0.521 0.521 1.365 0.521 1.885 0l8-8c0.52-0.52 0.52-1.365 0-1.885s-1.365-0.52-1.885 0l-7.057 7.057c0 0-7.057-7.057-7.057-7.057-0.52-0.52-1.365-0.52-1.885 0s-0.52 1.365 0 1.885z">
                            </path>
                          </svg>
                        </button>
                      </div>
                      <!--v-if--><!--v-if--><!--v-if-->
                    </div>
                  </div>
                  <button type="button" class="dp__btn dp__button" aria-label="Close time Picker" tabindex="0">
                    <!--v-if--><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="currentColor"
                      aria-hidden="true" class="dp__icon" role="img" @click="hideTimeArea">
                      <path
                        d="M29.333 8c0-2.208-1.792-4-4-4h-18.667c-2.208 0-4 1.792-4 4v18.667c0 2.208 1.792 4 4 4h18.667c2.208 0 4-1.792 4-4v-18.667zM26.667 8v18.667c0 0.736-0.597 1.333-1.333 1.333 0 0-18.667 0-18.667 0-0.736 0-1.333-0.597-1.333-1.333 0 0 0-18.667 0-18.667 0-0.736 0.597-1.333 1.333-1.333 0 0 18.667 0 18.667 0 0.736 0 1.333 0.597 1.333 1.333z">
                      </path>
                      <path
                        d="M20 2.667v5.333c0 0.736 0.597 1.333 1.333 1.333s1.333-0.597 1.333-1.333v-5.333c0-0.736-0.597-1.333-1.333-1.333s-1.333 0.597-1.333 1.333z">
                      </path>
                      <path
                        d="M9.333 2.667v5.333c0 0.736 0.597 1.333 1.333 1.333s1.333-0.597 1.333-1.333v-5.333c0-0.736-0.597-1.333-1.333-1.333s-1.333 0.597-1.333 1.333z">
                      </path>
                      <path
                        d="M4 14.667h24c0.736 0 1.333-0.597 1.333-1.333s-0.597-1.333-1.333-1.333h-24c-0.736 0-1.333 0.597-1.333 1.333s0.597 1.333 1.333 1.333z">
                      </path>
                    </svg>
                  </button>
                </div>
              </div>
            </div>
          </div>
          <!-- <div v-if="withTime" class="time-selection">
            <div class="time-scroller">
              <div class="time-label">{{ translations.hh }}</div>
              <transition name="fade">
                <ul class="time-list">
                  <li
                    v-for="hour in 24"
                    :key="hour"
                    :class="{ selected: selectedHour === hour - 1 }"
                    @click="selectHour(hour - 1)"
                  >
                    {{ pad(hour - 1) }}
                  </li>
                </ul>
              </transition>
            </div>
            <div class="time-scroller">
              <div class="time-label">{{ translations.mm }}</div>
              <transition name="fade">
                <ul class="time-list">
                  <li
                    v-for="minute in 60"
                    :key="minute"
                    :class="{ selected: selectedMinute === minute - 1 }"
                    @click="selectMinute(minute - 1)"
                  >
                    {{ pad(minute - 1) }}
                  </li>
                </ul>
              </transition>
            </div>
            <div class="time-scroller">
              <div class="time-label">{{ translations.ss }}</div>
              <transition name="fade">
                <ul class="time-list">
                  <li
                    v-for="second in 60"
                    :key="second"
                    :class="{ selected: selectedSecond === second - 1 }"
                    @click="selectSecond(second - 1)"
                  >
                    {{ pad(second - 1) }}
                  </li>
                </ul>
              </transition>
            </div>
          </div> -->

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
    type: String,
    default: "",
  },
  language: {
    type: String,
    default: "en",
  },
  format: {
    type: String,
    default: "", // Allows passing a custom format
  },
});

const emit = defineEmits(["update:modelValue", "cancel"]);

const isHijri = ref(props.initialType === "hijri");
const showPicker = ref(false);
const showTime = ref(false);
const currentDate = ref(
  isHijri.value ? moment().startOf("day").toDate() : new Date()
);
const selectedDate = ref(currentDate.value);
const selectedHour = ref(0);
const selectedMinute = ref(0);
const selectedSecond = ref(0);
const isYearSelection = ref(false);
const isMonthSelection = ref(false);
const yearRangeStart = ref(isHijri.value ? 1400 : 2000);

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
      convertedDate = moment(selectedDate.value).format("YYYY-MM-DD");
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
  const finalDate = new Date(selectedDate.value);
  finalDate.setHours(
    selectedHour.value,
    selectedMinute.value,
    selectedSecond.value,
    0
  );
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
  emit("update:modelValue", adjustedDate.toISOString());

  showPicker.value = false;

  // await nextTick();
  alert(showPicker.value);
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
  const defaultFormat = isHijri.value ? "iYYYY/iMM/iDD" : "yyyy-MM-dd";
  const timeFormat = isHijri.value
    ? "iYYYY/iMM/iDD HH:mm:ss"
    : "yyyy-MM-dd HH:mm:ss";

  const formatString = props.format
    ? props.format
    : props.withTime
      ? timeFormat
      : defaultFormat;
  selectedDate.value.setHours(
    selectedHour.value,
    selectedMinute.value,
    selectedSecond.value,
    0
  );

  return isHijri.value
    ? moment(selectedDate.value).format(formatString)
    : format(selectedDate.value, formatString);
});

onMounted(() => {
  if (props.modelValue) {
    selectedDate.value = isHijri.value
      ? moment.utc(props.modelValue).local().toDate()
      : new Date(props.modelValue);

    selectedHour.value = selectedDate.value.getHours();
    selectedMinute.value = selectedDate.value.getMinutes();
    selectedSecond.value = selectedDate.value.getSeconds();
  } else {
    selectedDate.value = isHijri.value
      ? moment().startOf("day").toDate()
      : new Date();
  }
  updateVisibleYears();
});
</script>

<style scoped>
/* Add your component styles here */
</style>
