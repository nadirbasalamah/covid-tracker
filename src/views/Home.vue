<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <DateSelect @get-dates="getDateData" v-if="countrySelected" />
    <DataGraph
      :country="stats"
      :fromDate="startDate"
      :toDate="endDate"
      v-if="showGraph"
    />
    <button
      @click="clearCountryData"
      v-if="stats.Country"
      class="
        bg-green-700
        text-white
        rounded
        p-3
        mt-10
        focus:outline-none
        hover:bg-green-600
      "
    >
      Clear Country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import DataTitle from "@/components/DataTitle";
import DataBoxes from "@/components/DataBoxes";
import CountrySelect from "@/components/CountrySelect";
import DateSelect from "../components/DateSelect";
import DataGraph from "../components/DataGraph";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    DateSelect,
    DataGraph,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      startDate: null,
      endDate: null,
      showGraph: false,
      countrySelected: false,
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");

      const data = await res.json();

      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
      this.countrySelected = true;
    },
    getDateData(dateData) {
      const isInvalid =
        dateData.startDate === null ||
        dateData.endDate === null ||
        dateData.startDate > dateData.endDate ||
        dateData.startDate === dateData.endDate;

      if (isInvalid) {
        alert("Please select valid start and end date");
        return;
      }

      this.startDate = dateData.startDate + "T00:00:00Z";
      this.endDate = dateData.endDate + "T00:00:00Z";
      this.showGraph = true;
    },
    async clearCountryData() {
      this.loading = true;

      const data = await this.fetchCovidData();

      this.title = "Global";
      this.stats = data.Global;
      this.loading = false;
      this.showGraph = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>
