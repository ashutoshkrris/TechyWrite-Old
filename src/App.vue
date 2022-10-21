<script setup>
import { ref } from "vue";
import Header from "./components/Header/Header.vue";
import NoResultsFound from "./components/NoResultsFound.vue";
import ScrollToTop from "./components/ScrollToTop.vue";
import OpportunityList from "./components/OpportunityList.vue";
import Footer from "./components/Footer/Footer.vue";

const checked = ref(false);
const searchTerm = ref("");
const totalLength = ref(0);

window.addEventListener("keydown", (event) => {
  if (event.ctrlKey && event.key === "/") {
    document.querySelector("#search-bar").focus();
  }
});

function searchTermChanged(value) {
  searchTermv.value = value;
}
function updateTotalLength(value){
  totalLength.value = value;
}
function checkedChanged(value) {
  checked.value = value;
}
</script>

<template>
  <div class="bg-void min-h-screen">
    <!-- Back to top button -->
    <ScrollToTop />

    <section class="max-w-6xl p-4 mx-auto">
      <!-- Header -->
      <Header
        :totalLength="totalLength"
        :searchTerm="searchTerm"
        :checked="checked"
        @search-term="searchTermChanged"
        @checked-event="checkedChanged"
      />

      <!-- No results found section -->
      <div v-if="totalLength == 0" class="py-12 space-y-20">
        <NoResultsFound :searchTerm="searchTerm" />
      </div>

      <!-- Opportunities List -->
      <OpportunityList 
        :totalLength="totalLength"
        :search-term="searchTerm"
        :checked="checked"
        @updateTotalLength="updateTotalLength"/>

      <!-- Footer -->
      <Footer />
    </section>
  </div>
</template>

<script>
export default {
};
</script>
