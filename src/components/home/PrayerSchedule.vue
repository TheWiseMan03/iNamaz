<template>
  <div class="mt-12">
    <div class="flex justify-center items-center">
      <h3 class="font-semibold dark:text-white">
        Choose your Region here {{ location }}
        <button class="cursor-pointer" @click.prevent="showModal = !showModal">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-caret-down-fill"
            viewBox="0 0 15 15"
          >
            <path
              d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"
            />
          </svg>
        </button>
      </h3>
    </div>

    <div class="mt-1">
      <h3 class="text-center text-slate-400 font-light">
        {{ date }}
      </h3>
    </div>

    <!-- Modal -->
    <div
      class="fixed flex justify-center items-center z-40 inset-0"
      v-if="showModal"
      @click.self="showModal = false"
    >
      <div class="fixed mx-auto w-4/5 h-1/2 max-w-sm overflow-y-auto top-36">
        <div class="bg-white mx-auto p-3 z-50 dark:bg-slate-800">
          <header class="flex justify-between">
            <span class="text-teal-500 font-semibold">List of Regions</span>
            <button
              class="rounded bg-red-400 text-white py-1 px-2 text-xs"
              @click="showModal = false"
            >
              Close
            </button>
          </header>
          <hr class="mt-2 mb-4 dark:border-teal-500" />

          <form>
            <input
              placeholder="Search Region"
              v-model="search"
              type="text"
              class="text-slate-400 border border-teal-500 rounded-sm w-full px-1 py-2 focus:outline-none focus:ring focus:ring-teal-500 bg-white placeholder:text-xs mb-3 dark:bg-slate-800"
            />
          </form>

          <div
            class="py-2 px-1 rounded hover:bg-teal-500 mb-1 cursor-pointer"
            v-for="(city, index) in filteredCities"
            :key="index"
            @click.prevent="
              getPrayerTimes(city.latitude, city.longitude);
              showModal = false;
              search = '';
              location = city.name;
            "
          >
            <p class="text-xs text-teal-500 hover:text-white font-semibold">
              {{ city.name }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="fixed z-30 opacity-25 bg-black inset-0"></div>

    <div class="mt-4 grid grid-cols-4 gap-1">
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Fajr</p>
        <p class="text-white font-semibold">{{ prayerTimes.Fajr }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Sunrise</p>
        <p class="text-white font-semibold">{{ prayerTimes.Sunrise }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Dhuhr</p>
        <p class="text-white font-semibold">{{ prayerTimes.Dhuhr }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Asr</p>
        <p class="text-white font-semibold">{{ prayerTimes.Asr }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Sunset</p>
        <p class="text-white font-semibold">{{ prayerTimes.Sunset }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Maghrib</p>
        <p class="text-white font-semibold">{{ prayerTimes.Maghrib }}</p>
      </div>
      <div
        class="
          bg-gradient-to-r
          from-emerald-700
          to-teal-500
          p-2
          text-center
          rounded-md
        "
      >
        <p class="text-white text-sm">Isha</p>
        <p class="text-white font-semibold">{{ prayerTimes.Isha }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      date: '',
      location: '',
      cities: [
  { name: 'Tashkent City', latitude: 41.3111, longitude: 69.2406},
  { name: 'Tashkent Region', latitude: 41.31 ,longitude: 69.28},
  { name: 'Andijan', latitude: 40.78, longitude: 72.34 },
  { name: 'Bukhara', latitude: 39.77, longitude: 64.42 },
  { name: 'Fergana', latitude: 40.38, longitude: 71.78 },
  { name: 'Jizzakh', latitude: 40.12, longitude: 67.83 },
  { name: 'Namangan', latitude: 41.00, longitude: 71.67 },
  { name: 'Navoiy', latitude: 43.70, longitude: 65.80 },
  { name: 'Qashqadaryo', latitude: 38.87, longitude: 66.00 },
  { name: 'Samarqand', latitude: 39.65, longitude: 66.95 },
  { name: 'Sirdaryo', latitude: 40.50, longitude: 68.78 },
  { name: 'Surxondaryo', latitude: 37.23, longitude: 67.27},
   {name:'Xorazm' , latitude: 41.55, longitude: 60.63}
],
      search: '',
      showModal: false,
      prayerTimes: {}
    }
  },
  mounted() {
    this.date = new Date().toISOString().slice(0, 10);
  },
  computed: {
    filteredCities() {
      return this.cities.filter(city =>
        city.name.toLowerCase().includes(this.search.toLowerCase())
      );
    }
  },
  methods: {
    async getPrayerTimes(latitude, longitude) {
      try {
        const response = await axios.get(`http://api.aladhan.com/v1/timings/${this.date}?latitude=${latitude}&longitude=${longitude}&method=2`);
        this.prayerTimes = response.data.data.timings;
      } catch (error) {
        console.log(error);
      }
    }
  }
}
</script>
