<template>
  <main :class="temperature >= 60 ? 'warm' : 'cold'">
    <div class="search-box">
      <input
        type="text"
        class="search-bar"
        @keyup.enter="search"
        v-model="query"
        placeholder="Search..."
      />
    </div>
    <div v-if="city != ''">
      <div class="location-box">
        <div class="location">{{ city }}, {{ country }}</div>
        <div class="date">{{ date }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ temperature }}&deg;f</div>
        <div class="weather">{{ weather }}</div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'Weather',
  created() {
    if ('geolocation' in navigator) {
      navigator.geolocation.getCurrentPosition((position) => {
        const long = position.coords.longitude;
        const lat = position.coords.latitude;

        console.log(long);

        fetch(
          `${this.api.base}weather?lat=${lat}&long=${long}&units=imperial&appid=${this.api.key}`
        )
          .then((res) => res.json())
          .then(
            (res) => (
              (this.city = res.name),
              (this.country = res.sys.country),
              ((this.temperature = Math.round(res.main.temp)),
              (this.weather = res.weather[0].main))
            )
          );
      });
    }
  },
  data() {
    return {
      api: {
        key: '67cdfbcbb33561a00ff9d8a3983bb0da',
        base: 'https://api.openweathermap.org/data/2.5/',
      },
      date: new Date().toDateString(),
      query: '',
      city: '',
      country: '',
      temperature: '',
      weather: '',
    };
  },
  methods: {
    search() {
      fetch(
        `${this.api.base}weather?q=${this.query}&units=imperial&appid=${this.api.key}`
      )
        .then((res) => res.json())
        .then(
          (res) => (
            (this.city = res.name),
            (this.country = res.sys.country),
            ((this.temperature = Math.round(res.main.temp)),
            (this.weather = res.weather[0].main))
          )
        );
    },
  },
};
</script>

<style scoped>
main {
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  display: flex;
  justify-content: center;
  width: 100%;
  margin: 0 0 75px;
}

.search-box .search-bar {
  display: block;
  width: 800px;
  max-width: 80%;
  padding: 15px;
  border: none;
  outline: none;

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 0 16px 16px;
  margin-top: -25px;

  box-shadow: 0 5px rgba(0, 0, 0, 0.2);

  color: #313131;
  font-size: 20px;

  transition: 0.4s ease;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 2px 2px rgba(50, 50, 70, 0.5);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  position: relative;
  display: inline-block;
  margin: 30px auto;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  padding: 15px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
  text-align: center;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}

.cold {
  background: linear-gradient(rgb(47, 150, 163), rgb(48, 62, 143));
  transition: 0.4s ease-out;
}

.warm {
  background: linear-gradient(rgb(143, 62, 48), rgb(163, 51, 47));
  transition: 0.4s ease-out;
}
</style>
