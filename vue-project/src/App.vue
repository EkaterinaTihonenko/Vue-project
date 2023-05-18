<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showSunrise() {
      let sunrise = this.info.sys.sunrise;
      let date = new Date(sunrise * 1000);
      var h = date.getHours();
      var m = date.getMinutes();
      var s = date.getSeconds();
      return "Восход: " + h + ":" + m + ":" + s;
    },
    showSunset() {
      let sunset = this.info.sys.sunset;
      let date = new Date(sunset * 1000);
      var h = date.getHours();
      var m = date.getMinutes();
      var s = date.getSeconds();
      return "Заход: " + h + ":" + m + ":" + s;
    },
    showTemp() {
      return "Температура: " + this.info.main.temp + "°";
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like + "°";
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min + "°";
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max + "°";
    },
    showPressure() {
      return "Давление: " + this.info.main.pressure + " мм";
    },
    showHumidity() {
      return "Влажность: " + this.info.main.humidity + " %";
    },
    showSpeed() {
      return "Ветер: " + this.info.wind.speed + " м/с";
    },
    newDate() {
      let today = new Date();
      let dd = String(today.getDate()).padStart(2, "0");
      let mm = String(today.getMonth() + 1).padStart(2, "0");
      let yyyy = today.getFullYear();

      today = dd + "." + mm + "." + yyyy + "г.";
      return today;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа!";
        return false;
      }
      this.error = "";
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=021f56dd29bfec5d2f23593a6461f304`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <video
      autoplay
      loop
      muted
      class="bgvideo"
      src="./assets/video/bg.mp4"
    ></video>
    <div class="wrapper-content">
      <div class="content">
        <h1>
          Прогноз погоды
          <br />
          на {{ newDate }}
        </h1>
        <p>
          Погода в городе
          {{ city == "" ? "?" : cityName }}
        </p>
        <div class="block-btn">
          <input type="text" v-model="city" placeholder="Введите город..." />
          <button v-if="city != ''" @click="getWeather()" type="button">
            Получить погоду
          </button>
          <button disabled v-else type="button">Введите название города</button>
        </div>
        <p class="error">{{ error }}</p>
      </div>
      <div class="show-btm">
        <div v-if="info != null" class="block-btm">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/horizon.svg"
            alt="img"
          />
          <p>{{ showSunrise }}</p>
        </div>
        <div v-if="info != null" class="block-btm">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/starry-night.svg"
            alt="img"
          />
          <p>{{ showSunset }}</p>
        </div>
      </div>
      <div v-if="info != null">
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/thermometer-celsius.svg"
            alt="img"
          />
          <p>{{ showTemp }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/thermometer.svg"
            alt="img"
          />
          <p>{{ showFeelsLike }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/thermometer-colder.svg"
            alt="img"
          />
          <p>{{ showMinTemp }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/thermometer-warmer.svg"
            alt="img"
          />
          <p>{{ showMaxTemp }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/pressure-high.svg"
            alt="img"
          />
          <p>{{ showPressure }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/raindrops.svg"
            alt="img"
          />
          <p>{{ showHumidity }}</p>
        </div>
        <div class="show-block">
          <img
            class="icon"
            src="https://basmilius.github.io/weather-icons/production/fill/all/wind.svg"
            alt="img"
          />
          <p>{{ showSpeed }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  height: 100vh;
  width: 100%;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.bgvideo {
  height: 100vh;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  object-fit: cover;
  z-index: 1;
}

.wrapper-content {
  z-index: 2;
}
.content {
  border-radius: 50px;
  background-color: #19cdfad8;
  border-bottom: 2px solid #ffffff;
  border-top: 2px solid #ffffff;
  color: rgb(0, 0, 0);
  padding: 20px;
  box-shadow: 1px 2px 8px 6px rgba(0, 0, 0, 0.36) inset;
}

.wrapper h1 {
  margin-top: 30px;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  position: relative;
}

.wrapper p {
  font-family: "Times New Roman", Times, serif;
  font-size: 1.3rem;
  font-weight: 500;
}

.wrapper input {
  margin-top: 15px;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.592);
  border-radius: 5px;
  color: rgba(0, 0, 0, 0.932);
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border: 1px solid rgb(255, 255, 255);
}

.wrapper button {
  background: #38373aab;
  box-shadow: 1px 1px 10px #ffffff;
  border-radius: 10px;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
  color: aliceblue;
  width: 150px;
}

.wrapper button:disabled {
  cursor: not-allowed;
  background: #38373aab;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: rgb(255, 0, 0);
}

.show-btm {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.block-btm {
  display: flex;
  align-items: center;
  box-shadow: 1px 2px 8px 6px rgba(0, 0, 0, 0.36) inset;
  border-radius: 20px;
  border: 1px solid #ffffff;
  padding: 0 1rem;
  margin: 0.2rem;
}

.show-block {
  background-color: #19cdfad8;
  box-shadow: 1px 2px 8px 6px rgba(0, 0, 0, 0.36) inset;
  border-radius: 20px;
  border: 1px solid #ffffff;
  display: flex;
  align-items: center;
  min-width: 400px;
  margin: 0.3rem 0;
}

.icon {
  width: 50px;
  height: 50px;
  margin: 0 0.5rem;
  filter: drop-shadow(1px 0 0.1rem #ffffff);
}
@media (max-width: 1024px) {
  .wrapper {
    flex-direction: column;
  }
}

@media (max-width: 850px) {
  .content {
    width: 380px;
    height: 220px;
  }

  .block-btn {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .block-btn button {
    margin: 1rem 0;
  }

  .wrapper h1 {
    margin-top: 40px;
    font-size: 1.5rem;
  }

  .wrapper p {
    font-family: "Times New Roman", Times, serif;
    font-size: 1rem;
  }

  @media (max-width: 425px) {
    .content {
      width: 360px;
      height: 220px;
    }
  }
}
</style>
