<template>
  <div>
    <Calendar
        timeOnly
        v-model="time"
        hourFormat="24"
        :showIcon="showIcon"
        @update:modelValue="onUpdateTime"
    >
      <template #footer>
        <div class="time-picker-buttons-wrapper">
          <button @click="getCurrentTime" class="time-picker-button">
            Сегодня
          </button>
          <button @click="clear" class="time-picker-button">
            Очистить
          </button>
        </div>
      </template>
    </Calendar>

  </div>
</template>

<script>
import Calendar from 'primevue/calendar';

export default {
  name: "DateTimePickerComponent",
  components: {
    Calendar,
  },
  emits: ["timeUpdate"],
  props: {
    selectedTime: {
      type: Date,
      required: false,
      default: null
    },
    showIcon: {
      type: Boolean,
      required: false,
      default: false,
    }
  },
  data() {
    return {
      time: null,
    }
  },
  beforeMount() {
    this.$data.time = this.$.props.selectedTime;
  },
  methods: {
    clear() {
      const today = new Date();
      today.setHours(0, 0, 0);
      this.$data.time = today;
      this.onUpdateTime();
    },
    getCurrentTime() {
      this.$data.time = new Date();
      this.onUpdateTime();
    },
    onUpdateTime() {
      this.$emit("timeUpdate", this.timeFormat);
    }
  },
  computed: {
    timeFormat() {
      const { time } = this.$data;
      const hours = time.getHours();
      const minutes = time.getMinutes();
      const hoursCorrect = hours < 10 ? "0" + hours : hours;
      const minutesCorrect = minutes < 10 ? "0" + minutes : minutes;
      return hoursCorrect + ":" + minutesCorrect;
    }
  }
}
</script>

<style scoped>
.time-picker-buttons-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  padding: 16px 0;
  border-top: 1px solid lightgray;
}
.time-picker-button {
  color: deepskyblue;
  padding: 8px;
  border: none;
  background: transparent;
  cursor: pointer;
}
.time-picker-button:hover {
  color: blue;
}
.p-calendar {
  width: 100%;
}
</style>