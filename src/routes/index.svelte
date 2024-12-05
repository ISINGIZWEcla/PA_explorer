<script>
    import Map from "../components/Map.svelte";
    import Controls from "../components/Controls.svelte";
  
    let selectedDay = 0; // Default to today
    let pollutant = "all"; // Show all pollutants
  
    // Example locations for air quality monitoring
    const locations = [
      { name: "Pittsburgh", lat: 40.4406, lon: -79.9959 },
      { name: "Philadelphia", lat: 39.9526, lon: -75.1652 },
      { name: "Harrisburg", lat: 40.2732, lon: -76.8867 },
      { name: "Allentown", lat: 40.6084, lon: -75.4902 },
    ];
  </script>
  
  <main>
    <Controls bind:selectedDay bind:pollutant />
    <Map {locations} {selectedDay} {pollutant} />
  </main>
  