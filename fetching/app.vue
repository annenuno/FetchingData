<script setup lang="ts">
//https://api.openweathermap.org/data/2.5/weather?q=Toronto&appid=64e28f64b16b0f6cfeea0039ad3536da

const search = ref("Toronto");
const input = ref("");
const background = ref("");


//adding () => will refresh the fetching process
const {data: city, error} = useFetch(() => `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=64e28f64b16b0f6cfeea0039ad3536da`);

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
  const formatedSearch = input.value.trim().split(" ").join("+")
  search.value = formatedSearch;
  input.value =  "";
}


</script>


<template>
  <div>
    <div class="h-screen relative overflow-hidden">
      <img :src="background"/>
      <div class="absolute w-full h-full top-0 overlay"/>
      <div class="absolute w-full h-full top-0 p-48">

        <div class="flex justify-between">
          <div>
            <h1 class="text-7xl text-white">{{city.name}}</h1>
            <p class="font-extralight text-2xl mt-2 text-white">Sunday Dec 4th</p>
            <img :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`" class="w-56 icon" />
          </div>
          <div>
            <p class="text-9xl text-white font-extralight">
              {{city.main.temp}}°
            </p>
          </div>
        </div>

        <div class="mt-20">
          <input type="text" class="w-1/2 h-10 px-4" placeholder="Search a city" v-model="input"/>
          <button class="bg-cyan-500 w-20 text-white h-10" @click="handleClick">Search</button>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
.overlay  {
  background-color: #AAA196;
}

/* .icon{
  ml-[-2.75rem] mt-[-2.75rem]
} */
</style>