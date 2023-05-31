<!DOCTYPE html>
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
        }
    </style>
</head>
<body>
    <h1>Geoguessr Data</h1>
    <table id="data-table">
        <thead>
            <tr>
                <th>Username</th>
                <th>Score</th>
                <th>Date</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
    <script>
        var apiUrl = 'https://ramen-kj.duckdns.org/api/geoguessr/';
        fetch(apiUrl)
            .then(response => response.json())
            .then(data => {
                // Sort the data by username, score, and date
                data.sort((a, b) => {
                    if (a.username < b.username) return -1;
                    if (a.username > b.username) return 1;
                    if (a.score < b.score) return -1;
                    if (a.score > b.score) return 1;
                    if (a.game_datetime < b.game_datetime) return -1;
                    if (a.game_datetime > b.game_datetime) return 1;
                    return 0;
                });     
                // Display the sorted data in the table
                var tableBody = document.querySelector('#data-table tbody');
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
    </script>
</body>
</html>
