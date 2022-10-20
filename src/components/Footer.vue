<template>
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
</template>

<script>
import Github from "../assets/github.svg";
import Twitter from "../assets/twitter.svg";

export default {
  name: "Footer",
  data() {
    return {
      contributors: [],
      Twitter: Twitter,
      GitHub: Github,
    };
  },
  async mounted() {
    try {
      const response = await fetch(
        "https://api.github.com/repos/ashutoshkrris/TechyWrite/contributors"
      );
      const data = await response.json();
      this.contributors = data;
    } catch (err) {
      console.error(err.message);
    }
  },
};
</script>
