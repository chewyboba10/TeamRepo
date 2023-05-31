<html>
<head>
  <title>GeoGuessr Leaderboard</title>
  <script>
    function sortTable(n) {
      // Existing sorting code
    }
  </script>
</head>
<body>
  <h1>Ramen KJ GeoGuesser Leaderboard</h1>
  <table id="leaderboardTable">
    <!-- Table will be populated dynamically -->
  </table>

  <script>
    function getData() {
      const leaderboardTable = document.getElementById("leaderboardTable");
      leaderboardTable.innerHTML = "";

      const headerRow = document.createElement("tr");
      const headers = ["Username", "Score", "Time", "Date"]; // Update the headers
      headers.forEach(header => {
        const th = document.createElement("th");
        th.textContent = header;
        th.setAttribute("onclick", "sortTable(" + headers.indexOf(header) + ")");
        headerRow.appendChild(th);
      });
      leaderboardTable.appendChild(headerRow);

      const username = localStorage.getItem("username");
      const points = localStorage.getItem("points");

      if (username && points) {
        const row = document.createElement("tr");
        const cells = [username, points, "", new Date().toLocaleDateString("en-US")]; // Update the cell values as needed
        cells.forEach(cellValue => {
          const cell = document.createElement("td");
          cell.textContent = cellValue;
          row.appendChild(cell);
        });
        leaderboardTable.appendChild(row);
      }
    }

    window.addEventListener("load", getData);
  </script>
</body>
</html>
