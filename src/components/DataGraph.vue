<template>
  <div>
    <p>{{ title }}</p>
    <DataChart v-if="loaded" :sampleData="data" />
  </div>
</template>

<script>
import DataChart from "./DataChart";

export default {
  name: "DataGraph",
  components: {
    DataChart,
  },
  props: ["country", "fromDate", "toDate"],
  data() {
    return {
      data: null,
      loaded: false,
      title: "Confirmed Cases",
    };
  },
  async created() {
    const res = await fetch(
      `https://api.covid19api.com/country/${this.country.Slug}/status/confirmed?from=${this.fromDate}&to=${this.toDate}`
    );

    const covidData = await res.json();

    this.data = covidData;
    this.title = `Confirmed Cases by Date for ${this.country.Country}`;
    this.loaded = true;
  },
};
</script>
