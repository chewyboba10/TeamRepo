<!DOCTYPE html>
<html>
<head>
    <title>Geoguessr Data</title>
    <style>
        /* Global Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f2f2f2;
        }
        /* Header Styles */
        h1 {
            text-align: center;
            padding: 20px;
            margin: 0;
        }
        /* Table Styles */
        table {
            border-collapse: collapse;
            width: 90%;
            margin-left: auto;
            margin-right: auto;
            margin-top: 20px;
        }
        th, td {
            padding: 8px;
            text-align: left;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
        }
        th {
            background-color: #3354a7;
            color: white;
        }
        th:hover {
            background-color: #4e7ac9;
        }
        tr:hover {
            background-color: #f5f5f5;
        }   
        .pagination {
            text-align: center;
            margin-top: 20px;
        }
        .pagination a {
            display: inline-block;
            padding: 8px 16px;
            text-decoration: none;
            color: #3354a7;
            border: 1px solid #ddd;
        }
        .pagination a.active {
            background-color: #3354a7;
            color: white;
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
    <div class="pagination">
        <a href="#" id="prev" onclick="previousPage()">Previous</a>
        <a href="#" id="next" onclick="nextPage()">Next</a>
    </div>
    <script>
        var apiUrl = 'https://ramen-kj.duckdns.org/api/geoguessr/';
        var currentSortColumn = ''; // Track the current sort column
        var sortAscending = true; // Track the sort order (ascending or descending)
        var currentPage = 1; // Track the current page    
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
                            return sortAscending ? new Date(a.dos) - new Date(b.dos) : new Date(b.dos) - new Date(a.dos);
                        }
                        return 0;
                    });                 
                    var totalPages = Math.ceil(data.length / 10); // Calculate the total number of pages
                    currentPage = 1; // Reset current page to 1
                    updateTableData(data); // Update table data for the first page        
                    // Enable/disable previous and next links based on the current page
                    var prevLink = document.querySelector('#prev');
                    var nextLink = document.querySelector('#next');
                    if (currentPage === 1) {
                        prevLink.classList.add('disabled');
                    } else {
                        prevLink.classList.remove('disabled');
                    }
                    if (currentPage === totalPages) {
                        nextLink.classList.add('disabled');
                    } else {
                        nextLink.classList.remove('disabled');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('An error occurred. Please try again.'); // Show an error message
                });
        }       
        function updateTableData(data) {
            var startIndex = (currentPage - 1) * 10; // Calculate the start index for the current page
            var endIndex = startIndex + 10; // Calculate the end index for the current page
            var pageData = data.slice(startIndex, endIndex); // Extract data for the current page            
            var tableBody = document.querySelector('#data-table tbody');
            tableBody.innerHTML = ''; // Clear existing table rows
            pageData.forEach(entry => {
                var row = document.createElement('tr');
                var usernameCell = document.createElement('td');
                usernameCell.textContent = entry.username;
                row.appendChild(usernameCell);
                var scoreCell = document.createElement('td');
                scoreCell.textContent = entry.score;
                row.appendChild(scoreCell);
                var dateCell = document.createElement('td');
                var date = new Date(Date.parse(entry.dos));
                dateCell.textContent = date.toLocaleDateString();
                row.appendChild(dateCell);
                tableBody.appendChild(row);
            });
        }       
        function previousPage() {
            if (currentPage > 1) {
                currentPage--; // Decrement the current page
                fetch(apiUrl)
                    .then(response => response.json())
                    .then(data => {
                        updateTableData(data); // Update table data for the previous page  
                        // Enable/disable previous and next links based on the current page
                        var prevLink = document.querySelector('#prev');
                        var nextLink = document.querySelector('#next');
                        if (currentPage === 1) {
                            prevLink.classList.add('disabled');
                        } else {
                            prevLink.classList.remove('disabled');
                        }
                        if (currentPage === Math.ceil(data.length / 10)) {
                            nextLink.classList.add('disabled');
                        } else {
                            nextLink.classList.remove('disabled');
                        }
                    })
                    .catch(error => {
                        console.error('Error:', error);
                        alert('An error occurred. Please try again.'); // Show an error message
                    });
            }
        }  
        function nextPage() {
            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    var totalPages = Math.ceil(data.length / 10); // Calculate the total number of pages
                    if (currentPage < totalPages) {
                        currentPage++; // Increment the current page
                        updateTableData(data); // Update table data for the next page                        
                        // Enable/disable previous and next links based on the current page
                        var prevLink = document.querySelector('#prev');
                        var nextLink = document.querySelector('#next');
                        if (currentPage === 1) {
                            prevLink.classList.add('disabled');
                        } else {
                            prevLink.classList.remove('disabled');
                        }
                        if (currentPage === totalPages) {
                            nextLink.classList.add('disabled');
                        } else {
                            nextLink.classList.remove('disabled');
                        }
                    }
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
