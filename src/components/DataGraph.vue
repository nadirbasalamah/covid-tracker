<template>
  <DateSelect @get-dates="getDateData" />
  <div>
    <h3>{{ title }}</h3>
  </div>
  <div>
    <p>The graphs will be shown here</p>
    <line-chart v-if="loaded" :chartdata="data" />
  </div>
</template>

<script>
import DateSelect from "./DateSelect";
import LineChart from "./LineChart.vue";

export default {
  name: "DataGraph",
  components: {
    DateSelect,
    LineChart,
  },
  props: ["country"],
  data() {
    return {
      title: "Confirmed Case by Date",
      loadingImage: require("../assets/hourglass.gif"),
      data: null,
      loaded: false,
    };
  },
  methods: {
    async getDateData(dateData) {
      this.loaded = false;

      const fromDate = dateData.startDate + "T00:00:00Z";
      const toDate = dateData.endDate + "T00:00:00Z";

      const res = await fetch(
        `https://api.covid19api.com/country/${this.country.Slug}/status/confirmed?from=${fromDate}&to=${toDate}`
      );

      const covidData = await res.json();

      this.data = covidData;
      this.title = `Confirmed Cases by Date for ${this.country.Country}`;
      this.loaded = true;
    },
  },
};
</script>
