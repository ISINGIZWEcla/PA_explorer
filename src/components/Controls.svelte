<script>
    export let selectedDay;
    export let pollutant;
  
    const days = [
      { label: "Today", value: 0 },
      { label: "Yesterday", value: -1 },
      { label: "2 Days Ago", value: -2 },
      { label: "3 Days Ago", value: -3 },
      { label: "4 Days Ago", value: -4 },
      { label: "Tomorrow", value: 1 },
      { label: "2 Days Ahead", value: 2 },
      { label: "3 Days Ahead", value: 3 },
    ];
  
    const pollutants = ["All", "pm10", "pm2_5", "o3", "no2", "so2", "co"];
  </script>
  
  <!-- <div class="filter-container">

    <label for="day">Day:</label>
    <select id="day" bind:value={selectedDay}>
      {#each days as day}
        <option value={day.value}>{day.label}</option>
      {/each}
    </select>
  
    <label for="pollutant">Pollutant:</label>
    <select id="pollutant" bind:value={pollutant}>
      {#each pollutants as p}
        <option value={p}>{p.toUpperCase()}</option>
      {/each}
    </select>
  </div>
  
  <style>
    /* Set a consistent font family */
    * {
      font-family: Arial, sans-serif; /* Change to your preferred font */
    }
  
    /* Style the filter container */
    .filter-container {
      display: flex;
      gap: 0.5rem; /* Adds space between filters */
      align-items: center; /* Aligns items vertically */
      margin: 1rem 0; /* Adds some space above and below the filters */
    }
  
    /* Style the labels and selects */
    label {
      font-weight: bold; /* Optional: Make labels bold */
    }
  
    select {
      padding: 0.5rem;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem; /* Ensures readable font size */
    }
  </style> -->

  <style>
    /* Set a consistent font family */
    * {
      font-family: Arial, sans-serif; /* Change to your preferred font */
    }
  
    /* Style the filter container */
    .filter-container {
      display: flex;
      gap: 6rem; /* Adds space between the Day and Pollutant sections */
      margin: 1rem 0; /* Adds some space above and below the filters */
      align-items: center;
    }
  
    /* Style each filter group (label and select) */
    .filter-group {
      display: flex;
      flex-direction: row; /* Keeps label above the select */
      align-items: center;
      gap: 0.5rem; /* Adds space between label and dropdown */
    }
  
    /* Style the labels */
    label {
      font-weight: bold; /* Optional: Make labels bold */
      margin-right: 0.5rem; /* Adds space between label and dropdown */
      
    }
  
    /* Style the dropdowns */
    select {
      padding: 0.5rem;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem; /* Ensures readable font size */
      margin-right: 1rem; /* Adds spacing between dropdowns if necessary */
    }
  </style>
  
  <div class="filter-container">
    <div class="filter-group">
      <label for="day">Day:</label>
      <select id="day" bind:value={selectedDay}>
        {#each days as day}
          <option value={day.value}>{day.label}</option>
        {/each}
      </select>
    </div>
  
    <div class="filter-group">
      <label for="pollutant">Pollutant:</label>
      <select id="pollutant" bind:value={pollutant}>
        {#each pollutants as p}
          <option value={p}>{p.toUpperCase()}</option>
        {/each}
      </select>
    </div>
  </div>
  