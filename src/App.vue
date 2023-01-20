<template>
  <div>
    <p>Date actuelle : {{ formattedDate }}</p>
    <select v-model="selectedTimezone" @change="updateDate">
      <option v-for="timezone in timezones" :value="timezone">{{ timezone }}</option>
    </select>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedTimezone: 'UTC',
      timezones: ['UTC', 'Europe/Paris', 'America/New_York', 'Asia/Tokyo', 'Australia/Sydney'],
      date: new Date()
    }
  },
  computed: {
    formattedDate() {
      return new Intl.DateTimeFormat('fr-FR', {
        timeZone: this.selectedTimezone,
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric',
        day: 'numeric',
        month: 'numeric',
        year: 'numeric'
      }).format(this.date);
    }
  },
  mounted() {
    this.getCurrentDate();
    this.interval = setInterval(() => {
      this.getCurrentDate();
    }, 1000);
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  methods: {
    async getCurrentDate() {
      this.date = await new Promise(resolve => setTimeout(() => {
        resolve(new Date());
      }, 0));
    },
    updateDate() {
      this.getCurrentDate();
    }
  }
}
</script>

