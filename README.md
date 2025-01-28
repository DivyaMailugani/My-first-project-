# My-first-project- 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>

<body>
    <div class="bg-container mt-5 shadow">
        <h1 class="heading">User Details</h1>

        <!-- Form to Add/Edit User -->
        <form id="my-form">
            <div class="form-group">
                <label for="FirstName" class="label-name">First Name</label>
                <input type="text" id="FirstName" placeholder="First Name" class="form-control" required>
            </div>

            <div class="form-group">
                <label for="LastName" class="label-name">Last Name</label>
                <input type="text" id="LastName" placeholder="Last Name" class="form-control" required>
            </div>

            <div class="form-group">
                <label for="Email" class="label-name">Email</label>
                <input type="email" id="Email" placeholder="Email" class="form-control" required>
            </div>

            <div class="form-group">
                <label for="Department" class="label-name">Department</label>
                <input type="text" id="Department" placeholder="Department" class="form-control" required>
            </div>

            <button type="submit" class="btn btn-primary" id="Add-Button">Add User</button>
        </form>

        <!-- Table to display added users -->
        <h2 class="mt-5">Added Users</h2>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Email</th>
                    <th>Department</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody id="userTableBody">
                <!-- Newly added users will be inserted here -->
            </tbody>
        </table>
    </div>

    <script>
        let usersList = []; // Array to store the added users
        let editingIndex = -1; // Store the index of the user being edited
        let userIdCounter = 1; // To generate a unique ID for each user

        // Function to handle adding or editing a user
        document.getElementById('my-form').addEventListener('submit', function(e) {
            e.preventDefault();

            const firstName = document.getElementById('FirstName').value;
            const lastName = document.getElementById('LastName').value;
            const email = document.getElementById('Email').value;
            const department = document.getElementById('Department').value;

            // If we are editing an existing user
            if (editingIndex !== -1) {
                // Editing existing user
                usersList[editingIndex] = {
                    id: usersList[editingIndex].id, // Keep the same ID
                    firstName,
                    lastName,
                    email,
                    department
                };
                editingIndex = -1; // Reset editing index
                document.getElementById('Add-Button').textContent = 'Add User'; // Change button text back to "Add User"
            } else {
                // Adding a new user with a unique ID
                usersList.push({
                    id: userIdCounter++, // Assign a new ID and increment the counter
                    firstName,
                    lastName,
                    email,
                    department
                });
            }

            // Update the table with the latest users
            updateUserTable();

            // Reset the form
            document.getElementById('my-form').reset();
        });

        // Function to update the user table
        function updateUserTable() {
            const userTableBody = document.getElementById('userTableBody');
            userTableBody.innerHTML = ''; // Clear the table before adding updated users

            usersList.forEach((user, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.id}</td>
                    <td>${user.firstName}</td>
                    <td>${user.lastName}</td>
                    <td>${user.email}</td>
                    <td>${user.department}</td>
                    <td>
                        <button class="btn btn-warning btn-sm" onclick="editUser(${index})">Edit</button>
                        <button class="btn btn-danger btn-sm" onclick="deleteUser(${index})">Delete</button>
                    </td>
                `;
                userTableBody.appendChild(row);
            });
        }

        // Function to edit a user
        function editUser(index) {
            const user = usersList[index];

            // Populate the form with the selected user's data
            document.getElementById('FirstName').value = user.firstName;
            document.getElementById('LastName').value = user.lastName;
            document.getElementById('Email').value = user.email;
            document.getElementById('Department').value = user.department;

            // Set the index of the user being edited
            editingIndex = index;
            document.getElementById('Add-Button').textContent = 'Update User'; // Change button text to "Update User"
        }

        // Function to delete a user
        function deleteUser(index) {
            // Remove the user from the list
            usersList.splice(index, 1);

            // Update the table after deletion
            updateUserTable();
        }
    </script>
</body>

</html>
