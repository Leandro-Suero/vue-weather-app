<template>
  <div class="text-white mb-8">
    <div class="places-input text-gray-800">
      <input type="text" class="w-full" />
    </div>
    <div
      class="weather-container font-sans w-128 max-w-lg rounded-lg overflow-hidden bg-gray-900 shadow-lg mt-4"
    >
      <div class="current-weather flex items-center justify-between px-6 py-8">
        <div class="flex items-center">
          <div>
            <div class="text-6xl font-semibold">7ºC</div>
            <div>Feels like 3ºC</div>
          </div>
        </div>
        <div>Icon</div>
      </div>
    </div>
    <!-- end of weather-container -->
    <div class="future-weather text-sm bg-gray-800 px-6 py-8 overflow-hidden">
      <div class="flex items-center mt-8">
        <div class="w-1/6 text-lg text-gray-200">FRI</div>
        <div class="w-4/6 px-4 flex items-center">
          <div>icon</div>
          <div class="ml-3">Cloudy with a chance of showers</div>
        </div>
        <div class="w-1/6 text-right">
          <div>7ºC</div>
          <div>-1ºC</div>
        </div>
      </div>
      <div class="flex items-center mt-8">
        <div class="w-1/6 text-lg text-gray-200">SAT</div>
        <div class="w-4/6 px-4 flex items-center">
          <div>icon</div>
          <div class="ml-3">Cloudy with a chance of showers</div>
        </div>
        <div class="w-1/6 text-right">
          <div>5ºC</div>
          <div>-2ºC</div>
        </div>
      </div>
      <div class="flex items-center mt-8">
        <div class="w-1/6 text-lg text-gray-200">SUN</div>
        <div class="w-4/6 px-4 flex items-center">
          <div>icon</div>
          <div class="ml-3">Sunshine for everyone</div>
        </div>
        <div class="w-1/6 text-right">
          <div>9ºC</div>
          <div>0ºC</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    console.log("Component mounted.");
  }
};
</script>
