<template>
  <div class="flex flex-col align-center justify-center text-center mt-10">
    <DateSelect @get-dates="getDateData" />
    <LineChart :chartData="testData" v-if="loaded" class="mt-10" />
  </div>
</template>

<script>
import { Chart, registerables } from "chart.js";
import { computed, defineComponent, ref } from "vue";
import { LineChart } from "vue-chart-3";
import moment from "moment";
import DateSelect from "@/components/DateSelect";

Chart.register(...registerables);

export default defineComponent({
  name: "DataChart",
  components: {
    DateSelect,
    LineChart,
  },
  data() {
    return {
      loaded: true,
    };
  },
  props: ["country"],
  setup(props) {
    const covidData = ref([]);

    const country = ref(props.country);

    const testData = computed(() => {
      const dateLabels = covidData.value.map((data) =>
        moment(data.Date).format("MM-DD-YYYY")
      );
      const confirmedCases = covidData.value.map((data) => data.Cases);

      return {
        labels: dateLabels,
        datasets: [
          {
            label: `Confirmed Cases for ${country.value.Country}`,
            data: confirmedCases,
            backgroundColor: ["#77CEFF"],
          },
        ],
      };
    });

    async function getDateData(dateData) {
      const isInvalid =
        dateData.startDate === null ||
        dateData.endDate === null ||
        dateData.startDate > dateData.endDate ||
        dateData.startDate === dateData.endDate;

      if (isInvalid) {
        alert("Please select valid start and end date");
        return;
      }

      const fromDate = dateData.startDate + "T00:00:00Z";
      const toDate = dateData.endDate + "T00:00:00Z";

      country.value = props.country;

      const res = await fetch(
        `https://api.covid19api.com/country/${country.value.Slug}/status/confirmed?from=${fromDate}&to=${toDate}`
      );

      const data = await res.json();

      covidData.value = data;
    }

    return { testData, getDateData };
  },
});
</script>
