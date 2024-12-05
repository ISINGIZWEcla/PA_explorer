<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p> -->

<script>
    import Nav from "../components/Nav.svelte";
    import Map from "../components/Map.svelte";
    import Controls from "../components/Controls.svelte";

  
  
    let selectedDay = 0; // Default to today
    let pollutant = "All"; // Show all pollutants
  
    // Example locations for air quality monitoring
    const locations = [
    { name: "Adams", lat: 39.87, lon: -77.22 },
    { name: "Allegheny", lat: 40.47, lon: -79.98 },
    { name: "Armstrong", lat: 40.81, lon: -79.46 },
    { name: "Beaver", lat: 40.69, lon: -80.35 },
    { name: "Bedford", lat: 39.99, lon: -78.49 },
    { name: "Berks", lat: 40.41, lon: -75.93 },
    { name: "Blair", lat: 40.48, lon: -78.35 },
    { name: "Bradford", lat: 41.78, lon: -76.51 },
    { name: "Bucks", lat: 40.34, lon: -75.12 },
    { name: "Butler", lat: 40.86, lon: -79.89 },
    { name: "Cambria", lat: 40.49, lon: -78.72 },
    { name: "Cameron", lat: 41.43, lon: -78.22 },
    { name: "Carbon", lat: 40.92, lon: -75.70 },
    { name: "Centre", lat: 40.92, lon: -77.77 },
    { name: "Chester", lat: 39.97, lon: -75.75 },
    { name: "Clarion", lat: 41.20, lon: -79.38 },
    { name: "Clearfield", lat: 40.97, lon: -78.47 },
    { name: "Clinton", lat: 41.24, lon: -77.60 },
    { name: "Columbia", lat: 41.05, lon: -76.36 },
    { name: "Crawford", lat: 41.65, lon: -80.10 },
    { name: "Cumberland", lat: 40.16, lon: -77.26 },
    { name: "Dauphin", lat: 40.41, lon: -76.80 },
    { name: "Delaware", lat: 39.93, lon: -75.40 },
    { name: "Elk", lat: 41.43, lon: -78.67 },
    { name: "Erie", lat: 41.99, lon: -80.12 },
    { name: "Fayette", lat: 39.92, lon: -79.64 },
    { name: "Forest", lat: 41.50, lon: -79.22 },
    { name: "Franklin", lat: 39.93, lon: -77.72 },
    { name: "Fulton", lat: 39.93, lon: -78.12 },
    { name: "Greene", lat: 39.89, lon: -80.23 },
    { name: "Huntingdon", lat: 40.42, lon: -78.02 },
    { name: "Indiana", lat: 40.65, lon: -79.09 },
    { name: "Jefferson", lat: 41.12, lon: -79.55 },
    { name: "Juniata", lat: 40.52, lon: -77.40 },
    { name: "Lackawanna", lat: 41.44, lon: -75.63 },
    { name: "Lancaster", lat: 40.04, lon: -76.25 },
    { name: "Lawrence", lat: 41.00, lon: -80.33 },
    { name: "Lebanon", lat: 40.37, lon: -76.45 },
    { name: "Lehigh", lat: 40.60, lon: -75.55 },
    { name: "Luzerne", lat: 41.20, lon: -75.99 },
    { name: "Lycoming", lat: 41.35, lon: -77.05 },
    { name: "McKean", lat: 41.80, lon: -78.56 },
    { name: "Mercer", lat: 41.30, lon: -80.24 },
    { name: "Mifflin", lat: 40.60, lon: -77.64 },
    { name: "Monroe", lat: 41.05, lon: -75.34 },
    { name: "Montgomery", lat: 40.21, lon: -75.37 },
    { name: "Montour", lat: 41.03, lon: -76.66 },
    { name: "Northampton", lat: 40.75, lon: -75.30 },
    { name: "Northumberland", lat: 40.85, lon: -76.71 },
    { name: "Perry", lat: 40.40, lon: -77.24 },
    { name: "Philadelphia", lat: 39.95, lon: -75.17 },
    { name: "Pike", lat: 41.34, lon: -75.04 },
    { name: "Potter", lat: 41.74, lon: -77.89 },
    { name: "Schuylkill", lat: 40.69, lon: -76.20 },
    { name: "Snyder", lat: 40.76, lon: -77.06 },
    { name: "Somerset", lat: 39.97, lon: -79.03 },
    { name: "Sullivan", lat: 41.45, lon: -76.50 },
    { name: "Susquehanna", lat: 41.80, lon: -75.80 },
    { name: "Tioga", lat: 41.77, lon: -77.25 },
    { name: "Union", lat: 40.96, lon: -77.05 },
    { name: "Venango", lat: 41.43, lon: -79.74 },
    { name: "Warren", lat: 41.85, lon: -79.27 },
    { name: "Washington", lat: 40.17, lon: -80.25 },
    { name: "Wayne", lat: 41.65, lon: -75.29 },
    { name: "Westmoreland", lat: 40.31, lon: -79.47 },
    { name: "Wyoming", lat: 41.51, lon: -76.00 },
    { name: "York", lat: 39.95, lon: -76.73 }
];

  </script>
  <!-- <Nav activePage="home" />  -->
  
  
  <div class="container">
    <!-- Sidebar Navigation -->
    <div class="sidebar">
        <Nav activePage="home" />
    </div>

    <!-- Main Content -->
    <div class="main-content">
        <h1>PA Air Quality Explorer</h1>
        <!-- <p>
            This dashboard provides a map-based view of air quality in different cities.
            You can filter by pollutant type and day to explore the trends in air quality.
        </p> -->

        <!-- Map and Legend Section -->
        <main>
          <Controls bind:selectedDay bind:pollutant />
          <Map {locations} {selectedDay} {pollutant} />
        </main>
    </div>
</div>

<style>
  .container {
      display: flex;
      height: 100vh;
      margin: 0;
      padding: 0;
  }

  /* Sidebar Styles */
  .sidebar {
      width: 250px;
      background-color: #f9f7f7;
      color: white;
      /* padding: 0.5rem; */
      box-shadow: 2px 0 5px rgba(237, 232, 232, 0.895);
  }

  /* Main Content Styles */
  .main-content {
      flex: 1;
      padding-left: 2rem;
      display: flex;
      flex-direction: column;
  }

  h1 {
      margin-bottom: 1rem;
      font-size: 24px;
      font-family: Verdana, Geneva, Tahoma, sans-serif;

  }
</style>
