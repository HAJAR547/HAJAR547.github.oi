<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Employee Login</title>
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
    <a href="register.html">Create a new account</a>
    <p id="loginMessage"></p>
    <script src="script.js"></script>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Register</title>
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
    <a href="index.html">Back to Login</a>
    <p id="registerMessage"></p>
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
    margin-bottom: 10px;
}

// تسجيل الموظفين
document.addEventListener("DOMContentLoaded", function() {
    let registerForm = document.getElementById("registerForm");
    if (registerForm) {
        registerForm.addEventListener("submit", function(event) {
            event.preventDefault();
            
            let emp_id = document.getElementById("reg_emp_id").value;
            let email = document.getElementById("reg_email").value;
            let phone = document.getElementById("reg_phone").value;
            let password = document.getElementById("reg_password").value;

            if (localStorage.getItem(emp_id)) {
                document.getElementById("registerMessage").innerText = "Employee already exists!";
                return;
            }

            let employee = { emp_id, email, phone, password };
            localStorage.setItem(emp_id, JSON.stringify(employee));
            document.getElementById("registerMessage").innerText = "Registration successful!";
        });
    }

    // تسجيل الدخول
    let loginForm = document.getElementById("loginForm");
    if (loginForm) {
        loginForm.addEventListener("submit", function(event) {
            event.preventDefault();
            
            let emp_id = document.getElementById("emp_id").value;
            let password = document.getElementById("password").value;
            
            let employee = localStorage.getItem(emp_id);
            if (employee) {
                let storedData = JSON.parse(employee);
                if (storedData.password === password) {
                    document.getElementById("loginMessage").innerText = "Login successful!";
                } else {
                    document.getElementById("loginMessage").innerText = "Incorrect password!";
                }
            } else {
                document.getElementById("loginMessage").innerText = "Employee not found!";
            }
        });
    }
});


git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name

git add .
git commit -m "Initial commit"
git push origin main

