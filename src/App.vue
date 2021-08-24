<template>
  <div id="app">
    <main>
      <div class="search">
        <input
          type="text"
          class="search_bar"
          placeholder="ingrese ciudad"
          v-model="Input"
          @keypress="FetchWeather"
        />
      </div>
      <div v-if="Citys.length" class="container_cards">
        <template v-for="Card in Citys">
          <div class="card" v-bind:key="Card.id">
            <button type="button" v-on:click="onClose(Card.id)" class="button">
              X
            </button>
            <div class="card_body">
              <div class="location-box">
                <div class="location">
                  {{ Card.name }},{{ Card.sys.country }}
                </div>
                <div class="date">{{ dateBuilder() }}</div>
                <div class="weather-box">
                  <div class="temp">{{ Math.round(Card.main.temp) }}º</div>
                  <div class="weather">{{ Card.weather[0].main }}</div>
                </div>
              </div>
            </div>
          </div>
        </template>
      </div>
      <p v-else class="No_hay">Ingrese una ciudad!</p>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiKey: "54ec1ec673dc55958cfec97a7a8ff36f",
      Url: "https://api.openweathermap.org/data/2.5/",
      Input: "",
      Citys: [],
    };
  },
  methods: {
    FetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.Url}weather?q=${this.Input}&appid=${this.apiKey}&units=metric`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.Citys.push(results);
    },
    onClose: function(id) {
      let filter = this.Citys.filter((card) => card.id !== id);
      this.Citys = filter;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
.search {
  text-align-last: center;
}
.No_hay {
  text-align: center;
  margin-top: 3rem;
}
.button {
  height: 20px;
  border: 1px solid;

  width: 20px;
  float: right;
  border-radius: 38px;
}
.search input {
  font-size: 20px;

  height: 40px;
  width: 20%;
  margin-top: 3rem;
  border: none;
  border-radius: 0;
  outline: none;
  border-bottom: 1px solid;
}
.container_cards {
  display: flex;
  flex-direction: row;
  place-content: center;
}
.card {
  margin: 10px;
  margin-top: 10%;
  width: fit-content;
  border-radius: 7px;
  border: 1px solid;
  background-image: url("./assets/atardecer.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.card_body {
  text-align: center;
  margin: 2rem;
}

.location {
  color: #fff;
  font-size: 26px;

  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date {
  color: #fff;
  font-size: 14px;

  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 45px;
  font-weight: 900;
  text-shadow: 3px 6px rgb(0 0 0 / 25%);
  margin: 30px 0px;
}
.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
@media (max-width: 700px) {
  .container_cards {
    flex-wrap: wrap;
  }
  .search input {
    font-size: xx-large;
    height: 82px;
    width: 80%;
  }
  .No_hay {
    font-size: unset;
  }
}
</style>
