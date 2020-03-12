<template>
  <div class="text-white mb-8">
    <div class="places-input text-gray-800">
      <div class="flex">
        <input id="address" type="text" class="w-full" />
        <a
          href="https://community.algolia.com/places/"
          title="Search by Algolia"
          target="_blank"
          rel="noopener noreferrer"
        >
          <img src="/img/algolia-blue-mark.png" alt="photo" class="w-10 ml-1" />
        </a>
      </div>
      <p>
        Selected:
        <strong id="address-value">none</strong>
      </p>
    </div>
    <div
      class="weather-container font-sans w-128 max-w-lg rounded-lg overflow-hidden bg-gray-900 shadow-lg mt-4"
    >
      <div class="current-weather flex items-center justify-between px-6 py-8">
        <div class="flex items-center">
          <div>
            <div class="text-6xl font-semibold">{{currentTemperature.actual}}ºC</div>
            <div>Feels like {{currentTemperature.feels}}ºC</div>
          </div>
          <div class="mx-5">
            <div class="font-semibold">{{currentTemperature.summary}}</div>
            <div>{{location.name}}</div>
            <a
              href="https://darksky.net/poweredby/"
              title="Powered by Dark Sky"
              target="_blank"
              rel="noopener noreferrer"
            >
              <img src="/img/poweredby-oneline-darkbackground.png" alt="photo" class="w-32" />
            </a>
          </div>
        </div>
        <div>
          <canvas ref="iconCurrent" id="iconCurrent" width="96" height="96"></canvas>
        </div>
      </div>
    </div>
    <!-- end of weather-container -->
    <div class="future-weather text-sm bg-gray-800 px-6 py-8 overflow-hidden">
      <div
        class="flex items-center"
        v-for="(day, index) in daily"
        :key="day.time"
        :class="{ 'mt-8' : index > 0 }"
      >
        <div class="w-1/6 text-lg text-gray-200">{{toDayOfWeek(day.time)}}</div>
        <div class="w-4/6 px-4 flex items-center">
          <div>
            <canvas
              :id="`icon${index+1}`"
              :data-icon="toKebabCase(day.icon)"
              height="24"
              width="24"
            ></canvas>
          </div>
          <div class="ml-3">{{ day.summary }}</div>
        </div>
        <div class="w-1/6 text-right">
          <div>{{Math.round(day.temperatureHigh)}}ºC</div>
          <div>{{Math.round(day.temperatureLow)}}ºC</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    console.log("WeatherApp Component mounted.");

    this.fetchData();

    let placesAutocomplete = places({
      appId: "plRL1LJ996UF",
      apiKey: "4e25cd8949e5d1fff4952f8755eabd44",
      container: document.querySelector("#address")
    }).configure({
      type: "city",
      aroundLatLngViaIP: false
    });

    let $address = document.querySelector("#address-value");

    placesAutocomplete.on("change", e => {
      $address.textContent = e.suggestion.value;

      this.location.name = `${e.suggestion.name}, ${e.suggestion.country}`;
      this.location.lat = e.suggestion.latlng.lat;
      this.location.lng = e.suggestion.latlng.lng;
    });

    placesAutocomplete.on("clear", () => ($address.textContent = "none"));
  },
  watch: {
    location: {
      handler(newValue, oldValue) {
        this.fetchData();
      },
      deep: true
    }
  },
  data() {
    return {
      currentTemperature: {
        actual: "",
        feels: "",
        summary: "",
        icon: ""
      },
      daily: [],
      location: {
        name: "Santa Fe, Argentina",
        lat: -31.6497208,
        lng: -60.7137262
      }
    };
  },
  methods: {
    fetchData() {
      var skycons = new Skycons({ color: "white" });

      fetch(`/api/weather?lat=${this.location.lat}&lng=${this.location.lng}`)
        .then(response => response.json())
        .then(data => {
          // console.log(data);

          this.currentTemperature.actual = Math.round(
            data.currently.temperature
          );
          this.currentTemperature.feels = Math.round(
            data.currently.apparentTemperature
          );
          this.currentTemperature.summary = data.currently.summary;
          this.currentTemperature.icon = this.toKebabCase(data.currently.icon);

          this.daily = data.daily.data;

          skycons.add("iconCurrent", this.currentTemperature.icon);
          skycons.play();

          this.$nextTick(() => {
            for (let index = 1; index <= this.daily.length; index++) {
              skycons.add(
                "icon" + index,
                document
                  .getElementById("icon" + index)
                  .getAttribute("data-icon")
              );
            }
          });
        });
    },
    toKebabCase(stringToConvert) {
      return stringToConvert.split(" ").join("-");
    },
    toDayOfWeek(timestamp) {
      const newDate = new Date(timestamp * 1000);
      const days = ["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"];

      return days[newDate.getDay()];
    }
  }
};
</script>
