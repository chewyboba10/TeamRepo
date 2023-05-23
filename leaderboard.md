<!DOCTYPE html>
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
    }
    
    tr:hover {
      background-color: #f5f5f5;
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
            if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
              shouldSwitch = true;
              break;
            }
          } else if (dir === "desc") {
            if (x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
              shouldSwitch = true;
              break;
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
    <tr>
      <th onclick="sortTable(0)">Username</th>
      <th onclick="sortTable(1)">Score</th>
      <th onclick="sortTable(2)">Time</th>
    </tr>
    <tr>
      <td>John</td>
      <td>1000</td>
      <td>00:45</td>
    </tr>
    <tr>
      <td>Jane</td>
      <td>800</td>
      <td>01:15</td>
    </tr>
    <tr>
      <td>Mark</td>
      <td>650</td>
      <td>00:55</td>
    </tr>
    <tr>
      <td>Sarah</td>
      <td>500</td>
      <td>01:21</td>
    </tr>
  </table>

</body>
</html>
