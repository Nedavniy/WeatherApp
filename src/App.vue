<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null
    }
  },
  computed: {
    cityName() {
      return '«' + this.city + '»'
    },
    showTemp() {
      return 'Температура: ' + this.info.main.temp
    },
    showFeelsLike() {
      return 'Ощущается как: ' + this.info.main.feels_like
    },
    showMinTemp() {
      return 'Минимальная температура: ' + this.info.main.temp_min
    },
    showMaxTemp() {
      return 'Максимальная температура: ' + this.info.main.temp_max
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Нужно название более одного символа'
        return false
      }

      this.error = ''
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ed21b9ce95b32e02ddc22a413750a5d9`
        )
        .then((res) => (this.info = res.data))
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == '' ? 'вашем городе ' : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
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
  color: rgb(248, 82, 82);
}

.wrapper {
  padding: 20px;

  width: 900px;
  height: 500px;
  border-radius: 50px;
  text-align: center;
  color: white;
  background: rgb(38, 31, 82);
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  margin-right: 15px;
  background: transparent;
  border: 0;
  border-bottom: solid 2px gray;
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: white;
}

.wrapper button:disabled {
  background: rgb(76, 70, 109);
  cursor: not-allowed;
}
.wrapper button {
  background: rgb(56, 45, 122);
  color: white;
  border-radius: 10px;
  border: solid 2px rgb(26, 22, 53);
  padding: 10px 15px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover {
  transform: scale(1.1);
}
</style>
