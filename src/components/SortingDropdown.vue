<template>
  <div>
    <div class="text-white flex items-center">
    <button @click='toggleShow' class='text-base rounded text-white bg-green-600 border-green-600 flex items-center justify-center py-3 px-6 hover:text-white hover:bg-green-700 hover:border-green-700 hover:cursor-pointer'>
      Sort By
    </button>
    
    <span class="text-xl ml-3">{{ selectedOrderSorting }}</span>

  </div>
  <ul v-if='dropdownShow'
      class='flex flex-col text-left rounded text-gray-900 cursor-pointer text-base absolute border-2 border-solid border-gray-900'>
      <template :key="sort.label + index" v-for="(sort, index) in sorting">
        <div 
          id = "selector"
          class='py-3 px-5 z-30 hover:bg-gray-300 hover:cursor-pointer text-white hover:text-gray-900'
          :class="sort.label == selectedParamSorting && order == selectedOrderSorting ? 'bg-gray-600 text-white hover:text-gray-900': 'bg-gray-900'"
          v-for="order in sort.sorting"
          :key="order"
          @click='itemClicked(sort.label, order)'>
          {{ sort.label }} ({{ order }})
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
      selectedOrderSorting: "A-Z",
      dropdownShow: false,
      sorting: [
        { label: "Alphabetically", sorting: ["A-Z", "Z-A"]},
        { label: "Rate", sorting: ["High to Low", "Low to High"]},
      ]
    };
  },
  methods: {
    itemClicked(sort, order) {
      if (this.dropdownShow) this.toggleShow()
      this.selectedOrderSorting = order
      this.selectedParamSorting = sort
      this.$emit('sorted-change', {sort, order});
    },
    eventCallback(event) {
      if (!this.$el.contains(event.target)){
        this.dropdownShow = false
        document.body.removeEventListener("click", this.eventCallback, {capture: true})
      } 
    },
    toggleShow() {
      this.dropdownShow = !this.dropdownShow;

      if (this.dropdownShow) {
        document.body.addEventListener("click", this.eventCallback, {capture: true})
      } else {
        document.body.removeEventListener("click", this.eventCallback, {capture: true})

      }
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