<template>
  <main
    class="bg-gradient-to-r from-cyan-300 to-blue-300 h-screen w-screen"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16
        ? 'bg-gradient-to-r from-orange-300 to-red-300'
        : ''
    "
  >
    <div>
      <input
        class="mt-8 mx-8 md:mx-40 px-4 py-2 w-3/5 md:w-2/5 bg-slate-50 rounded-tr-lg rounded-bl-lg shadow-sky-300 shadow-md focus:outline-none text-sky-500 focus:bg-sky-100 placeholder:text-sky-300"
        type="text"
        v-model="query"
        @keypress="fetchWeather"
        placeholder="Search..."
      />
    </div>

    <div
      v-if="typeof weather.main != 'undefined'"
      class="text-center my-8 text-gray-700 font-semibold font-sans"
    >
      <div>
        <div class="text-2xl my-2 italic">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="text-lg">{{ setDate() }}</div>
      </div>
      <div
        class="bg-slate-50 rounded-md px-4 py-2 text-2xl my-4 shadow-slate-50 shadow-sm inline-block"
      >
        <div>{{ weather.main.temp }}&deg;C</div>
        <div class="italic">{{ weather.weather[0].main }}</div>
      </div>
    </div>
    <div
      class="text-center text-blue-500 my-4 text-xl font-mono"
      v-if="weather.cod == 404"
    >
      This city will be add soon.
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      api_key: "8b6aafdbe8f6f07d9e64dd2d317be3c8",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => res.json())
          .then((result) => {
            this.weather = result;
          });

        this.query = "";
      }
    },
    setDate() {
      const d = new Date();
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      const day = days[d.getDay()];
      const month = months[d.getMonth()];
      const date = d.getDate();
      const year = d.getFullYear();

      return `${day} ${date} ${month} ${year} `;
    },
  },
};
</script>
<style></style>
