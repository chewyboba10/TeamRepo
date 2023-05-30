<html>
<head>
  <meta charset="UTF-8">
  <title>Data Table Sorting</title>
  <style>
    table {
      border-collapse: collapse;
    }
    th, td {
      padding: 8px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }
  </style>
</head>
<body>
  <h2>Data Table</h2>
  <table id="dataTable">
    <thead>
      <tr>
        <th onclick="sortTable(0)">Name</th>
        <th onclick="sortTable(1)">Age</th>
        <th onclick="sortTable(2)">City</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>John</td>
        <td>25</td>
        <td>New York</td>
      </tr>
      <tr>
        <td>Jane</td>
        <td>30</td>
        <td>London</td>
      </tr>
      <tr>
        <td>Mike</td>
        <td>22</td>
        <td>Paris</td>
      </tr>
    </tbody>
  </table>

  <script>
    function sortTable(columnIndex) {
      var table, rows, switching, i, x, y, shouldSwitch;
      table = document.getElementById("dataTable");
      switching = true;

      while (switching) {
        switching = false;
        rows = table.getElementsByTagName("tr");

        for (i = 1; i < (rows.length - 1); i++) {
          shouldSwitch = false;

          x = rows[i].getElementsByTagName("td")[columnIndex];
          y = rows[i + 1].getElementsByTagName("td")[columnIndex];

          if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
            shouldSwitch = true;
            break;
          }
        }

        if (shouldSwitch) {
          rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
          switching = true;
        }
      }
    }
  </script>
</body>
</html>
