<script setup lang="ts">
// Can use this for date api
// https://timeapi.io/swagger/index.html
//https://api.openweathermap.org/data/2.5/weather?q=Toronto&appid=64e28f64b16b0f6cfeea0039ad3536da

const search = ref("San Francisco");
const input = ref("");
const background = ref("");

//adding () => will refresh the fetching process
const { data: city, error } = useFetch(
  () =>
    `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=64e28f64b16b0f6cfeea0039ad3536da`
);

// const {data: city, error } = useAsyncData("city", async () => {
//   const response = await $fetch(`https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=64e28f64b16b0f6cfeea0039ad3536da`);

//   const temp = response.main.temp;

//   if (temp <= -10) {
//         background.value =
//           "https://images.unsplash.com/photo-1483664852095-d6cc6870702d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
//       } else if (temp > -10 && temp <= 0) {
//         background.value =
//           "https://images.unsplash.com/photo-1476820865390-c52aeebb9891?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3540&q=80";
//       } else if (temp > 0 && temp <= 10) {
//         background.value =
//           "https://images.unsplash.com/photo-1560258018-c7db7645254e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=4032&q=80";
//       } else {
//         background.value =
//           "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3546&q=80";
//       }

//   return response;
// }

// );

const handleClick = () => {
  // Los Angeles => Los+Angeles
  const formatedSearch = input.value.trim().split(" ").join("+");
  search.value = formatedSearch;
  input.value = "";
};
</script>

<template>
  <div>
    <div class="relative w-screen h-screen mx-auto bg-primaryWhite">
      <div class="flex flex-col justify-center pt-20 md:pt-60">
        <h1 class="flex justify-center text-primaryBlue text-7xl">
          {{ city.name }}
        </h1>
        <div class="flex justify-center mt-16 md:mt-20">
          <input
            type="text"
            class="w-1/2 h-12 px-6 text-white placeholder rounded-l-3xl bg-primaryBeige"
            placeholder="Search a city"
            v-model="input"
          />
          <button
            class="h-12 px-12 text-white bg-primaryBlue rounded-r-3xl"
            @click="handleClick"
          >
            Search
          </button>
        </div>
      </div>
      <div
        class="absolute inset-x-0 bottom-0 w-11/12 mx-auto overflow-auto bg-primaryGreen h-4/6 md:h-1/2 rounded-t-2xl"
      >
        <div class="flex flex-col gap-5 p-10 xl:flex-row something">
          <div class="p-10 h-2/5 xl:h-full xl:w-2/5 bg-primaryWhite rounded-xl">
            <div class="flex justify-center text-3xl">{{ city.main.temp }}</div>
            <!-- <div class="flex justify-center">
              <img
                :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
                class="w-56 icon"
              />
            </div> -->
          </div>
          <div class="flex flex-col gap-5 h-3/5 xl:h-full xl:w-3/5">
            <div class="flex flex-col gap-5 md:flex-row h-1/2">
              <div
                class="p-10 md:w-1/3 bg-primaryWhite rounded-xl text-keylime"
              >
                Feels like
                {{ city.main.feels_like }}
              </div>
              <div class="p-10 md:w-1/3 bg-primaryWhite rounded-xl">
                Temp Min {{ city.main.temp_min }}
              </div>
              <div class="p-10 md:w-1/3 bg-primaryWhite rounded-xl">
                Temp Max {{ city.main.temp_max }}
              </div>
            </div>
            <div class="flex flex-col gap-5 md:flex-row h-1/2">
              <div class="p-10 md:w-1/3 bg-primaryWhite rounded-xl">
                Pressure {{ city.main.pressure }}
              </div>
              <div class="p-10 md:w-1/3 bg-primaryWhite rounded-xl">
                Humidity {{ city.main.humidity }}
              </div>
              <div class="p-10 md:w-1/3 bg-primaryWhite rounded-xl">
                Sea Level{{ city.main.sea_level }}, Ground Level
                {{ city.main.grnd_level }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="relative h-screen overflow-hidden"> -->
      <!-- <img :src="background" /> -->
      <!-- <div class="absolute top-0 w-full h-full overlay"></div> -->
      <!-- <div class="w-4/5 bg-white h-1/2">x</div> -->
      <!-- <div class="absolute top-0 w-full h-full p-48">
        <div class="flex justify-between">
          <div>
            <h1 class="text-white text-7xl">{{ city.name }}</h1>
            <p class="mt-2 text-2xl text-white font-extralight">
              Sunday Dec 4th
            </p>
            <img
              :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
              class="w-56 icon"
            />
          </div>
          <div>
            <p class="text-white text-9xl font-extralight">
              {{ city.main.temp }}°
            </p>
            <div>{{ city.main }}</div>
          </div>
        </div>

        <div class="mt-20">
          <input
            type="text"
            class="w-1/2 h-10 px-4"
            placeholder="Search a city"
            v-model="input"
          />
          <button class="w-20 h-10 text-white bg-cyan-500" @click="handleClick">
            Search
          </button>
        </div>
      </div> -->
    </div>
  </div>
</template>

<style scoped>
/* .overlay {
  @apply bg-primaryBeige;
} */

.placeholder {
  input {
    placeholder {
      @apply text-white;
    }
  }
}

.something {
  height: -webkit-fill-available;
}

/* .icon{
  ml-[-2.75rem] mt-[-2.75rem]
} */
</style>
