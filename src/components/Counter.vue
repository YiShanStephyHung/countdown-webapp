<template>
  <div v-if="loaded">
    <section
      class="
        flex
        justify-center
        content-center
        mx-auto
        my-3
        text-2xl text-center
      "
    >
      <h3 v-if="!expired" class="text-6xl font-light mx-8">Count Down for <span class="font-bold">{{ purpose }}</span> !</h3>
      <h3 v-else>Time's Up</h3>
    </section>
    <section class="my-5 text-xl">
      Due on {{ year }} - {{ month + 1 }} - {{ date }}
    </section>
    <section class="flex justify-center content-center text-5xl">
      <div class="days mr-4 relative">
        {{ displayDays }}
        <div class="label text-base bottom--1 right-0 absolute">days</div>
      </div>
      <span class="leading-tight">:</span>
      <div class="hours mx-4 relative">
        {{ displayHours }}
        <div class="label text-base bottom--1 right-0 absolute">hours</div>
      </div>
      <span class="leading-tight">:</span>
      <div class="minutes mx-4 relative">
        {{ displayMinutes }}
        <div class="label text-base bottom--1 right-0 absolute">minutes</div>
      </div>
      <span class="leading-tight">:</span>
      <div class="seconds ml-4 relative">
        {{ displaySeconds }}
        <div class="label text-base bottom--1 right-0 absolute">seconds</div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  props: [
    "year",
    "month",
    "date",
    "hour",
    "minute",
    "second",
    "millisecond",
    "purpose",
  ],
  data() {
    return {
      displayDays: 0,
      displayHours: 0,
      displayMinutes: 0,
      displaySeconds: 0,
      loaded: false,
      expired: false,
    };
  },
  computed: {
    _seconds() {
      //一秒是一千毫秒
      return 1000;
    },
    _minutes() {
      return this._seconds * 60;
    },
    _hours() {
      return this._minutes * 60;
    },
    _days() {
      return this._hours * 24;
    },
    end() {
      return new Date(
        this.year,
        this.month,
        this.date,
        this.hour,
        this.minute,
        this.second,
        this.millisecond
      );
    },
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    //logic part
    formatNum: (num) => (num < 10 ? "0" + num : num),
    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();
        // const end = new Date(2023, 7, 10, 10, 10, 10);
        const distance = this.end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.expired = "true";
          this.loaded = "true";
          return;
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);
        this.displaySeconds = this.formatNum(seconds);
        this.displayMinutes = this.formatNum(minutes);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        this.loaded = true;
      }, 1000);
    },
  },
};
</script>

<style scoped>
.days,
.hours,
.minutes,
.seconds {
  min-width: 52px;
}
</style>