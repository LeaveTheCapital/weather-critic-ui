<script>
  export let name;
  import { onMount } from "svelte";
  import Raindrops from "./Raindrops.svelte";

  // blue 3789C6
  // grey #FFFFFF;

  let forecasts = [];

  onMount(async () => {
    const res = await fetch(
      `https://weather-critic-api.herokuapp.com/forecasts`
    );
    forecasts = await res.json();
  });

  const formatHour = (hour) => (hour.length > 1 ? `${hour}:00` : `0${hour}:00`);
</script>

<main>
  <h1>{name}</h1>
  <p>
    Have you ever wondered which Weather Forecast is the most accurate? We're on
    a mission to find out!
  </p>
  <div class="forecasts">
    {#each forecasts as forecast}
      <h2>Date: {forecast._id}</h2>
      <div>
        <h2>BBC Forecast</h2>
        <h3>Predicted Rainfall Hourly</h3>

        <div class="hours">
          {#each Object.keys(forecast.bbc.rainfallHourly) as hour}
            <div class="hour">
              {formatHour(hour)}: {forecast.bbc.rainfallHourly[hour].includes(
                "%"
              )
                ? forecast.bbc.rainfallHourly[hour]
                : forecast.bbc.rainfallHourly[hour] + "%"}
              <Raindrops />
            </div>
          {/each}
        </div>
        <h3>Predicted Temperature Hourly</h3>
        <div class="hours">
          {#each Object.keys(forecast.bbc.tempHourly) as hour}
            <div class="hour">
              {formatHour(hour)}: {forecast.bbc.tempHourly[hour].includes("°")
                ? forecast.bbc.tempHourly[hour]
                : forecast.bbc.tempHourly[hour] + "°"}
            </div>
          {/each}
        </div>
      </div>
      <div>
        <h2>Met Office Forecast</h2>
        <h3>Predicted Rainfall Hourly</h3>
        <div class="hours">
          {#each Object.keys(forecast.met.rainfallHourly) as hour}
            <div class="hour">
              {formatHour(hour)}: {forecast.met.rainfallHourly[hour].includes(
                "%"
              )
                ? forecast.met.rainfallHourly[hour]
                : forecast.met.rainfallHourly[hour] + "%"}
              <Raindrops />
            </div>
          {/each}
        </div>
        <h3>Predicted Temperature Hourly</h3>
        <div class="hours">
          {#each Object.keys(forecast.met.tempHourly) as hour}
            <div class="hour">
              {formatHour(hour)}: {forecast.met.tempHourly[hour].includes("°")
                ? forecast.met.tempHourly[hour]
                : forecast.met.tempHourly[hour] + "°"}
            </div>
          {/each}
        </div>
      </div>
    {:else}
      <!-- this block renders when forecasts.length === 0 -->
      <p>loading...</p>
    {/each}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  .hours {
    display: flex;
    flex-wrap: wrap;
  }

  .hour {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
