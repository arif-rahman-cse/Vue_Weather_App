<template>
  <section>
    <div class="row">
      <div
        class="col-md-6"
        v-for="weatherData in weatherDatas"
        :key="weatherData.location.name">
        <div class="p-4 mb-5 weather_card">
          <div class="card-body">
            <div class="row mb-4">
              <div class="col">
                <h5>
                  <i class="fas fa-location-dot" style="color: #666666"> </i>
                  {{ weatherData.location.name }},
                  {{ weatherData.location.country }}
                </h5>
                <h6>
                  <i class="fas fa-calendar-days" style="color: #666666"> </i>
                  {{ formatCurrentDate(weatherData.location.localtime) }}
                </h6>
              </div>
              <div v-if="weatherData.current" class="col text-center">
                <img
                  :src="weatherData.current.condition.icon"
                  :alt="weatherData.current.condition.text"
                  class="fa-fade m-0"
                />
                <h6 class="small m-0">
                  {{ weatherData.current.condition.text }}
                </h6>
                <h6 class="m-0">
                  <strong>{{ weatherData.current.temp_c }} °C</strong>
                </h6>
                <p class="small m-0">
                  Humidity: {{ weatherData.current.humidity }} %
                </p>
              </div>
            </div>
            <br />
            <div class="row">
              <div
                v-for="day in weatherData.forecast.forecastday"
                :key="day.date_epoch"
                class="col-2 text-center"
              >
                <p class="small">
                  <strong>{{ getDayName(day.date) }}</strong>
                </p>
                <img
                  :src="day.day.condition.icon"
                  :alt="day.day.condition.icon"
                  class="img-fluid"
                  :class="{ 'fa-spin': day.day.condition.text === '晴れ' }"
                />
                <p class="smalls">
                  <strong>{{ day.day.avgtemp_c }}°C</strong>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    weatherDatas: Array,
  },
  methods : {
    getDayName(date) {
      const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      const dayIndex = new Date(date).getDay();
      return daysOfWeek[dayIndex];
    },
    formatCurrentDate(currDate) {
      var date = new Date(currDate);
      var options = {
        weekday: "short",
        day: "numeric",
        month: "long",
        year: "numeric",
      };
      return date.toLocaleDateString("en-US", options);
    },
  }
};
</script>

<style></style>
