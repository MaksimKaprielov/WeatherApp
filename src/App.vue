<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false;
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e4920e82606c8dbdc08bd383decfc142`)
      .then(res => (this.info = res.data))

      console.log(info)
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>WeatherApp</h1>
    <p class="desc">Точный прогноз погоды в <span>{{ city == "" ? "вашем городе" : "городе " + city }}</span></p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>

  </div>
</template>

<style scoped>
.error {
  color: #fff;
  margin-top: 10px;
  font-weight: 500;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 20px 20px 40px -6px rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  padding: 20px;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.desc {
  margin-top: 5px;
}

.desc span {
  font-size: 18px;
  font-weight: 600;
}

.wrapper input {
  background-color: transparent;
  margin-top: 30px;
  border: 0;
  border-bottom: 1px solid #fff;
  color: #fff;
  font-size: 18px;
  font-weight: 600;
  padding: 5px 8px;
  outline: none;
}

.wrapper input::placeholder {
  font-size: 18px;
  font-weight: 600;
  color: #fff;
}

.wrapper input:focus {
  border-bottom-color: #c2c1c1;
}

.wrapper button {
  background-color: #e100ff;
  color: #fff;
  border-radius: 10px;
  border: 1px solid #e100ff;
  padding: 10px 15px;
  margin-left: 10px;
  cursor: pointer;
  transition: all .3s ease;

  font-size: 18px;
  font-weight: 600;
}

.wrapper button:disabled {
  background: #630070;
  border: 1px solid #630070;
  cursor: not-allowed;
}

.wrapper button:disabled:hover {
  background: #630070;
  border: 1px solid #630070;
}

.wrapper button:hover {
  background-color: transparent;
  color: #fff;
  border: 1px solid #fff;
  transition: all .3s ease;
}
</style>
