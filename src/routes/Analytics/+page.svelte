<script>
    import * as d3 from "d3";
    import Nav from "../../components/Nav.svelte";

    let month = "January";
    let pollutant = "aqi";
    let data = [];
    let errorMessage = "";
    let yearlyData = [];

    // Mapping months to their numerical representation for the API
    const monthMap = {
        January: 0,
        February: 1,
        March: 2,
        April: 3,
        May: 4,
        June: 5,
        July: 6,
        August: 7,
        September: 8,
        October: 9,
        November: 10,
        December: 11,
    };

    const cities = [
        { name: "Philadelphia", lat: 39.9526, lon: -75.1652 },
        { name: "Pittsburgh", lat: 40.4406, lon: -79.9959 },
        { name: "Allentown", lat: 40.6084, lon: -75.4902 },
        { name: "Erie", lat: 42.1292, lon: -80.0851 },
        { name: "Reading", lat: 40.3356, lon: -75.9269 },
        { name: "York", lat: 39.9626, lon: -76.7277 },
        { name: "Harrisburg", lat: 40.2732, lon: -76.8867 },
        { name: "Lancaster", lat: 40.0379, lon: -76.3055 },
        { name: "Bethlehem", lat: 40.6259, lon: -75.3705 },
        { name: "Scranton", lat: 41.4089, lon: -75.6624 },
    ];
    const apiKey = "6db2c3a1eeaf702a7ab3d756f7a63a15";

    async function fetchYearlyData() {
        try {
            errorMessage = "";
            yearlyData = []; // Clear previous data

            const year = new Date().getFullYear();

            // Fetch data for each month
            const promises = Object.keys(monthMap).map(async (monthName) => {
                const start = Math.floor(new Date(year, monthMap[monthName], 1).getTime() / 1000);
                const end = Math.floor(new Date(year, monthMap[monthName] + 1, 0, 23, 59, 59).getTime() / 1000);

                const cityPromises = cities.map(async (city) => {
                    const url = `https://api.openweathermap.org/data/2.5/air_pollution/history?lat=${city.lat}&lon=${city.lon}&start=${start}&end=${end}&appid=${apiKey}`;
                    const response = await fetch(url);

                    if (!response.ok) {
                        throw new Error(`Failed to fetch data for ${city.name} in ${monthName}`);
                    }

                    const result = await response.json();
                    const avgValue =
                        result.list.reduce(
                            (sum, entry) => sum + (pollutant === "aqi" ? entry.main.aqi : entry.components[pollutant] || 0),
                            0
                        ) / result.list.length;

                    return { name: city.name, month: monthName, avgValue };
                });

                const monthlyData = await Promise.all(cityPromises);
                const averageForMonth =
                    monthlyData.reduce((sum, city) => sum + city.avgValue, 0) / monthlyData.length;

                return { month: monthName, avgValue: averageForMonth };
            });

            yearlyData = await Promise.all(promises);
            console.log("Yearly data:", yearlyData);

            renderLineChart();
        } catch (error) {
            console.error("Error fetching yearly data:", error);
            errorMessage = error.message;
        }
    }

    async function fetchData() {
        try {
            errorMessage = "";
            data = []; // Clear previous data

            const now = new Date();
            const year = now.getFullYear();
            const selectedMonth = monthMap[month];

            // Start and end time for the selected month
            const start = Math.floor(new Date(year, selectedMonth, 1).getTime() / 1000);
            const end = Math.floor(new Date(year, selectedMonth + 1, 0, 23, 59, 59).getTime() / 1000);

            console.log(`Fetching data for: Start = ${start}, End = ${end}`);

            // Fetch data for each city
            const promises = cities.map(async (city) => {
                const url = `https://api.openweathermap.org/data/2.5/air_pollution/history?lat=${city.lat}&lon=${city.lon}&start=${start}&end=${end}&appid=${apiKey}`;
                console.log(`Fetching URL: ${url}`);
                const response = await fetch(url);

                if (!response.ok) {
                    throw new Error(`Failed to fetch data for ${city.name}`);
                }

                const result = await response.json();

                const avgValue =
                    result.list.reduce(
                        (sum, entry) => sum + (pollutant === "aqi" ? entry.main.aqi : entry.components[pollutant] || 0),
                        0
                    ) / result.list.length;

                return { name: city.name, avgValue };
            });

            data = await Promise.all(promises);

            // Sort data to get top 10 cities
            data = data.sort((a, b) => b.avgValue - a.avgValue).slice(0, 10);

            console.log("Top 10 data:", data);

            renderBarChart();
        } catch (error) {
            console.error("Error fetching data:", error);
            errorMessage = error.message;
        }
    }
    
    

    function renderLineChart() {
        const svg = d3.select("#line-chart").html("").append("svg");
        const margin = { top: 20, right: 30, bottom: 50, left: 60 };
        const width = 800 - margin.left - margin.right;
        const height = 400 - margin.top - margin.bottom;

        svg.attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom);

        const chart = svg.append("g").attr("transform", `translate(${margin.left}, ${margin.top})`);

        // Scales
        const x = d3.scalePoint()
            .domain(yearlyData.map((d) => d.month))
            .range([0, width]);

        const y = d3.scaleLinear()
            .domain([0, d3.max(yearlyData, (d) => d.avgValue)])
            .range([height, 0]);

        // Axes
        chart.append("g")
            .attr("transform", `translate(0, ${height})`)
            .call(d3.axisBottom(x));

        chart.append("g").call(d3.axisLeft(y));

        // Line
        const line = d3.line()
            .x((d) => x(d.month))
            .y((d) => y(d.avgValue));

        chart.append("path")
            .datum(yearlyData)
            .attr("fill", "none")
            .attr("stroke", "steelblue")
            .attr("stroke-width", 2)
            .attr("d", line);

        // Dots
        chart.selectAll(".dot")
            .data(yearlyData)
            .enter()
            .append("circle")
            .attr("cx", (d) => x(d.month))
            .attr("cy", (d) => y(d.avgValue))
            .attr("r", 4)
            .attr("fill", "steelblue");
    }

    $: if (pollutant) {
        console.log("Pollutant changed:", pollutant);
        fetchYearlyData();
    }
    

    function renderBarChart() {
        const svg = d3.select("#bar-chart").html("").append("svg");
        const margin = { top: 20, right: 30, bottom: 50, left: 60 };
        const width = 800 - margin.left - margin.right;
        const height = 400 - margin.top - margin.bottom;

        svg.attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom);

        const chart = svg.append("g").attr("transform", `translate(${margin.left}, ${margin.top})`);

        // Scales
        const x = d3.scaleBand()
            .domain(data.map((d) => d.name))
            .range([0, width])
            .padding(0.1);

        const y = d3.scaleLinear()
            .domain([0, d3.max(data, (d) => d.avgValue)])
            .range([height, 0]);

        // Axes
        chart.append("g")
            .attr("transform", `translate(0, ${height})`)
            .call(d3.axisBottom(x))
            .selectAll("text")
            .attr("transform", "rotate(-45)")
            .style("text-anchor", "end");

        chart.append("g").call(d3.axisLeft(y));

        // Bars
        chart.selectAll(".bar")
            .data(data)
            .enter()
            .append("rect")
            .attr("class", "bar")
            .attr("x", (d) => x(d.name))
            .attr("y", (d) => y(d.avgValue))
            .attr("width", x.bandwidth())
            .attr("height", (d) => height - y(d.avgValue))
            .attr("fill", "steelblue");

        // Add labels to bars
        chart.selectAll(".label")
            .data(data)
            .enter()
            .append("text")
            .attr("x", (d) => x(d.name) + x.bandwidth() / 2)
            .attr("y", (d) => y(d.avgValue) - 5)
            .attr("text-anchor", "middle")
            .text((d) => d.avgValue.toFixed(2));
    }

    // Reactive statement to trigger fetch on changes
    $: {
        console.log("Month or pollutant changed:", month, pollutant);
        fetchData();
    }

