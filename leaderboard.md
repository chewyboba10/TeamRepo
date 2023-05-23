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
    // Sortable table columns
    function sortTable(n) {
      var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
      table = document.getElementById("leaderboardTable");
      switching = true;
      // Set the sorting direction to ascending
      dir = "asc";
      /* Make a loop that will continue until
      no switching has been done */
      while (switching) {
        switching = false;
        rows = table.rows;
        /* Loop through all table rows (except the
        first, which contains table headers) */
        for (i = 1; i < (rows.length - 1); i++) {
          shouldSwitch = false;
          /* Get the two elements you want to compare,
          one from the current row and one from the next */
          x = rows[i].getElementsByTagName("TD")[n];
          y = rows[i + 1].getElementsByTagName("TD")[n];
          /* Check if the two rows should switch place,
          based on the direction, asc or desc */
          if (dir === "asc") {
            if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
              // If so, mark as a switch and break the loop
              shouldSwitch = true;
              break;
            }
          } else if (dir === "desc") {
            if (x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
              // If so, mark as a switch and break the loop
              shouldSwitch = true;
              break;
            }
          }
        }
        if (shouldSwitch) {
          /* If a switch has been marked, make the switch
          and mark that a switch has been done */
          rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
          switching = true;
          // Each time a switch is done, increase switchcount by 1
          switchcount++;
        } else {
          /* If no switching has been done AND the direction is "asc",
          set the direction to "desc" and run the while loop again */
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
  <h1>GeoGuessr Leaderboard</h1>
  
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
