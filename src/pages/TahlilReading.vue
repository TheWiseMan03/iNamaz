<template>
  <default-container>
    <div class="bg-gradient-to-r from-emerald-700 to-teal-500 rounded-md mb-2">
      <div class="p-3 flex items-center justify-between">
        <router-link to="/">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-5 h-14 text-white"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M10.5 19.5L3 12m0 0l7.5-7.5M3 12h18"
            />
          </svg>
        </router-link>
        <h3 class="font-sans text-white mx-auto">Tasbeeh</h3>
      </div>
    </div>

    <!-- Maximum Value -->
    <div class="text-center mt-4">
      <label for="max-value" class="block text-sm font-medium text-gray-700">Set Maximum Value</label>
      <input type="number" id="max-value" v-model.number="maxValue" @click.stop style="width: 30%; margin: auto; text-align: center;" 
      class="mt-1 block rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
    </div>

    <!-- Counter -->
    <div class="text-center text-6xl font-bold mt-4">{{ count }}</div>

    <!-- Instructions -->
    <div class="text-center mt-4">Tap anywhere to begin</div>

    <!-- Reset Button -->
    <div class="text-right mt-4 mr-4">
      <button @click="resetCount" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">Reset</button>
    </div>
  </default-container>
</template>

<script>
import DefaultContainer from "../components/DefaultContainer.vue";
import { onMounted, ref } from "vue";

export default {
  components: { DefaultContainer },
  setup() {
    const count = ref(0);
    const maxValue = ref(33);
    let clicked = false;

    const incrementCount = () => {
      if (clicked) {
        if (count.value >= maxValue.value) {
          count.value = 0;
        } else {
          count.value += 1;
        }
      } else {
        clicked = true;
      }
    };

    const resetCount = () => {
      count.value = 0;
      clicked = false;
    };

    onMounted(() => {
      document.addEventListener('click', incrementCount);
    });

    return {
      count,
      maxValue,
      resetCount,
    };
  },
};
</script>