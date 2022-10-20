<script setup>
import { computed, ref } from "vue";
import data from "./data.json";
import Twitter from "./assets/twitter.svg";
import GitHub from "./assets/github.svg";
import GitHubSVG from "./assets/githubsvg.svg";
import NoResultsFound from "./components/404.vue";

const checked = ref(false);
const originalData = [...data];
const searchTerm = ref("");
const currentPage = ref(0);
const onePageSize = 10;
const maxPage = ref(originalData.length / onePageSize + 1);
const totalLength = ref(0);

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
  const operationArray =
    searchTerm.value !== "" ? sortedFilteredData : filteredData;
  totalLength.value = operationArray.length;

  const pageData = [];
  maxPage.value = Math.floor(operationArray.length / onePageSize);
  for (let i = 1; i <= maxPage.value; i++) {
    pageData.push(operationArray.slice((i - 1) * onePageSize, i * onePageSize));
  }
  pageData.push(operationArray.slice(-(operationArray.length % onePageSize)));
  maxPage.value += 1;
  if (currentPage.value >= maxPage) {
    currentPage.value = maxPage - 1;
  }
  return pageData[currentPage.value];
});

function nextPage() {
  if (currentPage.value === maxPage.value - 1) {
    currentPage.value = 0;
  } else {
    currentPage.value += 1;
  }
}

function previousPage() {
  if (currentPage.value === 0) {
    currentPage.value = maxPage.value - 1;
  } else {
    currentPage.value -= 1;
  }
}

const contributors = ref([]);

// this function calls github api for fetching repo contributors
const fetchContributors = async () => {
  try {
    const response = await fetch(
      "https://api.github.com/repos/ashutoshkrris/TechyWrite/contributors"
    );
    const data = await response.json();
    contributors.value = data;
  } catch (err) {
    console.log(err.message);
  }
};
fetchContributors();

window.addEventListener("keydown", (event) => {
  if (event.ctrlKey && event.key === "/") {
    document.querySelector("#search-bar").focus();
  }
});
</script>

