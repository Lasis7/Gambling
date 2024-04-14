<script>
  const europeWeather = async () => {
    const response = await fetch(
      `https://api.open-meteo.com/v1/metno?latitude=59.91&longitude=10.75&current=temperature_2m&timezone=Europe%2FLondon`
    );
    if (!response.ok) {
      throw new Error('Error in fetching the data!');
    }
    return await response.json();
  };

  let weatherData = europeWeather();
</script>

<div class="backdrop" />

<div class="modal">
  <div class="modal-dialog">
    <div class="modal-body">
      <header>Weather in Europe</header>
      <p>&#40;Because why not&#41;</p>
      <hr />
      {#await weatherData}
        <div>Loading....</div>
      {:then responseWeather}
        {#each responseWeather as weather}
          {weather.latitude}
        {/each}
      {:catch error}
        {error.message}
      {/await}
    </div>
  </div>
</div>

<style>
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

  header {
    color: black;
    font-size: 2em;
    margin-left: 20px;
    margin-top: 10px;
  }

  p {
    color: black;
  }
</style>
