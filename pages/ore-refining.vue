<template>
  <div class="max-w-md mx-auto mt-10">
    <div class="mb-4">
      <label for="sellingPrice" class="block text-sm font-bold mb-2">
        Selling Price of an Ingot (in copper):
      </label>
      <input
        type="number"
        id="sellingPrice"
        v-model.number="sellingPrice"
        class="shadow appearance-none border rounded w-full py-2 px-3 leading-tight focus:outline-none focus:shadow-outline"
        placeholder="Enter selling price"
      />
    </div>
    <div v-if="maxPricePerOre !== null" class="mt-4">
      <p>
        Maximum price per Ore to still make a profit:
        <strong>{{ maxPricePerOre }} copper</strong>
      </p>
      <p v-if="estimatedProfit !== null">
        Estimated profit per sold ingot:
        <strong>{{ estimatedProfit }} copper</strong>
      </p>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';

const sellingPrice = ref(0);

const maxPricePerOre = computed(() => {
  const listingFeePercent = 0.05;
  const sellingFeePercent = 0.1;
  const totalFeePercent = listingFeePercent + sellingFeePercent;
  const netSellingPrice = sellingPrice.value * (1 - totalFeePercent);
  return parseFloat((netSellingPrice / 2).toFixed(2)); // Rounded to 2 decimal places for precision
});

const estimatedProfit = computed(() => {
  const costOfProduction = maxPricePerOre.value * 2;
  const netSellingPrice = sellingPrice.value * (1 - (0.05 + 0.1));
  return parseFloat((netSellingPrice - costOfProduction).toFixed(2));
});
</script>

<style scoped></style>
