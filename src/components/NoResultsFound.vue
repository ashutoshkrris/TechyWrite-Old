<script setup>
import cat from "../assets/cat.svg";
</script>

<template>
  <div class="w-64 md:w-[20rem] mx-auto">
    <object type="image/svg+xml" :data="cat"></object>
  </div>
  <div class="text-center space-y-4" style="margin-top: 0px">
    <p class="text-white text-2xl md:text-2xl">
      Uh Oh! We couldn't find what you are looking for
    </p>
    <p class="text-gray-300 md:text-xl">
      No search results matched your query "<strong>{{ searchTerm }}</strong
      >".
    </p>
  </div>
</template>

<script>
export default { props: { searchTerm: { type: String, required: true } } };
</script>
