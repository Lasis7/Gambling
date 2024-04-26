<script>
  const europeWeather = async () => {
    const response = await fetch(
      `https://api.open-meteo.com/v1/metno?latitude=59.91&longitude=10.75&current=temperature_2m,relative_humidity_2m,precipitation,rain,showers,snowfall,weather_code,wind_speed_10m&timezone=Europe%2FLondon`
    );
    if (!response.ok) {
      throw new Error('Error in fetching the data!');
    }
    return await response.json();
  };

  let weatherData = europeWeather();

  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
</script>

<div class="backdrop" />

<div class="modal">
  <div class="modal-dialog">
    <div class="modal-body">
      <header>Weather in Europe</header>
      <p>&#40;Because why not&#41;</p>
      <hr />
      {#await weatherData}
        <div class="loading">Loading....</div>
      {:then responseWeather}
        <p>Timezone: {responseWeather.timezone}</p>
        <p>Latitude: {responseWeather.latitude}</p>
        <p>Longitude: {responseWeather.longitude}</p>
        <p>Current time: {responseWeather.current.time}</p>
        <p>Temperature: {responseWeather.current.temperature_2m}</p>
        <p>
          Wind speed &#40;10m&#41;: {responseWeather.current.wind_speed_10m}
        </p>
        <p>Relative humidity: {responseWeather.current.relative_humidity_2m}</p>
        <p>Precipitation: {responseWeather.current.precipitation}</p>
        <p>Rain: {responseWeather.current.rain}</p>
        <p>Showers: {responseWeather.current.showers}</p>
        <p>Snowfalls: {responseWeather.current.snowfall}</p>
        <p>Weather code: {responseWeather.current.weather_code}</p>
      {:catch error}
        {error.message}
      {/await}
      <hr />
      <div class="closeWeather">
        <button on:click={() => dispatch('closeWeather')}>Close</button>
      </div>
    </div>
  </div>
</div>

<style>
  header {
    color: black;
    font-size: 2em;
    margin-left: 20px;
    margin-top: 10px;
  }

  p {
    color: black;
  }

  button:hover {
    color: rgb(218, 248, 46);
  }

  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 1000vh;
    background: rgba(57, 57, 57, 0.7);
    z-index: 10;
  }

  .modal {
    position: fixed;
    display: block;
    top: 200px;
    left: 305px;
    width: 60%;
    max-height: 80vh;
    background: white;
    border-radius: 50px;
    z-index: 100;
  }

  .modal-dialog {
    overflow-y: initial;
  }

  .modal-body {
    height: 400px;
    overflow-y: auto;
  }

  .closeWeather {
    margin-top: 20px;
    margin-right: 30px;
    margin-bottom: 20px;
    display: flex;
    justify-content: right;
  }

  .loading {
    color: black;
  }
</style>
