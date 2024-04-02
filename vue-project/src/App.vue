<template>
  <div className="weatherBox">
    <h1>Приложение погоды</h1>
    <p>
      Узнать погоду в <span>{{ city == "" ? "Вашем городе" : cityName }}</span>
    </p>
    <div className="row">
      <input
        className="inputCity"
        v-model="city"
        type="text"
        placeholder="Введите город"
      />
      <button
        v-if="city != ''"
        v-on:click="getWeather()"
        className="weatherBox_button"
      >
        Узнать погоду
      </button>
      <button disabled v-else="city == ''" className="weatherBox_button">
        введите город
      </button>
    </div>
    <h2 className="error">{{ error }}</h2>

    <div className="weatherShow" v-show="info != ''">
      <p>
        Температура : 
        <hr><span className="spanText">{{ info.temp }} ℃ </span><br />

        Чувствуется как : 
        <hr><span className="spanText">{{ info.feels_like }} ℃</span>
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: "",
    };
  },
  computed: {
    cityName() {
      return "« " + this.city + " »";
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length <= 2) {
        this.error = "Ввели мало символов!";
        return false;
      }
      this.error = "";
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=aab2729b0dea37bb8558ee1bfd4cff3e`
        )
        .then((res) => (this.info = res.data.main));
    },
  },
};
</script>

<style scoped>
.weatherBox {
  display: flex;

  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: 600px;
  height: 500px;
  border-radius: 50px;
  background-color: rgba(150, 182, 185, 0.325);
  text-align: center;
  color: white;
  -webkit-box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  -moz-box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  padding-top: 20px;
}
/* блок для выравнивания элементов ввода */
.row {
  display: flex;
}
.spanText{
  font-size: 30px;
  color:black;
  font-weight: bold;
  
}
.weatherShow {
  animation: showBox 3s;
  transition: 0.5s;
  padding-top: 50px;
  background: rgba(255, 255, 255, 0.31);
  padding: 10px;
  border-radius: 5px;
  margin-right: 30px;
  box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  color: white;
}
.weatherShow:hover{
  transform: scale(1.5);
}

.weatherBox h1 {
  font-size: 50px;
  font-family: Arial, Helvetica, sans-serif;
  margin-bottom: 20px;
}
.weatherBox p {
  margin-bottom: 20px;
}
.inputCity {
  border: none;
  background: rgba(255, 255, 255, 0.552);
  padding: 5px;
  border-radius: 5px;
  margin-right: 30px;
  box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  color: rgb(39, 31, 31);
  border-bottom: 2px solid white;
  outline: none;
}
.weatherBox_button {
  align-self: center;
  cursor: pointer;
  padding: 5px;
  border: none;
  border-radius: 5px;
  transition: 0.5s;
  background-color: rgba(255, 213, 0, 0.896);
  box-shadow: -12px 18px 40px 7px rgba(0, 0, 0, 0.38);
  border-bottom: 2px solid white;
}
.weatherBox_button:hover {
  transform: scale(1.1);
}
.error {
  margin-top: 1rem;
  color: white;
  background-color: red;
  margin-bottom: 60px;
  width: 30%;
}

@keyframes showBox {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
