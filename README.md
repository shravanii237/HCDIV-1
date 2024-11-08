<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Singapore 2-Hr Weather Forecast</title>
    <style>
        /* Background gradient for the entire page */
        body {
            background: linear-gradient(to right, #a8edea, #fed6e3);
            font-family: Poppins, sans-serif;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            padding: 20px;
        }

        /* Center and style the heading */
        h2 {
            text-align: center;
            font-size: 2em;
            color: #3B0A67;
            margin-bottom: 10px;
        }

        /* Style the timestamp */
        #timestring {
            color: #555;
            font-size: 1em;
            margin-bottom: 20px;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 5px 15px;
            border-radius: 10px;
        }

        /* Style the search box */
        #search {
            padding: 10px;
            margin-bottom: 20px;
            width: 80%;
            max-width: 400px;
            font-size: 1em;
            border: 1px solid #ccc;
            border-radius: 5px;
            outline: none;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        /* Style the table */
        table {
            width: 90%;
            max-width: 800px;
            border-collapse: collapse;
            background-color: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        /* Style for table headers */
        th {
            background-color: #3B0A67;
            color: white;
            padding: 12px;
            font-size: 1.1em;
        }

        /* Style for table rows and cells */
        td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        /* Add hover effect for rows */
        tr:hover {
            background-color: #f1f1f1;
        }

        /* Style for alternating row colors */
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
    </style>
</head>
<body>

<h2>Singapore 2-Hr Weather Forecast</h2>
<h4 id="timestring">Last Updated: </h4>

<!-- Search bar for filtering by area -->
<input type="text" id="search" placeholder="Search by area...">

<table id="weatherTable">
    <tr>
        <th>Area</th>
        <th>Forecast</th>
    </tr>
</table>

<script>
fetch('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast')
  .then(response => response.json())
  .then(responsedata => {
    const timestamp = responsedata.items[0].update_timestamp;
    const forecasts = responsedata.items[0].forecasts;

    // Display the timestamp in a human-readable format
    document.getElementById("timestring").textContent = `Last Updated: ${new Date(timestamp).toLocaleString()}`;

    const weatherTable = document.getElementById("weatherTable");

    // Populate the table with forecast data
    forecasts.forEach(forecast => {
        const row = document.createElement("tr");

        const areaCell = document.createElement("td");
        areaCell.textContent = forecast.area;
        row.appendChild(areaCell);

        const forecastCell = document.createElement("td");
        forecastCell.textContent = forecast.forecast;
        row.appendChild(forecastCell);

        weatherTable.appendChild(row);
    });

    // Search functionality for filtering table by area
    document.getElementById("search").addEventListener("input", function() {
        const searchValue = this.value.toLowerCase();
        const rows = weatherTable.getElementsByTagName("tr");

        // Loop through each row to show or hide based on search
        for (let i = 1; i < rows.length; i++) {
            const areaCell = rows[i].getElementsByTagName("td")[0];
            const areaText = areaCell.textContent.toLowerCase();

            if (areaText.includes(searchValue)) {
                rows[i].style.display = "";
            } else {
                rows[i].style.display = "none";
            }
        }
    });
  })
  .catch(error => console.error("Error fetching data:", error));
</script>

</body>
</html>