</script>

<div class="container">
    <!-- Sidebar Navigation -->
    <div class="sidebar">
        <Nav activePage="Analytics" />
    </div>

    <!-- Main Content -->
    <div class="main-content">
        <h1>PA Air Quality Trends Visualization</h1>
        <!-- <p>
            This dashboard provides a map-based view of air quality in different cities.
            You can filter by pollutant type and day to explore the trends in air quality.
        </p> -->

        <!-- Map and Legend Section -->
        <div class="filter-container">
            <div class="filter-group">
                <label for="month">Select Month:</label>
                <select id="month" bind:value={month}>
                    {#each Object.keys(monthMap) as m}
                        <option value={m}>{m}</option>
                    {/each}
                </select>
            </div>

            <div class="filter-group">
                <label for="pollutant">Select Pollutant:</label>
                <select id="pollutant" bind:value={pollutant}>
                    <option value="aqi">AQI</option>
                    <option value="pm10">PM10</option>
                    <option value="pm2_5">PM2.5</option>
                    <option value="O3">O3</option>
                    <option value="no2">NO2</option>
                    <option value="so2">SO2</option>
                    <option value="co">CO</option>

                </select>
            </div>
        </div>
        
        <h3>Top 10 Cities with the Highest AQI for the Selected Month</h3>
        <p>
            This bar chart highlights the top 10 cities in Pennsylvania with the highest Air Quality Index (AQI) or pollutant concentration 
            for the selected month. You can choose the month and pollutant to explore trends across the state.
        </p>
        <div id="bar-chart"></div>
        
        <h3>Yearly Trend of Selected Pollutant</h3>
        <p>
            This line chart shows the monthly variation in the selected pollutant for Pennsylvania. It helps visualize seasonal trends 
            in air quality and pollutant levels throughout the year.
        </p>
        <div id="line-chart"></div>
        
        {#if errorMessage}
            <p style="color: red;">{errorMessage}</p>
        {/if}
    </div>
</div>

<!-- 
<Nav activePage="Analytics" />

<div>
    <label for="month">Select Month:</label>
    <select id="month" bind:value={month}>
        {#each Object.keys(monthMap) as m}
            <option value={m}>{m}</option>
        {/each}
    </select>

    <label for="pollutant">Select Pollutant:</label>
    <select id="pollutant" bind:value={pollutant}>
        <option value="aqi">AQI</option>
        <option value="pm10">PM10</option>
        <option value="pm2_5">PM2.5</option>
        <option value="o3">Ozone (O3)</option>
        <option value="no2">Nitrogen Dioxide (NO2)</option>
    </select>
</div>

<div id="bar-chart"></div>

<div id="line-chart"></div>

{#if errorMessage}
    <p style="color: red;">{errorMessage}</p>
{/if} -->

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

    p {
        margin-bottom: 1rem;
        font-size: 16px;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
    }

    h3 {
        margin-bottom: 1rem;
        font-size: 20px;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
    }

  </style>
