<template>
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
</template>

<script>
export default {
  name: "SearchBar",
  emits: ["search-term"],
  methods: {
    searchTermChanged(value) {
      this.$emit("search-term", value);
    },
  },
};
</script>
