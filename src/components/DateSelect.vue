<template>
  <div class="flex flex-col align-center justify-center text-center mt-4">
    <p>Select start and end date</p>
    <form @submit="onSubmit">
      <input
        id="start-date"
        class="form-input mt-10 block w-full border border-blue-600 p-3 rounded"
        type="date"
        placeholder="select start date"
        v-model="startDate"
      />
      <input
        class="form-input mt-10 block w-full border border-blue-600 p-3 rounded"
        type="date"
        placeholder="select end date"
        v-model="endDate"
      />
      <button
        class="
          bg-blue-700
          text-white
          rounded
          p-3
          mt-10
          focus:outline-none
          hover:bg-blue-600
        "
        type="submit"
      >
        Filter by Date
      </button>
    </form>
  </div>
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
