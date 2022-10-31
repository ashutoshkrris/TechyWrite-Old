<template>
  <section class="flex flex-wrap justify-center gap-4 mb-5">
    <img
      src="\images\logo.png"
      class="object-cover rounded-full p-4"
      style="width: 60%"
      alt="logo"
    />
  </section>
  <a
    class="github-corner"
    href="https://github.com/ashutoshkrris/TechyWrite"
    target="_blank"
    rel="noopener"
  >
    <img class="absolute top-0 right-0" :src="GitHubSVG" alt="Github SVG" />
  </a>

  <div class="flex flex-col md:flex-row items-center justify-between mb-6 px-3">
    <!-- sort dropdown -->
    <sorting-dropdown @sorted-change="sortChanged"></sorting-dropdown>

    <!-- Search bar -->
    <div class="flex items-center">
      <SearchBar @search-term="searchTermChanged" />
      <h2 class="text-white mr-5">{{ opportunitiesData.length }} Opportunities</h2>
    </div>
  </div>
</template>

<script>
import GitHubSVG from '../../assets/githubsvg.svg';
import SearchBar from "./components/SearchBar.vue";
import SortingDropdown from '../SortingDropdown.vue';
export default {
  name: "Header",
  props: {
    opportunitiesData: {
      type: Array,
      required: true,
    },
    searchTerm: {
      type: String,
      required: true,
    },
    checked: {
      type: Boolean,
      required: true,
    },
  },
  data(){
    return{
      GitHubSVG,
    }
  },
  methods: {
    searchTermChanged(value) {
      this.$emit("search-term", value);
    },
    checkedChanged(value) {
      this.$emit("checked-event", value);
    },
    sortChanged(value) {
      this.$emit("sort-changed", value);
    }
  },
  components: { SearchBar, SortingDropdown },
};
</script>
