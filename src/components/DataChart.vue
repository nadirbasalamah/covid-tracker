<template>
  <LineChart :chartData="testData" />
</template>

<script>
import { Chart, registerables } from "chart.js";
import { defineComponent } from "vue";
import { LineChart } from "vue-chart-3";
import moment from "moment";

Chart.register(...registerables);

export default defineComponent({
  name: "DataChart",
  components: { LineChart },
  props: ["sampleData"],
  setup(props) {
    console.log("current: ", props.sampleData[0]);

    const dateLabels = props.sampleData.map((data) =>
      moment(data.Date).format("MM-DD-YYYY")
    );

    const confirmedCases = props.sampleData.map((data) => data.Cases);

    const testData = {
      labels: dateLabels,
      datasets: [
        {
          label: "Confirmed Cases",
          data: confirmedCases,
          backgroundColor: ["#77CEFF"],
        },
      ],
    };

    return { testData };
  },
});
</script>
