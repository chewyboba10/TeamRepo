<!DOCTYPE html>
<html>
<head>
  <title>Leaderboard</title>
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
    form {
      margin-bottom: 20px;
    }
    form input[type=“text”], form input[type=“number”] {
      padding: 8px;
      width: 200px;
      margin-right: 10px;
    }
    form input[type=“submit”] {
      padding: 8px 16px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
  <script>
    function sortTable(n) {
      var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
      table = document.getElementById(“leaderboardTable”);
      switching = true;
      // Set the sorting direction to ascending
      dir = “asc”;
      while (switching) {
        switching = false;
        rows = table.rows;
        for (i = 1; i < (rows.length - 1); i++) {
          shouldSwitch = false;
          x = rows[i].getElementsByTagName(“TD”)[n];
          y = rows[i + 1].getElementsByTagName(“TD”)[n];
          if (dir === “asc”) {
            if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
              shouldSwitch = true;
              break;
            }
          } else if (dir === “desc”) {
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
          if (switchcount === 0 && dir === “asc”) {
            dir = “desc”;
            switching = true;
          }
        }
      }
    }
  </script>
</head>
<body>
  <h1>Leaderboard</h1>
  <table id=“leaderboardTable”>
    <tr>
      <th onclick=“sortTable(0)“>Username</th>
      <th onclick=“sortTable(1)“>Time</th>
      <th onclick=“sort