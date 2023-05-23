<html>
<head>
  <title>GeoGuessr Leaderboard</title>
  <style>
  * {
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  }
  h1 {
    font-size: 32pt;
    text-align: center;
    margin-top: 40px;
  }
  table.board {
    font-size: 13pt;
    border-collapse: collapse;
    margin: 25px 0;
    margin-top: 40px;
    width: 75%;
  }
  .board thead tr {
    font-size: 17pt;
    font-weight: bold;
    background-color: #246186;
    color: #ffffff;
    text-align: left;
  }
  .board td {
    text-align: center;
    padding: 12px 15px;
    border: none;
    height: 50px;
  }
  .board tr {
    font-weight: bold;
    border: none;
    transition-duration: 0.3s;
    color: #0a2f47;
    background-color: #58a4d3;
    border-bottom: 4px solid #2b8bc6;
    width: 50%;
  }
  .board tr:hover {
    color: #ffffff;
    background-color: #246186;
  }
  #container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  #dots {
    margin-top: -60px;
    margin-bottom: -35px;
    font-size: 60pt;
    color: #f1cc0c;
    opacity: 0.5;
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
      <td>Ryan</td>
      <td>1000</td>
      <td>00:45</td>
    </tr>
    <tr>
      <td>Aniket</td>
      <td>800</td>
      <td>01:15</td>
    </tr>
    <tr>
      <td>Max</td>
      <td>650</td>
      <td>00:55</td>
    </tr>
    <tr>
      <td>Evan</td>
      <td>500</td>
      <td>01:21</td>
    </tr>
    <tr>
      <td>Nathan</td>
      <td>650</td>
      <td>00:47</td>
    </tr>
    <tr>
      <td>Kalani</td>
      <td>0</td>
      <td>10:32</td>
    </tr>
    <tr>
      <td>Jaden</td>
      <td>1100</td>
      <td>00:31</td>
    </tr>
  </table>

</body>
</html>
