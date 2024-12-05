<script>
    import { onMount } from "svelte";
    import mapboxgl from "mapbox-gl";
  
    export let locations = [];
    export let selectedDay = 0;
    export let pollutant = "AQI";
  
    let map;
    let markers = [];
  
    mapboxgl.accessToken = "pk.eyJ1IjoiY2xhcmlzc2U0NSIsImEiOiJjbTJuZ2xxOHYwNjBhMmlwdHJ0aTIzNDY0In0.XSSjbq9ozdSSlFj8Pkue3g";
  
    onMount(() => {
        map = new mapboxgl.Map({
            container: "map",
            style: "mapbox://styles/mapbox/light-v10",
            center: [-77.5, 40.5],
            zoom: 4,
            maxBounds: [
            [-82, 38.8], // Southwest corner of PA
            [-72.5, 43.2],  // Northeast corner of PA
        ],
        });

        map.on("load", () => {
            map.addSource("counties", {
                type: "geojson",
                data: "https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json"
            });

            map.addLayer({
                id: "county-boundaries",
                type: "line",
                source: "counties",
                paint: {
                    "line-color": "#D3D3D3",
                    "line-width": 0.5
                }
            });

            map.addLayer({
                id: "county-labels",
                type: "symbol",
                source: "counties",
                layout: {
                    "text-field": ["get", "NAME"],
                    "text-size": 12
                },
                paint: {
                    "text-color": "#000000"
                }
            });
        });

        return () => {
            markers.forEach(marker => marker.remove());
            map.remove();
        };
    });

    $: {
        if (map) {
            markers.forEach(marker => marker.remove());
            markers = [];

            locations.forEach(async (loc) => {
                const data = await fetchAirQuality(loc.lat, loc.lon, selectedDay);

                if (data && data.list && data.list.length > 0) {
                    const aqi = data.list[0].main.aqi;
                    const components = data.list[0].components;
                    const color = getAQIColor(aqi);
                    console.log("API Response:", data);
                    console.log("Components Data:", data.list[0]?.components);


                    const marker = new mapboxgl.Marker({ color })
                        .setLngLat([loc.lon, loc.lat])
                        .setPopup(
                            new mapboxgl.Popup().setHTML(`
                                <b>${loc.name}</b><br>
                                AQI: ${aqi}<br>
${
                                pollutant === "all"
                                    ? `
                    PM10: ${components.pm10 !== undefined ? components.pm10 : 'N/A'} µg/m³<br>
                    PM2.5: ${components.pm2_5 !== undefined ? components.pm2_5 : 'N/A'} µg/m³<br>
                    Ozone (O3): ${components.o3 !== undefined ? components.o3 : 'N/A'} µg/m³<br>
                    Nitrogen Dioxide (NO2): ${components.no2 !== undefined ? components.no2 : 'N/A'} µg/m³<br>
                    Sulfur Dioxide (SO2): ${components.so2 !== undefined ? components.so2 : 'N/A'} µg/m³<br>
                    Carbon Monoxide (CO): ${components.co !== undefined ? components.co : 'N/A'} µg/m³
                    `
                    : `${pollutant.toUpperCase()}: ${components[pollutant] !== undefined ? components[pollutant] : 'N/A'} µg/m³`
                            }
                            `)
                        )
                        .addTo(map);

                    markers.push(marker);
                }
            });
        }
    }

    async function fetchAirQuality(lat, lon, selectedDay) {
        const now = new Date();
        now.setDate(now.getDate() + selectedDay);
        const timestamp = Math.floor(now.getTime() / 1000);

        const url = `https://api.openweathermap.org/data/2.5/air_pollution/history?lat=${lat}&lon=${lon}&start=${timestamp}&end=${timestamp + 86400}&appid=6db2c3a1eeaf702a7ab3d756f7a63a15`;

        try {
            const response = await fetch(url);
            if (!response.ok) {
                console.error("Error fetching data:", response.statusText);
                return;
            }
            const data = await response.json();
            return data;
        } catch (error) {
            console.error("Error in fetchAirQuality:", error);
        }
    }

    function getAQIColor(aqi) {
        switch (aqi) {
            case 1: return "green";
            case 2: return "yellow";
            case 3: return "orange";
            case 4: return "red";
            case 5: return "purple";
            default: return "gray";
        }
    }
</script>

<div id="map-container" style="margin: 0; padding: 0;">
    <div id="map" style="width: 100%; height: 80vh;"></div>
    <div id="legend" class="legend">
        <div class="legend-item"><span class="legend-color green"></span>Good</div>
        <div class="legend-item"><span class="legend-color yellow"></span>Moderate</div>
        <div class="legend-item"><span class="legend-color orange"></span>Unhealthy for Sensitive Groups</div>
        <div class="legend-item"><span class="legend-color red"></span>Unhealthy</div>
        <div class="legend-item"><span class="legend-color purple"></span>Very Unhealthy</div>
    </div>
</div>

<style>
    #map {
        width: 70vw;
        height: 80vh;
        margin: 0 auto; /* Center the map */
        border: 1px solid #ccc;
        border-radius: 5px;
    }

    .legend {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 10px;
        padding: 10px;
        background-color: white;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-family: Arial, sans-serif;
    }

    .legend-item {
        display: flex;
        align-items: center;
        margin: 0 10px;
        font-size: 14px;
    }

    .legend-color {
        width: 20px;
        height: 20px;
        border-radius: 50%;
        margin-right: 5px;
    }

    .legend-color.green { background-color: green; }
    .legend-color.yellow { background-color: yellow; }
    .legend-color.orange { background-color: orange; }
    .legend-color.red { background-color: red; }
    .legend-color.purple { background-color: purple; }
</style>
