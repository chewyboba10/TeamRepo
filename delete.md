<html>
<head>
    <title>Delete Geoguessr User</title>
</head>
<body>
    <h1>Delete Geoguessr User</h1>
    
    <form id="deleteForm">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required>
        <button type="submit">Delete User</button>
    </form>
    
    <script>
        document.getElementById('deleteForm').addEventListener('submit', function(event) {
            event.preventDefault(); // Prevent form submission
            
            var username = document.getElementById('username').value;
            var apiUrl = 'https://ramen-kj.duckdns.org/api/geoguessr/';
            
            fetch(apiUrl, {
                method: 'DELETE',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ username: username })
            })
            .then(response => response.json())
            .then(data => {
                console.log(data); // Handle the response data
                alert(data); // Show a success message or perform any desired action
            })
            .catch(error => {
                console.error('Error:', error);
                alert('An error occurred. Please try again.'); // Show an error message
            });
        });
    </script>
</body>
</html>