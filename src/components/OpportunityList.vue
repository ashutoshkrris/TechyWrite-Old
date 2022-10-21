<template>
  <div>
    <ul class="gap-4 mx-auto mb-5">
      <li
        v-for="opportunity in opportunitiesData"
        :key="opportunity.link"
        class="p-3 w-full h-full"
      >
        <div class="shadow-lg hover:shadow-xl">
          <div
            class="flex rounded-lg h-full bg-gray-800 bg-opacity-30 p-8 flex-col hover:bg-opacity-50"
            style="position: relative"
          >
            <h2 class="flex rounded-2xl opportunity-type">
              {{ opportunity.type }}
            </h2>
            <div class="flex items-center mb-3">
              <a
                class="text-3xl font-semibold text-white"
                :href="opportunity.link"
                target="_blank"
                rel="noopener"
                >{{ opportunity.name }}</a
              >
            </div>
            <div class="flex justify-start mb-2" v-if="opportunity.description">
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
      </li>
    </ul>
    <!-- page tab -->
    <div
      class="text-white flex justify-center child:border-1 child:border-white"
    >
      <ul v-if="maxPage > 1" class="inline-flex">
        <li
          class="py-2 px-3 mx-[3px] leading-tight rounded-lg border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default"
          @click="previousPage"
        >
          Previous
        </li>
        <li
          v-if="maxPage !== 1 && maxPage > 1"
          :class="
            (currentPage === 1 ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = 1"
        >
          1
        </li>
        <li
          v-if="currentPage <= 3 || maxPage < 5"
          :class="
            (currentPage === 2 ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = 2"
        >
          2
        </li>
        <li
          v-if="currentPage <= 3 || maxPage < 5"
          :class="
            (currentPage === 3 ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = 3"
        >
          3
        </li>
        <li
          v-if="currentPage >= 4 && currentPage <= maxPage - 2"
          class="py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default"
          @click="currentPage -= 1"
        >
          {{ currentPage - 1 }}
        </li>
        <li
          v-if="currentPage >= 4 && currentPage <= maxPage - 3"
          :class="
            (currentPage !== 0 && currentPage !== maxPage - 1
              ? 'bg-transparent border-0 '
              : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
        >
          {{ currentPage }}
        </li>
        <li
          v-if="currentPage >= 3 && currentPage <= maxPage - 4"
          class="py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default"
          @click="currentPage += 1"
        >
          {{ currentPage + 1 }}
        </li>
        <li
          v-if="currentPage > maxPage - 4 && maxPage > 4"
          :class="
            (currentPage === maxPage - 2 ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = maxPage - 2"
        >
          {{ maxPage - 2 }}
        </li>
        <li
          v-if="currentPage > maxPage - 3 && maxPage > 4"
          :class="
            (currentPage === maxPage - 1 ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = maxPage - 1"
        >
          {{ maxPage - 1 }}
        </li>
        <li
          v-if="maxPage !== 3"
          :class="
            (currentPage === maxPage ? 'bg-transparent border-0 ' : '') +
            'py-2 px-3 mx-[3px] leading-tight border rounded-lg bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default'
          "
          @click="currentPage = maxPage"
        >
          {{ maxPage }}
        </li>  
        <li
          class="py-2 px-3 mx-[3px] leading-tight rounded-lg border bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white cursor-default"
          @click="nextPage"
        >
          Next
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import data from "../data.json";
import {ref,computed}from 'vue';

const props = defineProps({
  searchTerm: String,
  checked: Boolean,
});

const emit = defineEmits(['updateTotalLength'])

const originalData = [...data];
const totalLength = ref(0)
const currentPage = ref(1);
const onePageSize = 10;
const searchTerm = props.searchTerm;
const checked = props.checked;
const maxPage = ref(originalData.length / onePageSize + 1);



const opportunitiesData = computed(() => {
  // filter original data
  const filteredData = originalData.filter((data) => {
    return (
      data.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
      data.description.toLowerCase().includes(searchTerm.toLowerCase()) ||
      data.categories
        .toString()
        .toLowerCase()
        .includes(searchTerm.toLowerCase())
    );
  });

  // sort filtered data
  const sortedFilteredData = filteredData
    .slice()
    .sort((a, b) => a.name.localeCompare(b.name));

  const operationArray =
    checked ? sortedFilteredData : filteredData;
  totalLength.value = operationArray.length;
  emit('updateTotalLength',totalLength.value);

  const pageData = [];
  maxPage.value = Math.floor(operationArray.length / onePageSize);
  for (let i = 1; i <= maxPage.value; i++) {
    pageData.push(operationArray.slice((i - 1) * onePageSize, i * onePageSize));
  }
  if(operationArray.length % onePageSize !== 0){
    pageData.push(operationArray.slice(-(operationArray.length % onePageSize)));
    maxPage.value += 1;
  }
  if (currentPage.value >= maxPage.value) {
    currentPage.value = maxPage.value;
  }
  return pageData[currentPage.value - 1];
});

function nextPage() {
  if (currentPage.value === maxPage.value) {
    currentPage.value = 1;
  } else {
    currentPage.value += 1;
  }
}

function previousPage() {
  if (currentPage.value === 1) {
    currentPage.value = maxPage.value;
  } else {
    currentPage.value -= 1;
  }
}
</script>
