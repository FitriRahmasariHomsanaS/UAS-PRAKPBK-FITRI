<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="bg-grey">
      <q-toolbar>
        <q-avatar>
          <img src="/src/assets/logo.png" alt="Logo" />
        </q-avatar>

        <q-toolbar-title class="text-white">WEATHER</q-toolbar-title>
        <div class="current-date text-white">{{ currentDate }}</div>
        <div class="current-time text-white">{{ currentTime }}</div>

        <q-btn-group>
          <q-btn label="Tugas Fitri Rahmasari Homsana S" color="black" class="bg-black">
            <q-menu auto-close>
              <q-list style="min-width: 200px">
                <q-item style="color:black"
                  to="/tugas/1"
                  href="https://fitri-projectcv.netlify.app/"
                  >Tugas Pertemuan 1</q-item>
                <q-item style="color:black"
                  to="/tugas/2"
                  href="https://t2-pbk-fitri.vercel.app/"
                  >Tugas Pertemuan 2</q-item>
                <q-item style="color:black"
                  to="/tugas/3"
                  href="https://t3-pbk-fitri-esz2.vercel.app/"
                  >Tugas Pertemuan 3</q-item>
                <q-item style="color:black"
                  to="/tugas/4"
                  href="https://t4-pbk-fitri.vercel.app/"
                  >Tugas Pertemuan 4</q-item>
                <q-item style="color:black"
                  to="/tugas/5"
                  href="https://t5-pbk-fitrir.vercel.app/"
                  >Tugas Pertemuan 5</q-item>
                <q-item style="color:black"
                  to="/tugas/6"
                  href="https://t6-pbk-fitri-rhs.vercel.app/"
                  >Tugas Pertemuan 6</q-item>
              </q-list>
            </q-menu>
          </q-btn>
        </q-btn-group>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page class="flex flex-center">
        <q-card class="q-pa-md glass-card">
          <q-card-section>
            <div class="text-h5 text-center text-purple text-primary">
              <strong>PERKIRAAN CUACA</strong>
            </div>
          </q-card-section>

          <q-card-section>
            <q-input
              v-model="location"
              outlined
              placeholder="Masukkan lokasi"
              class="input-field"
              @keyup.enter="fetchWeather"
            >
              <template v-slot:append>
                <q-btn label="Cari" color="purple" @click="fetchWeather" />
              </template>
            </q-input>
          </q-card-section>

          <q-card-section v-if="weather" class="weather-info">
            <div class="text-center text-black">
              <q-icon name="place" class="q-mr-sm" />
              <strong>Lokasi:</strong> {{ weather.name }}
            </div>
            <div :class="temperatureClass" class="text-center">
              <q-icon name="thermostat" class="q-mr-sm" />
              <strong>Temperatur:</strong> {{ weather.main.temp }}°C
            </div>
            <div class="text-center text-black">
              <q-icon name="cloud" class="q-mr-sm" />
              <strong>Deskripsi:</strong> {{ weather.weather[0].description }}
            </div>
          </q-card-section>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref, watch, onMounted } from "vue";
import axios from "axios";

export default {
  name: "App",
  setup() {
    const location = ref("");
    const weather = ref(null);
    const apiKey = "a93c10b59b0e334f6ba34e02cf783d90";
    const currentTime = ref(new Date().toLocaleTimeString());
    const currentDate = ref(new Date().toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }));

    const fetchWeather = async () => {
      if (location.value) {
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${location.value}&appid=${apiKey}&units=metric`;
        try {
          const response = await axios.get(apiUrl);
          weather.value = response.data;
          console.log(weather.value); 
        } catch (error) {
          console.error("Error fetching the weather data:", error);
        }
      }
    };

    const temperatureClass = ref("");

    watch(weather, (newVal) => {
      if (newVal && newVal.main.temp) {
        const temp = newVal.main.temp;
        if (temp > 30) {
          temperatureClass.value = "text-red";
        } else if (temp >= 25 && temp <= 30) {
          temperatureClass.value = "text-purple";
        } else {
          temperatureClass.value = "text-green";
        }
      }
    });

    const updateTime = () => {
      currentTime.value = new Date().toLocaleTimeString();
      currentDate.value = new Date().toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
    };
    setInterval(updateTime, 1000);

    onMounted(() => {
      fetchWeather();
    });

    return {
      location,
      weather,
      fetchWeather,
      temperatureClass,
      currentTime,
      currentDate,
    };
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');

.text-red {
  color: #e57373;
}

.text-purple {
  color: #ff3bef;
}

.text-green {
  color: #81c784;
}

body {
  font-family: 'Lato', sans-serif;
  background: url('src/assets/background.jpg') no-repeat center center fixed;
  background-size: cover;
  color: #ffffff;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.bg-gradient {
  background: linear-gradient(90deg, #00c6ff, #0072ff);
}

.glass-card {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 15px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  border: 1px solid rgba(255, 255, 255, 0.18);
  transition: transform 0.2s, box-shadow 0.2s;
  padding: 20px;
  margin: 20px;
  max-width: 600px;
  width: 100%;
}

.glass-card:hover {
  transform: scale(1.05);
  box-shadow: 0 12px 36px 0 rgba(31, 38, 135, 0.5);
}

.text-white {
  color: rgb(255, 255, 255);
}

.text-primary {
  color: #00bcd4;
}

.input-field .q-field__control {
  background: rgba(218, 218, 218, 0.2);
  border-radius: 5px;
  color: white;
}

.input-field .q-field__control:before,
.input-field .q-field__control:after {
  border-color: rgba(255, 255, 255, 0.3);
}

.input-field .q-input__inner {
  color: white;
}

.weather-info {
  margin-top: 20px;
  animation: fadeIn 1s ease-in-out;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
  border-radius: 10px;
  padding: 15px;
}

.current-time,
.current-date {
  margin-left: auto;
  padding-right: 20px;
  font-weight: 700;
}

.bg-black .q-menu {
  background-color: black;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
