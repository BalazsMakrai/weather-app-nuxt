<template>
  <div class="h-screen relative overflow-hidden">
    <img :src="background" class="w-full" />
    <div class="absolute w-full h-full top-0 overlay" />
    <div class="absolute w-full h-full top-0 p-48">
      <div class="flex justify-between">
        <div>
          <h1 class="text-7xl text-white">{{ city.name }}</h1>
          <p class="font-extralight text-2xl mt-2 text-white">Sunday Dec 9th</p>
          <img :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`" class="w-56 icon" />
        </div>
        <div>
          <p class="text-9xl text-white font-extralight">{{ Math.round(city.main.temp) }} °C</p>
        </div>
      </div>
      <div class="mt-20">
        <input type="text" class="w-1/2 h10" placeholder="Search a city..." v-model="input" />
        <button class="bg-sky-400 w-20 text-white h-10" @click="handleClick">Search</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Input } from 'postcss';

//https://api.openweathermap.org/data/2.5/weather?q=Debrecen&appid=cef17c0d00e16108b03e2dd6314f6b30
const search = ref('Toronto');
const input = ref('');
const background = ref('');
//const { data: city, error } = useFetch(() => `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=cef17c0d00e16108b03e2dd6314f6b30`);

const { data: city, error, refresh } = useAsyncData('city', async () => {
  const response = await $fetch(`https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&appid=cef17c0d00e16108b03e2dd6314f6b30`);
  const temp = response.main.temp;
  if (temp <= -10) {
    background.value = 'https://images.unsplash.com/photo-1486496146582-9ffcd0b2b2b7?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80';
  } else if (temp > -10 && temp <= 0) {
    background.value = 'https://images.unsplash.com/photo-1606231056998-f6d5bff4612a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1632&q=80';
  } else if (temp > 0 && temp <= 15) {
    background.value = 'https://images.unsplash.com/photo-1470240731273-7821a6eeb6bd?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80';
  }
  else {
    background.value = 'https://images.unsplash.com/photo-1491929007750-dce8ba76e610?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1167&q=80';
  }

  return response;
});
const handleClick = () => {
  const formattedSearh = input.value.trim().replaceAll(' ', '+');
  search.value = formattedSearh;
  input.value = '';
  refresh();
};

</script>
<style scoped>
.overlay {
  background-color: rgba(0, 0, 0, 0.5);
}

.icon {
  margin-left: -2.75rem;
  margin-top: -2.75rem;
}
</style>