<html>
<head>
    <title>Geoguessr Data</title>
    <style>
        table {
            border-collapse: collapse;
            width: 100%;
        } 
        th, td {
            padding: 8px;
            text-align: left;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Geoguessr Data</h1>
    <table id="data-table">
        <thead>
            <tr>
                <th onclick="sortTable('username')">Username</th>
                <th onclick="sortTable('score')">Score</th>
                <th onclick="sortTable('date')">Date</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
    <script>
        var apiUrl = 'https://ramen-kj.duckdns.org/api/geoguessr/';
        var currentSortColumn = ''; // Track the current sort column
        var sortAscending = true; // Track the sort order (ascending or descending)
        function sortTable(column) {
            if (currentSortColumn === column) {
                sortAscending = !sortAscending; // Toggle sort order if the same column is clicked
            } else {
                sortAscending = true; // Reset sort order to ascending if a different column is clicked
            }
            currentSortColumn = column; // Update the current sort column
            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    data.sort((a, b) => {
                        // Sorting logic based on the selected column
                        if (column === 'username') {
                            return sortAscending ? a.username.localeCompare(b.username) : b.username.localeCompare(a.username);
                        } else if (column === 'score') {
                            return sortAscending ? a.score - b.score : b.score - a.score;
                        } else if (column === 'date') {
                            return sortAscending ? new Date(a.game_datetime) - new Date(b.game_datetime) : new Date(b.game_datetime) - new Date(a.game_datetime);
                        }
                        return 0;
                    });
                    var tableBody = document.querySelector('#data-table tbody');
                    tableBody.innerHTML = ''; // Clear existing table rows
                    data.forEach(entry => {
                        var row = document.createElement('tr');
                        var usernameCell = document.createElement('td');
                        usernameCell.textContent = entry.username;
                        row.appendChild(usernameCell);
                        var scoreCell = document.createElement('td');
                        scoreCell.textContent = entry.score;
                        row.appendChild(scoreCell);
                        var dateCell = document.createElement('td');
                        var date = new Date(Date.parse(entry.game_datetime));
                        dateCell.textContent = date.toLocaleDateString();
                        row.appendChild(dateCell);
                        tableBody.appendChild(row);
                    });
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('An error occurred. Please try again.'); // Show an error message
                });
        }       
        // Initial sorting based on the 'username' column in ascending order
        sortTable('username');
    </script>
</body>
</html>
