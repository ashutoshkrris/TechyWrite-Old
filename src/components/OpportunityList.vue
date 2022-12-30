<template>
  <ul class="gap-4 mx-auto mb-2">
    <li
      v-for="opportunity in opportunitiesData.slice(
        (page - 1) * pageSize,
        page * pageSize
      )"
      :key="opportunity.link"
      class="p-3 w-full h-full"
    >
      <div class="shadow-lg hover:shadow-xl">
        <div
          class="flex rounded-lg h-full bg-gray-800 bg-opacity-30 p-8 flex-col hover:bg-opacity-50"
          style="position: relative"
        >
          <h2
            class="flex rounded-2xl opportunity-type text-xs font-semibold mr-4 px-2.5 py-0.5 dark:bg-green-700 dark:text-green-100 rounded-lg"
          >
            {{ opportunity.type }}
          </h2>
          <div
            class="flex items-center mb-3 justify"
            style="white-space: normal; width: 60%; word-break: break-word"
          >
            <a
              class="text-2xl font-semibold text-white"
              :href="opportunity.link"
              target="_blank"
              rel="noopener"
              >{{ opportunity.name }}</a
            >
          </div>
          <div
            class="flex justify-start mb-2 mt-4"
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
              ><h3
                class="text-white"
                style="margin-top: 3px; word-break: break-all"
              >
                {{ opportunity.contact }}
              </h3></a
            >
          </div>

          <hr
            v-if="
              (opportunity.description || opportunity.contact) &&
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
          </div>
          <div class="flex opportunity-category justify-content-end flex-wrap">
            <li v-for="i in opportunity.categories" v-bind:key="i">
              <span
                class="bg-green-900 text-green-100 text-xs font-semibold mr-2 px-2.5 py-0.5 dark:bg-green-700 dark:text-green-100 flex rounded-lg mt-2"
                >{{ i }}</span
              >
            </li>
          </div>
        </div>
      </div>
    </li>
  </ul>
  <div class="pagination-container">
    <ul v-if="totalPages >= 1" class="pagination">
      <li
        class="page-item"
        aria-label="go to previous page"
        @click="prevPage"
        :class="{
          disabled: page === 1,
        }"
      >
        <span class="page-link">&laquo;</span>
      </li>
      <li
        v-for="p in pages"
        :key="p"
        @click="gotoPage(p)"
        :aria-label="'go to page ' + p"
        class="page-item"
      >
        <div
          class="page-link"
          :class="{
            'active-page': page === p,
          }"
        >
          {{ p }}
        </div>
      </li>
      <li
        class="page-item"
        :class="{
          disabled: page === totalPages || !totalPages,
        }"
        aria-label="go to next page"
        @click="nextPage"
      >
        <div class="page-link">&raquo;</div>
      </li>
    </ul>
  </div>
</template>

<style scoped lang="scss">
.pagination-container {
  display: inline-block;
}

.pagination {
  background: rgb(31 41 55 / var(--tw-bg-opacity));
  margin: 0px;
  display: flex;
  gap: 5px;
  align-items: center;
  border: none;
  box-sizing: border-box;
  overflow: hidden;
  word-wrap: break-word;
  align-content: center;
  border-radius: 0.5rem;
}

.page-item {
  display: flex;
  cursor: pointer;
  margin-bottom: 0px;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently supported by Chrome, Edge, Opera and Firefox */
}

.page-link {
  color: white;
  border-radius: 5px;
  padding: 10px 15px;
  font-size: 14px;
  // font-weight: 800;
  &:hover {
    color: #333333;
    background-color: #e9e9e9;
    border: none;
  }
}

.active-page {
  background-color: rgb(21 128 61 / var(--tw-bg-opacity)) !important;
  color: white !important;
  &:hover {
    border: none;
  }
}

.disabled {
  .page-link {
    background-color: rgb(31 41 55 / var(--tw-bg-opacity));
  }
  cursor: not-allowed;
}
</style>

<script>
export default {
  props: {
    opportunitiesData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      page: 1,
      pageSize: 10,
      pages: [],
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.opportunitiesData.length / this.pageSize);
    },
  },
  methods: {
    prevPage() {
      if (this.page > 1) {
        this.page--;
        this.updatePages();
      }
    },
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++;
        this.updatePages();
      }
    },
    gotoPage(p) {
      this.page = p;
      this.updatePages();
    },
    updatePages() {
      this.pages = [];
      if (this.page > 3) {
        this.pages.push(1);
        this.pages.push("...");
      }
      for (
        let i = Math.max(1, this.page - 2);
        i <= Math.min(this.totalPages, this.page + 2);
        i++
      ) {
        this.pages.push(i);
      }
      if (this.page < this.totalPages - 2) {
        this.pages.push("...");
        this.pages.push(this.totalPages);
      }
    },
  },
};
</script>
