<html>
<head>
  <title>GeoGuessr Leaderboard</title>
  <style>
    table {
        border-collapse: collapse;
        width: 100%;
        }
        th, td {
            padding: 8px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        th {
            background-color: #f2f2f2;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        tr:hover {
            background-color: #f5f5f5;
        }
        @keyframes slideIn {
            0% {
                opacity: 0;
                transform: translateX(-50px);
            }
            100% {
                opacity: 1;
                transform: translateX(0);
            }
        }
        @keyframes fadeIn {
            0% {
                opacity: 0;
            }
            100% {
                opacity: 1;
            }
        }
        th, td {
            animation: slideIn 0.5s ease;
        }
        table {
            animation: fadeIn 1s ease;
        }
  </style>

  <script>
    function sortTable(n) {
      var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
      table = document.getElementById("leaderboardTable");
      switching = true;
      dir = "asc";
      while (switching) {
        switching = false;
        rows = table.rows;
        for (i = 1; i < (rows.length - 1); i++) {
          shouldSwitch = false;
          x = rows[i].getElementsByTagName("TD")[n];
          y = rows[i + 1].getElementsByTagName("TD")[n];
          if (dir === "asc") {
            if (n === 0) {
              if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
                shouldSwitch = true;
                break;
              }
            } else {
              if (parseInt(x.innerHTML) > parseInt(y.innerHTML)) {
                shouldSwitch = true;
                break;
              }
            }
          } else if (dir === "desc") {
            if (n === 0) {
              if (x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
                shouldSwitch = true;
                break;
              }
            } else {
              if (parseInt(x.innerHTML) < parseInt(y.innerHTML)) {
                shouldSwitch = true;
                break;
              }
            }
          }
        }
        if (shouldSwitch) {
          rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
          switching = true;
          switchcount++;
        } else {
          if (switchcount === 0 && dir === "asc") {
            dir = "desc";
            switching = true;
          }
        }
      }
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
      fetch('http://172.22.157.253:8086/api/geoguessr/')
        .then(response => response.json())
        .then(data => {
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

          data.forEach(rowData => {
            const row = document.createElement("tr");
            const values = Object.values(rowData).filter((value, index) => index !== 0); // Exclude the first value (id)
            values.forEach((value, index) => {
              const cell = document.createElement("td");
              if (index === values.length - 1) {
                // Format the date as desired (modify the code below accordingly)
                const date = new Date(value);
                const formattedDate = date.toLocaleDateString("en-US");
                cell.textContent = formattedDate;
              } else {
                cell.textContent = value;
              }
              row.appendChild(cell);
            });
            leaderboardTable.appendChild(row);
          });
        })
        .catch(error => console.error(error));
    }

    window.addEventListener("load", getData);
  </script>
</body>
</html>