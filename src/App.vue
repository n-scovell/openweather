<template>
  <div class="mainCont">
    <section>
      <h2>{{ ci.city }}</h2>
      <p>{{ ci.longitude }}, {{ ci.latitude }}</p>
    </section>
    <section>
      <div class="left">
        <div class="imgCont">
          <img :src="getSrc(ci.ico)" />
        </div>
        <h3>{{ Math.ceil(ci.temp) }}</h3>
        <div class="fandc"><button :class="{'active' : degree == 'F'}" @click="cf(originalTemp, 'F')">°F</button><button :class="{'active' : degree == 'C'}" @click="cf(originalTemp, 'C')">°C</button></div>
        <div class="spotInfo">
          <p><strong>Humidity:</strong> {{ ci.humidity }}%</p>
          <p><strong>Wind:</strong> {{ ci.speed }} mph</p>
          <p><strong>High/Low:</strong> °{{ ci.max }}/°{{  ci.min }}</p>
        </div>
      </div>
      <div class="right">
        <div class="spotInfo">
          <p><strong>Weather</strong></p>
          <p>This will be today</p>
          <p>Skies in {{ ci.city }} look {{ ci.main }}</p>
        </div>
      </div>
    </section>
    <section>
      <div v-for="d in forecast" :key="d+'day'">
        <img :src="getSrc(d.ico)" />
        <p class="a">{{ d.myDay }}</p>
        <p class="b"><span v-if="degree=='F'">{{ getF(d.temp) }}°F</span><span v-else>{{ Math.ceil(d.temp) }}°C</span></p>
      </div>
    </section>
    <section>
      <input v-model="myCity"><button @click="runCity()">GET</button></input>
    </section>

  </div>
</template>
<script>
const API_KEY =  import.meta.env.VITE_OPEN_WEATHER_API_KEY
export default {
  data() {
    return {
      myCity: 'Orlando',
      degree:'F',
      originalTemp:null,
      loading: true,
      ci: {},
      message: '',
      intervalId: null,
      forecast: [],
    }
  },
  methods: {
    getF(a) {
      return Math.ceil(1.8*a+32)
    },
    cf(a, b) {
      this.degree = b
      this.ci.temp = b =='F' ? Math.round(1.8*a+32) : Math.ceil(a)
      return this.ci.temp
    },
    getSrc(s) {
      return `http://openweathermap.org/img/wn/${s}.png`
    },
    disDay(d) {
      let pickDay = ''
      switch(d) {
          case 0:
          pickDay = 'Sunday'
          break;
          case 1:
          pickDay = 'Monday'
          break;
          case 2:
          pickDay = 'Tuesday'
          break;
          case 3:
          pickDay = 'Wednesday'
          break;
          case 4:
          pickDay = 'Thursday'
          break;
          case 5:
          pickDay = 'Friday'
          break;
          case 6:
          pickDay = 'Saturday'
          break;
      }
      return pickDay
    },
    async fetchAPI(u) {
      const response = await fetch(u)
      if (!response.ok) throw new Error(`HTTP error! ${response.status}`)
      return await response.json()
    },
    async getCity(city, apiKey) {
      this.message = `Fetching weather from: ${city}`
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`
      try {
        const ci = await this.fetchAPI(url)
        const { lat, lon } = ci.coord
        const { feels_like, humidity, pressure, temp, temp_max, temp_min } = ci.main
        const { main, description, icon } = ci.weather[0]
        const { speed } = ci.wind
        this.originalTemp = temp
        let newTemp = this.degree == 'F' ? this.cf(temp,'F') : temp
        let newMax = this.degree == 'F' ? this.cf(temp_max,'F') : Math.ceil(temp_max)
        let newMin = this.degree == 'F' ? this.cf(temp_min,'F') : Math.ceil(temp_min)
        this.myCity = city
        this.ci = {
          city:city,
          latitude:lat, 
          longitude:lon,
          feels:feels_like,
          humidity:humidity,
          pressure:pressure,
          temp:newTemp,
          max:newMax,
          min:newMin,
          main:main,
          desc:description,
          ico:icon,
          speed:Math.ceil(speed)
        }
        const indi = await this.fetchAPI(`https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&exclude=current,minutely,hourly,alerts&appid=${API_KEY}&units=metric`)
        const wanted = "12:00:00"
        const daily = indi.list.filter(item => item.dt_txt.endsWith(wanted));
        let date = new Date()
        this.forecast = []
        for (let i = 0; i < daily.length; i++) {
          const day = daily[i]
          date = new Date(new Date().setDate(new Date().getDate() + i+1));
          let actualDay = this.disDay(date.getDay()).substring(0, 3)
          this.forecast.push({temp: day.main.temp, ico: day.weather[0].icon, myDay:actualDay })
        }
      } catch (err) {
        console.error('Failed to fetch weather:', err.message)
        this.error = err.message
        if (err.message.includes('401')) {
          alert('Invalid or missing OpenWeatherMap API key!')
        }
      } finally {
        this.loading = false
      }
    },
    runCity() {
      this.getCity(this.myCity, API_KEY)
    }
  },
  mounted() {
    this.getCity(this.myCity, API_KEY)
    // this.intervalId = setInterval(() => {
    //   console.log('RUNNING CITY')
    //   this.getCity(this.myCity, API_KEY)
    // }, 1000)
  }
}
</script>
