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
    // Sortable table columns
    function sortTable(n) {
      var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
      table = document.getElementById(“leaderboardTable”);
      switching = true;
      // Set the sorting direction to ascending
      dir = “asc”;