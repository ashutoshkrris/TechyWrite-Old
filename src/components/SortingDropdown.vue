<template>
  <div>
    <button @click='toggleShow' class='text-base rounded text-white bg-green-600 border-green-600 flex items-center justify-center py-3 px-6 hover:text-white hover:bg-green-700 hover:border-green-700 hover:cursor-pointer'>
      Sort By
    </button>
    <ul v-if='dropdownShow'
      class='flex flex-col text-left rounded text-gray-900 cursor-pointer text-base absolute border-2 border-solid border-gray-900'>
      <template :key="sort.label + index" v-for="(sort, index) in sorting">
        <div 
          class='py-3 px-5 z-30 hover:bg-gray-300 hover:cursor-pointer text-white hover:text-gray-900'
          :class="sort.label == selectedParamSorting && order == selectedOrderSorting ? 'bg-gray-600 text-white hover:text-gray-900': 'bg-gray-900'"
          v-for="order in sort.sorting"
          :key="order"
          @click='itemClicked(sort.label, order)'>
          {{ sort.label }} {{ order }}
        </div>
      </template>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedParamSorting: "Alphabetically",
      selectedOrderSorting: "ASC",
      dropdownShow: false,
      sorting: [
        { label: "Alphabetically", sorting: ["ASC", "DESC"]},
        { label: "Rating", sorting: ["ASC", "DESC"]},
      ]
    };
  },
  methods: {
    itemClicked(sort, order) {
      this.toggleShow()
      this.selectedOrderSorting = order
      this.selectedParamSorting = sort
      this.$emit('sorted-change', {sort, order});
    },
    toggleShow() {
      this.dropdownShow = !this.dropdownShow;
    },
  },
};
</script>

<style>
.anchor::after {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: .5em;
  vertical-align: .255em;
  content: "";
  border-top: .3em solid;
  border-right: .28em solid transparent;
  border-bottom: 0;
  border-left: .28em solid transparent;
}

</style>