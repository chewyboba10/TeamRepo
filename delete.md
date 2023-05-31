<!DOCTYPE html>
<html>
<head>
  <title>User Deletion</title>
</head>
<body>
  <h1>User Deletion</h1>
  <form id="deleteForm">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    <button type="submit">Delete User</button>
  </form>

  <script>
    const form = document.getElementById('deleteForm');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();

      const username = document.getElementById('username').value;

      try {
        const response = await fetch(`http://localhost:5000/api/geoguessr?username=${username}`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json'
          }
        });

        const result = await response.json();

        if (response.ok) {
          alert('User deleted successfully');
        } else {
          alert(`Failed to delete user: ${result.message}`);
        }
      } catch (error) {
        console.error('An error occurred:', error);
      }
    });
  </script>
</body>
</html>
