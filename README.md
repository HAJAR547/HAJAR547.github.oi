/almajdouie-accounting
│-- index.html       (Login Page)
│-- register.html    (Register Page)
│-- employees.html   (Employee Data Page)
│-- style.css        (CSS Styles)
│-- script.js        (JavaScript Logic)


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login - Almajdouie Accounting</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h2>Employee Login</h2>
    <form id="loginForm">
        <label>Employee ID:</label>
        <input type="text" id="emp_id" required>
        <label>Password:</label>
        <input type="password" id="password" required>
        <button type="submit">Login</button>
    </form>
    <p id="loginMessage"></p>
    <a href="register.html">Create a new account</a>
    <script src="script.js"></script>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Register - Almajdouie Accounting</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h2>Employee Registration</h2>
    <form id="registerForm">
        <label>Employee ID:</label>
        <input type="text" id="reg_emp_id" required>
        <label>Email:</label>
        <input type="email" id="reg_email" required>
        <label>Phone:</label>
        <input type="text" id="reg_phone" required>
        <label>Password:</label>
        <input type="password" id="reg_password" required>
        <button type="submit">Register</button>
    </form>
    <p id="registerMessage"></p>
    <a href="index.html">Back to Login</a>
    <script src="script.js"></script>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Employee List - Almajdouie Accounting</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h2>Employee Information</h2>
    <table>
        <tr>
            <th>Full Name</th>
            <th>Email</th>
            <th>Phone</th>
            <th>Branch</th>
        </tr>
        <tbody id="employeeTable"></tbody>
    </table>
    <a href="index.html">Logout</a>
    <script src="script.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 20px;
}

form {
    display: inline-block;
    text-align: left;
}

input {
    display: block;
    margin-bottom: 10px;
}

table {
    width: 80%;
    margin: auto;
    border-collapse: collapse;
}

th, td {
    border: 1px solid black;
    padding: 8px;
}


document.addEventListener("DOMContentLoaded", function() {
    // Employee data
    let employees = [
        { emp_id: "2221640697", name: "Hajar Ahmed Al-Majdouie", email: "mmashwali99@gmail.com", phone: "0552377985", branch: "Dammam" },
        { emp_id: "1002345678", name: "Ali Mohammed Al-Qahtani", email: "ali@almajdouie.com", phone: "0501234567", branch: "Riyadh" },
        { emp_id: "1009876543", name: "Fatima Saeed Al-Harbi", email: "fatima@almajdouie.com", phone: "0539876543", branch: "Jeddah" }
    ];

    // Save employee data to localStorage
    if (!localStorage.getItem("employees")) {
        localStorage.setItem("employees", JSON.stringify(employees));
    }

    // Register employee
    let registerForm = document.getElementById("registerForm");
    if (registerForm) {
        registerForm.addEventListener("submit", function(event) {
            event.preventDefault();

            let emp_id = document.getElementById("reg_emp_id").value;
            let email = document.getElementById("reg_email").value;
            let phone = document.getElementById("reg_phone").value;
            let password = document.getElementById("reg_password").value;

            let storedEmployees = JSON.parse(localStorage.getItem("employees"));
            storedEmployees.push({ emp_id, name: "New Employee", email, phone, branch: "Unknown" });
            localStorage.setItem("employees", JSON.stringify(storedEmployees));
            localStorage.setItem(emp_id, password);

            document.getElementById("registerMessage").innerText = "Registration successful!";
        });
    }

    // Employee login
    let loginForm = document.getElementById("loginForm");
    if (loginForm) {
        loginForm.addEventListener("submit", function(event) {
            event.preventDefault();

            let emp_id = document.getElementById("emp_id").value;
            let password = document.getElementById("password").value;

            let storedPassword = localStorage.getItem(emp_id);
            if (storedPassword && storedPassword === password) {
                window.location.href = "employees.html";
            } else {
                document.getElementById("loginMessage").innerText = "Invalid Employee ID or Password!";
            }
        });
    }

    // Display employee data
    let employeeTable = document.getElementById("employeeTable");
    if (employeeTable) {
        let storedEmployees = JSON.parse(localStorage.getItem("employees"));
        storedEmployees.forEach(emp => {
            let row = `<tr>
                <td>${emp.name}</td>
                <td>${emp.email}</td>
                <td>${emp.phone}</td>
                <td>${emp.branch}</td>
            </tr>`;
            employeeTable.innerHTML += row;
        });
    }
});

