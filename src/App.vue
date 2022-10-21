<script setup>
import { computed, ref } from "vue";
import data from "./data.json";
import Header from "./components/Header/Header.vue";
import NoResultsFound from "./components/NoResultsFound.vue";
import ScrollToTop from "./components/ScrollToTop.vue";
import OpportunityList from "./components/OpportunityList.vue";
import Footer from "./components/Footer/Footer.vue";

const checked = ref(false);
const originalData = [...data];
const searchTerm = ref("");

const opportunitiesData = computed(() => {
  // filter original data
  const filteredData = originalData.filter((data) => {
    return (
      data.name.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
      data.description.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
      data.categories
        .toString()
        .toLowerCase()
        .includes(searchTerm.value.toLowerCase())
    );
  });

  // sort filtered data
  const sortedFilteredData = filteredData
    .slice()
    .sort((a, b) => a.name.localeCompare(b.name));

  return checked.value ? sortedFilteredData : filteredData;
});

window.addEventListener("keydown", (event) => {
  if (event.ctrlKey && event.key === "/") {
    document.querySelector("#search-bar").focus();
  }
});
</script>

<template>
  <div class="bg-void min-h-screen">
    <!-- Back to top button -->
    <ScrollToTop />

    <section class="max-w-6xl p-4 mx-auto">
      <!-- Header -->
      <Header
        :opportunitiesData="opportunitiesData"
        :searchTerm="searchTerm"
        :checked="checked"
        @search-term="searchTermChanged"
        @checked-event="checkedChanged"
      />

      <!-- No results found section -->
      <div v-if="opportunitiesData.length == 0" class="py-12 space-y-20">
        <NoResultsFound :searchTerm="searchTerm" />
      </div>

      <!-- Opportunities List -->
      <OpportunityList :opportunitiesData="opportunitiesData" />

      <!-- Footer -->
      <Footer />
    </section>
  </div>
</template>

<script>
export default {
  methods: {
    searchTermChanged(value) {
      this.searchTerm = value;
    },
    checkedChanged(value) {
      this.checked = value;
    },
  },
};
</script>
