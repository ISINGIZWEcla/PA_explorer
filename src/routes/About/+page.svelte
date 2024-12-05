<script>
    import Nav from "../../components/Nav.svelte";  
    </script>
      
    <div class="container">
        <!-- Sidebar Navigation -->
        <div class="sidebar">
            <Nav activePage="About" />
        </div>
    
        <!-- Main Content -->
        <div class="main-content">
            <h1>About PA Air Quality Explorer</h1>
            <!-- Main Heading Image -->
            <img src="home.jpg" alt="Air Quality Dashboard" class="main-image" />
    
            <p>Welcome to Pennsylvania's Air Quality Explorer, an interactive dashboard designed to provide real-time, historical, and forecasted air quality insights for Pennsylvania. Our mission is to empower users—including individuals with respiratory conditions, policymakers, businesses, and researchers—to make informed decisions by visualizing critical air quality patterns and trends.</p>
    
            <h2>Key Features</h2>
            <ul>
                <li><b>Interactive Map:</b> Explore air quality levels across Pennsylvania using a dynamic, color-coded map. Filter by pollutant type, time frame, and region to gain actionable insights.</li>
                <li><b>Real-Time Data and Forecasting:</b> Monitor current air quality and plan ahead with a four-day forecast for major pollutants (PM2.5, Ozone, CO, NO2, and SO2).</li>
                <li><b>Temporal Visualizations:</b> Understand seasonal and long-term trends through customizable bar charts and line graphs.</li>
            </ul>
    
            <h2>Team Members</h2>
            <!-- Team Member 1 -->
            <div class="team-member">
                <img src="passport.JPG" alt="Clarisse Isingizwe" class="team-photo" />
                <div>
                    <p><strong>Clarisse Isingizwe</strong></p>
                    <p>API Integration and CSS Design</p>
                </div>
            </div>
            <!-- Team Member 2 -->
            <div class="team-member">
                <img src="ifoto.jpg" alt="Emma Sylvine Ingabire Ishimirwe" class="team-photo" />
                <div>
                    <p><strong>Emma Sylvine Ingabire Ishimirwe</strong></p>
                    <p>Visualizations and Interactivity</p>
                </div>
            </div>
    
            <h2>Acknowledgements</h2>
            <p>We are grateful to the following individuals and organizations for their support and contributions:</p>
            <ul>
                <li><strong>Professor Adam</strong>, for his invaluable guidance and mentorship throughout this project as part of the Data Visualization class.</li>
                <li><strong>OpenWeather API</strong>, for providing the real-time, historical, and forecasted air quality data used in this dashboard.</li>
                <li><strong>D3.js and Mapbox communities</strong>, for their open-source resources and inspiring examples of interactive visualizations, which helped shape the design of our dashboard.</li>
                <li><strong>U.S. EPA Air Quality System</strong>, <strong>NOAA's Integrated Surface Database</strong>, and <strong>Data Commons Air Quality Metrics</strong>, for providing comprehensive datasets and metadata that enriched our project’s analysis.</li>
            </ul>
            <p>This dashboard is a testament to the power of teamwork, open data, and cutting-edge visualization technologies to address pressing public health challenges. We hope it serves as a valuable tool for all who use it.</p>
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
          box-shadow: 2px 0 5px rgba(237, 232, 232, 0.895);
      }
    
      /* Main Content Styles */
      .main-content {
          flex: 1;
          padding-left: 2rem;
          display: flex;
          flex-direction: column;
          overflow-y: auto;
      }
    
      .main-image {
          max-width: 80%;
          height: auto;
          margin-bottom: 1rem;
          border-radius: 8px;
      }
    
      h1 {
          margin-bottom: 1rem;
          font-size: 24px;
          font-family: Verdana, Geneva, Tahoma, sans-serif;
      }
    
      h2 {
          margin-top: 1.5rem;
          margin-bottom: 0.5rem;
          font-size: 20px;
          font-family: Verdana, Geneva, Tahoma, sans-serif;
      }
    
      p {
          margin-bottom: 1rem;
          line-height: 1.6;
          font-size: 16px;
          font-family: Arial, Helvetica, sans-serif;
      }
    
      ul {
          margin: 0 0 1rem 1.5rem;
          padding: 0;
          font-size: 16px;
          font-family: Arial, Helvetica, sans-serif;
      }
    
      li {
          margin-bottom: 0.5rem;
      }
    
      /* Team Member Styles */
      .team-member {
          display: flex;
          align-items: center;
          margin-bottom: 1rem;
      }
    
      .team-photo {
          width: 80px;
          height: 80px;
          border-radius: 50%;
          margin-right: 1rem;
          object-fit: cover;
      }

      /* Styling <strong> in blue */
  strong {
      color: blue;
  }
    </style>
    