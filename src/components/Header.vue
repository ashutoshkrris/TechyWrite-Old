<script setup>
import GitHubSVG from "../assets/githubsvg.svg";
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
      <h2 class="text-white mr-5">Total: {{ opportunitiesData.length }}</h2>
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
    <div class="flex items-center">
      <!-- Search icon -->
      <span class="z-10 text-gray-300 w-4 h-4 shrink-0">
        <svg
          class="fill-current"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
        >
          <path
            d="M416 208c0 45.9-14.9 88.3-40 122.7L502.6 457.4c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L330.7 376c-34.4 25.2-76.8 40-122.7 40C93.1 416 0 322.9 0 208S93.1 0 208 0S416 93.1 416 208zM208 352c79.5 0 144-64.5 144-144s-64.5-144-144-144S64 128.5 64 208s64.5 144 144 144z"
          />
        </svg>
      </span>
      <input
        type="text"
        id="search-bar"
        class="px-4 py-2 bg-gray-800 text-gray-300 outline-none rounded-full w-[16rem] -ml-8 -mr-[4.5rem] pl-12 pr-20 focus:ring focus:ring-psybeam/80"
        @input="searchTermChanged($event.target.value)"
      />
      <span
        class="z-10 text-gray-500 border border-gray-500 shrink-0 text-xs px-2 mr-4 py-1 rounded-lg select-none"
      >
        Ctrl + /
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  emits: ["search-term", "checked-event"],
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
