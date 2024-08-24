<script setup lang="ts">
// Can use this for date api
// https://timeapi.io/swagger/index.html
//https://api.openweathermap.org/data/2.5/weather?q=Toronto&appid=64e28f64b16b0f6cfeea0039ad3536da

const search = ref("Guadalajara");
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
    <div class="relative h-screen w-screen overlay mx-auto">
      <div class="flex flex-col justify-center pt-60">
        <h1 class="flex justify-center text-white text-7xl">{{ city.name }}</h1>
        <div class="flex justify-center mt-20">
          <input
            type="text"
            class="w-1/2 h-10 px-4"
            placeholder="Search a city"
            v-model="input"
          />
          <button class="bg-cyan-500 w-20 text-white h-10" @click="handleClick">
            Search
          </button>
        </div>
      </div>
      <div
        class="bg-slate absolute inset-x-0 bottom-0 w-11/12 h-1/2 rounded-t-2xl mx-auto"
      >
        <div class="flex flex-row gap-5 p-10 something">
          <div class="w-2/5 h-full bg-mist rounded-xl p-10">
            <div class="flex justify-center text-3xl">{{ city.main.temp }}</div>
            <div class="flex justify-center">
              <img
                :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
                class="w-56 icon"
              />
            </div>
          </div>
          <div class="flex flex-col gap-5 w-3/5">
            <div class="flex flex-row gap-5 h-1/2">
              <div class="w-1/3 bg-mist rounded-xl text-keylime">
                Feels like
                {{ city.main.feels_like }}
              </div>
              <div class="w-1/3 bg-mist rounded-xl">
                Temp Min {{ city.main.temp_min }}
              </div>
              <div class="w-1/3 bg-mist rounded-xl">
                Temp Max {{ city.main.temp_max }}
              </div>
            </div>
            <div class="flex flex-row gap-5 h-1/2">
              <div class="w-1/3 bg-mist rounded-xl">
                Pressure {{ city.main.pressure }}
              </div>
              <div class="w-1/3 bg-mist rounded-xl">
                Humidity {{ city.main.humidity }}
              </div>
              <div class="w-1/3 bg-mist rounded-xl">
                Sea Level{{ city.main.sea_level }}, Ground Level
                {{ city.main.grnd_level }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="h-screen relative overflow-hidden"> -->
      <!-- <img :src="background" /> -->
      <!-- <div class="absolute w-full h-full top-0 overlay"></div> -->
      <!-- <div class="w-4/5 h-1/2 bg-white">x</div> -->
      <!-- <div class="absolute w-full h-full top-0 p-48">
        <div class="flex justify-between">
          <div>
            <h1 class="text-7xl text-white">{{ city.name }}</h1>
            <p class="font-extralight text-2xl mt-2 text-white">
              Sunday Dec 4th
            </p>
            <img
              :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
              class="w-56 icon"
            />
          </div>
          <div>
            <p class="text-9xl text-white font-extralight">
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
          <button class="bg-cyan-500 w-20 text-white h-10" @click="handleClick">
            Search
          </button>
        </div>
      </div> -->
    </div>
  </div>
</template>

<style scoped>
.overlay {
  background-color: #aaa196;
}

.something {
  height: -webkit-fill-available;
}

/* .icon{
  ml-[-2.75rem] mt-[-2.75rem]
} */
</style>
