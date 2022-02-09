<template>
  <form @submit="onSubmit">
    <input type="date" placeholder="select start date" v-model="startDate" />
    <input type="date" placeholder="select end date" v-model="endDate" />
    <button type="submit">Filter by Date</button>
  </form>
</template>

<script>
export default {
  name: "DateSelect",
  data() {
    return {
      startDate: null,
      endDate: null,
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      this.$emit("get-dates", {
        startDate: this.startDate,
        endDate: this.endDate,
      });
    },
  },
};
</script>
