<script setup>
import SearchBar from "./components/SearchBar.vue";
import GitHubSVG from "../../assets/githubsvg.svg";
</script>

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
    <!-- toggle -->
    <div class="flex items-center justify-center mb-6">
      <h2 class="text-white mr-5">Total: {{ totalLength }}</h2>
      <button
        type="button"
        aria-pressed="false"
        aria-labelledby="toggleLabel"
        class="relative inline-flex flex-shrink-0 h-6 transition-colors duration-200 ease-in-out border-2 border-transparent rounded-full cursor-pointer w-11 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        :class="checked ? 'bg-psybeam' : 'bg-gray-300'"
        @click="checkedChanged(!checked)"
      >
        <span class="sr-only">Use setting</span>
        <span
          aria-hidden="true"
          class="inline-block w-5 h-5 transition duration-200 ease-in-out transform bg-white rounded-full shadow ring-0"
          :class="checked ? 'translate-x-5' : 'translate-x-0'"
        ></span>
      </button>
      <span class="ml-3" id="toggleLabel">
        <span class="text-sm font-medium text-white">Sort alphabetically</span>
      </span>
    </div>

    <!-- Search bar -->
    <SearchBar @search-term="searchTermChanged" />
  </div>
</template>

<script>
export default {
  name: "Header",
  emits: ["search-term", "checked-event"],
  props: {
    totalLength: {
      type: Number,
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

  setup() {
    const GitHubSVG = GitHubSVG;

    return {
      GitHubSVG,
    };
  },

  methods: {
    searchTermChanged(value) {
      this.$emit("search-term", value);
    },
    checkedChanged(value) {
      this.$emit("checked-event", value);
    },
  },
};
</script>
