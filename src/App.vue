<script>
import { computed, ref } from "vue";
import data from "./data.json";
import Twitter from "./components/Twitter.vue";
import GitHub from "./components/GitHub.vue";
import GitHubSVG from "./components/GitHubSVG.vue";

export default {
  setup() {
    const checked = ref(false);
    const originalData = [...data];
    const sortedData = data.sort((a, b) => a.name.localeCompare(b.name));
    const opportunitiesData = computed(() => {
      return checked.value ? sortedData : originalData;
    });
    return { opportunitiesData, checked };
  },
  components: { Twitter, GitHub, GitHubSVG },
};
</script>
  
<template>
  <div class="bg-void">
    <section class="max-w-6xl p-4 mx-auto">
      <!-- header  -->
      <section class="flex flex-wrap justify-center gap-4 mb-5">
        <img src="../public/image/Symbol.jpg" class="object-cover w-16 h-16 rounded-full" />
        <h1 class="mt-2 text-4xl md:text-5xl font-semibold text-center text-white">
          TechyWrite
        </h1>
      </section>
      <a class="github-corner" href="https://github.com/ashutoshkrris/hacktoberfest-swag" target="_blank"
        rel="noopener">
        <GitHubSVG />
      </a>
      <!-- toggle -->
      <div class="flex flex-col items-center justify-center mx-auto mb-6">
        <div class="flex items-center justify-center mx-auto mb-6">
          <h2 class="text-white mr-5">Total: {{ opportunitiesData.length }}</h2>
          <button type="button" aria-pressed="false" aria-labelledby="toggleLabel"
            class="relative inline-flex flex-shrink-0 h-6 transition-colors duration-200 ease-in-out border-2 border-transparent rounded-full cursor-pointer w-11 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            :class="checked ? 'bg-psybeam' : 'bg-gray-300'" @click="checked = !checked">
            <span class="sr-only">Use setting</span>
            <span aria-hidden="true"
              class="inline-block w-5 h-5 transition duration-200 ease-in-out transform bg-white rounded-full shadow ring-0"
              :class="checked ? 'translate-x-5' : 'translate-x-0'"></span>
          </button>
          <span class="ml-3" id="toggleLabel">
            <span class="text-sm font-medium text-white">Sort alphabetically</span>
          </span>
        </div>
      </div>
      <!-- list -->
      <ul class="gap-4 mx-auto mb-5">
        <li v-for="opportunity in opportunitiesData" :key="opportunity.link" class="p-3 w-full h-full">
          <a :href="opportunity.link" target="_blank" rel="noopener">
            <div class="shadow-lg hover:shadow-xl">
              <div class="flex rounded-lg h-full bg-gray-800 bg-opacity-30 p-8 flex-col hover:bg-opacity-50">
                <div class="flex items-center mb-3">
                  <a class="text-3xl font-semibold text-white" :href="opportunity.link" target="_blank"
                    rel="noopener">{{ opportunity.name }}</a>
                </div>
                <div class="flex flex-wrap justify-start mb-2">
                  <h2 class="pr-2 text-psybeam">Description:</h2>
                  <h3 class="text-white">{{ opportunity.description }}</h3>
                </div>

                <div v-if="opportunity.rate" class="flex flex-wrap justify-start mb-2">
                  <h2 class="pr-2 text-psybeam">Rate:</h2>
                  <h3 class="text-white">{{ opportunity.rate }}</h3>
                </div>
              </div>
            </div>
          </a>
        </li>
      </ul>
      <!-- footer -->
      <footer>
        <div class="px-4 py-4 mx-auto max-w-7xl sm:px-6 md:flex md:items-center md:justify-between lg:px-8">
          <div class="flex justify-center space-x-6 md:order-2">
            <a href="https://twitter.com/ashutoshkrris" class="text-gray-400 hover:text-white" target="_blank"
              rel="noopener">
              <span class="sr-only">Twitter</span>
              <Twitter />
            </a>

            <a href="https://github.com/ashutoshkrris" class="text-gray-400 hover:text-white" target="_blank"
              rel="noopener">
              <span class="sr-only">GitHub</span>
              <GitHub />
            </a>
          </div>
          <div class="mt-8 md:mt-0 md:order-1">
            <p class="text-sm text-center text-gray-400">
              &copy; {{ new Date().getFullYear() }} Made with ❤️ by Ashutosh
              Krishna
            </p>
          </div>
        </div>
      </footer>
    </section>
  </div>
</template>
  
<style>
.github-corner:hover .octo-arm {
  animation: octocat-wave 560ms ease-in-out;
}

@keyframes octocat-wave {

  0%,
  100% {
    transform: rotate(0);
  }

  20%,
  60% {
    transform: rotate(-25deg);
  }

  40%,
  80% {
    transform: rotate(10deg);
  }
}

@media (max-width: 500px) {
  .github-corner:hover .octo-arm {
    animation: none;
  }

  .github-corner .octo-arm {
    animation: octocat-wave 560ms ease-in-out;
  }
}
</style>