<template>
  <div class="bg-void min-h-screen">
    <section class="max-w-6xl p-4 mx-auto">
      <!-- header  -->
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

      <div
        class="flex flex-col md:flex-row items-center justify-between mb-6 px-3"
      >
        <!-- toggle -->
        <div class="flex items-center justify-center mb-6">
          <h2 class="text-white mr-5">Total: {{ totalLength }}</h2>
          <button
            type="button"
            aria-pressed="false"
            aria-labelledby="toggleLabel"
            class="relative inline-flex flex-shrink-0 h-6 transition-colors duration-200 ease-in-out border-2 border-transparent rounded-full cursor-pointer w-11 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            :class="checked ? 'bg-psybeam' : 'bg-gray-300'"
            @click="checked = !checked"
          >
            <span class="sr-only">Use setting</span>
            <span
              aria-hidden="true"
              class="inline-block w-5 h-5 transition duration-200 ease-in-out transform bg-white rounded-full shadow ring-0"
              :class="checked ? 'translate-x-5' : 'translate-x-0'"
            ></span>
          </button>
          <span class="ml-3" id="toggleLabel">
            <span class="text-sm font-medium text-white"
              >Sort alphabetically</span
            >
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
            @input="searchTerm = $event.target.value"
          />
          <span
            class="z-10 text-gray-500 border border-gray-500 shrink-0 text-xs px-2 mr-4 py-1 rounded-lg select-none"
          >
            Ctrl + /
          </span>
        </div>
      </div>

      <!-- No results found section -->
      <div v-if="opportunitiesData.length == 0" class="py-12 space-y-20">
        <div class="w-64 md:w-[20rem] mx-auto">
          <NoResultsFound />
        </div>
        <div class="text-center space-y-4">
          <p class="text-white text-2xl md:text-2xl">
            Uh Oh! We couldn't find what you are looking for
          </p>
          <p class="text-gray-300 md:text-xl">
            No search results matched your query "<strong>{{
              searchTerm
            }}</strong
            >".
          </p>
        </div>
      </div>
      <!--page tab-->
      <div
        class="text-white flex justify-center child:border-1 child:border-white"
      >
        <ul class="inline-flex -space-x-px">
          <li
            class="py-2 px-3 ml-0 leading-tight rounded-l-lg border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white"
            @click="previousPage"
          >
            Previous
          </li>
          <li
            :class="
              (currentPage === 0 ? 'bg-gray-700 text-white ' : '') +
              'py-2 px-3 ml-0 leading-tight border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white'
            "
            @click="currentPage = 0"
          >
            {{1}}
          </li>
          <li
            v-if="(currentPage !== 0 && currentPage !== maxPage - 1)"
            :class="
              (currentPage !== 0 && currentPage !== maxPage - 1 ? 'bg-gray-700 text-white ' : '') +
              'py-2 px-3 ml-0 leading-tight border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white'
            "
          >
            {{ currentPage + 1}}
          </li>
          <li
            :class="
              (currentPage === maxPage - 1 ? 'bg-gray-700 text-white ' : '') +
              'py-2 px-3 ml-0 leading-tight border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white'
            "
            @click="currentPage = maxPage - 1"
          >
            {{ maxPage}}
          </li>
          <li
            class="py-2 px-3 ml-0 leading-tight rounded-r-lg border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white"
            @click="nextPage"
          >
            Next
          </li>
        </ul>
      </div>
      <!-- list -->
      <ul class="gap-4 mx-auto mb-5">
        <li
          v-for="opportunity in opportunitiesData"
          :key="opportunity.link"
          class="p-3 w-full h-full"
        >
          <a :href="opportunity.link" target="_blank" rel="noopener">
            <div class="shadow-lg hover:shadow-xl">
              <div
                class="flex rounded-lg h-full bg-gray-800 bg-opacity-30 p-8 flex-col hover:bg-opacity-50"
                style="position: relative"
              >
                <h2 class="flex rounded-2xl opportunity-type">
                  {{ opportunity.type }}
                </h2>
                <div class="flex items-center mb-3">
                  <p class="opportunity-name font-semibold text-white">
                    {{ opportunity.name }}
                  </p>
                </div>
                <div
                  class="flex justify-start mb-2"
                  v-if="opportunity.description"
                >
                  <em class="bi bi-info-circle me-3 icon"></em>
                  <h3 class="text-white" style="margin-top: 3px">
                    {{ opportunity.description }}
                  </h3>
                </div>

                <div class="flex justify-start mb-2" v-if="opportunity.contact">
                  <em class="bi bi-envelope me-3 icon"></em>
                  <a :href="'mailto:' + opportunity.contact"
                    ><h3 class="text-white" style="margin-top: 3px">
                      {{ opportunity.contact }}
                    </h3></a
                  >
                </div>

                <hr
                  v-if="
                    opportunity.description &&
                    (opportunity.maxRate ||
                      opportunity.hourlyMaxRate ||
                      opportunity.royaltyRate)
                  "
                  style="margin-bottom: 5px"
                />

                <div
                  v-if="
                    opportunity.maxRate ||
                    opportunity.hourlyMaxRate ||
                    opportunity.royaltyRate
                  "
                  class="flex justify-start mb-2"
                >
                  <em class="bi bi-credit-card icon"></em>
                  <h3
                    class="text-white"
                    style="margin-top: 3px"
                    v-if="opportunity.minRate && opportunity.maxRate"
                  >
                    ${{ opportunity.minRate }} - ${{ opportunity.maxRate }}
                  </h3>
                  <h3
                    class="text-white"
                    style="margin-top: 3px"
                    v-if="!opportunity.minRate && opportunity.maxRate"
                  >
                    ${{ opportunity.maxRate }}
                  </h3>
                  <h3
                    class="text-white"
                    style="margin-top: 3px"
                    v-if="opportunity.hourlyMaxRate"
                  >
                    ${{ opportunity.hourlyMaxRate }} per hour
                  </h3>
                  <h3
                    class="text-white"
                    style="margin-top: 3px"
                    v-if="opportunity.royaltyRate"
                  >
                    {{ opportunity.royaltyRate }}
                  </h3>
                  <div
                    class="flex flex-row"
                    style="position: absolute; right: 10px; bottom: 10px"
                  >
                    <li v-for="i in opportunity.categories" v-bind:key="i">
                      <span
                        class="bg-green-900 text-green-100 text-xs font-semibold mr-2 px-2.5 py-0.5 dark:bg-green-700 dark:text-green-100 flex rounded-lg"
                        >{{ i }}</span
                      >
                    </li>
                  </div>
                </div>
              </div>
            </div>
          </a>
        </li>
      </ul>
      <!-- footer -->
      <footer>
        <div class="px-4 py-4 mx-auto max-w-7xl sm:px-6 lg:px-8 space-y-8">
          <!-- Mention contributors -->
          <div
            class="text-center border-2 border-gray-500 rounded-xl px-2 md:px-6 py-4 space-y-6"
          >
            <p class="text-gray-200 text-lg font-medium">Our Contributors</p>
            <ul class="flex flex-wrap items-center justify-center">
              <li v-for="contributor in contributors" :key="contributor.id">
                <div class="flex flex-col items-center px-2 md:px-6 py-4">
                  <img
                    :src="contributor.avatar_url"
                    :alt="contributor.login"
                    class="rounded-full w-8 h-8"
                  />

                  <div class="flex flex-col items-center space-y-1">
                    <a
                      :href="contributor.html_url"
                      target="_blank"
                      rel="noopener"
                      class="text-psybeam hover:text-psybeam/80"
                    >
                      {{ contributor.login }}
                    </a>
                    <span
                      class="text-gray-200 bg-gray-800 text-sm rounded-full px-2"
                    >
                      {{ contributor.contributions }}
                    </span>
                  </div>
                </div>
              </li>
            </ul>
          </div>

          <div class="md:flex md:items-center md:justify-between">
            <div class="flex justify-center space-x-6 md:order-2">
              <a
                href="https://twitter.com/ashutoshkrris"
                class="text-gray-400 hover:text-white"
                target="_blank"
                rel="noopener"
              >
                <span class="sr-only">Twitter</span>
                <img class="w-[30px]" :src="Twitter" alt="Twitter logo" />
              </a>

              <a
                href="https://github.com/ashutoshkrris"
                class="text-gray-400 hover:text-white"
                target="_blank"
                rel="noopener"
              >
                <span class="sr-only">GitHub</span>
                <img :src="GitHub" alt="github" />
              </a>
            </div>
            <div class="mt-8 md:mt-0 md:order-1">
              <p class="text-sm text-center text-gray-400">
                &copy; {{ new Date().getFullYear() }} Made with ❤️ by Ashutosh
                Krishna
              </p>
            </div>
          </div>
        </div>
      </footer>
    </section>
  </div>
</template>
