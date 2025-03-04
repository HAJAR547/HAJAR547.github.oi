/employee_system
│-- app.py  (Flask backend)
│-- database.db  (SQLite database)
│-- /templates
│   │-- index.html  (Login page)
│   │-- register.html  (Registration page)
│   │-- dashboard.html  (Employee search)
│-- /static
│   │-- style.css  (Styling)


from flask import Flask, render_template, request, redirect, url_for, session
import sqlite3

app = Flask(_name_)
app.secret_key = "secret_key"

# Create the database
def create_database():
    conn = sqlite3.connect("database.db")
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS employees (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        emp_id TEXT UNIQUE,
                        name TEXT,
                        email TEXT UNIQUE,
                        branch TEXT,
                        password TEXT)''')
    conn.commit()
    conn.close()

create_database()

# Login Page
@app.route("/", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        emp_id = request.form["emp_id"]
        password = request.form["password"]

        conn = sqlite3.connect("database.db")
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM employees WHERE emp_id = ? AND password = ?", (emp_id, password))
        employee = cursor.fetchone()
        conn.close()

        if employee:
            session["emp_id"] = emp_id
            return redirect(url_for("dashboard"))
        else:
            return "Invalid Employee ID or Password!"

    return render_template("index.html")

# Registration Page
@app.route("/register", methods=["GET", "POST"])
def register():
    if request.method == "POST":
        emp_id = request.form["emp_id"]
        name = request.form["name"]
        email = request.form["email"]
        branch = request.form["branch"]
        password = request.form["password"]

        conn = sqlite3.connect("database.db")
        cursor = conn.cursor()
        try:
            cursor.execute("INSERT INTO employees (emp_id, name, email, branch, password) VALUES (?, ?, ?, ?, ?)",
                           (emp_id, name, email, branch, password))
            conn.commit()
        except sqlite3.IntegrityError:
            return "Employee already exists or email is in use."
        finally:
            conn.close()

        return redirect(url_for("login"))

    return render_template("register.html")

# Dashboard (Search Employee)
@app.route("/dashboard", methods=["GET", "POST"])
def dashboard():
    if "emp_id" not in session:
        return redirect(url_for("login"))

    employee = None
    if request.method == "POST":
        search_emp_id = request.form["search_emp_id"]

        conn = sqlite3.connect("database.db")
        cursor = conn.cursor()
        cursor.execute("SELECT emp_id, name, email, branch FROM employees WHERE emp_id = ?", (search_emp_id,))
        employee = cursor.fetchone()
        conn.close()

    return render_template("dashboard.html", employee=employee)

# Logout
@app.route("/logout")
def logout():
    session.pop("emp_id", None)
    return redirect(url_for("login"))

if _name_ == "_main_":
    app.run(debug=True)

    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h2>Employee Login</h2>
    <form method="POST">
        <label>Employee ID:</label>
        <input type="text" name="emp_id" required>
        <label>Password:</label>
        <input type="password" name="password" required>
        <button type="submit">Login</button>
    </form>
    <a href="{{ url_for('register') }}">Create a new account</a>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Register</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h2>Employee Registration</h2>
    <form method="POST">
        <label>Employee ID:</label>
        <input type="text" name="emp_id" required>
        <label>Name:</label>
        <input type="text" name="name" required>
        <label>Email:</label>
        <input type="email" name="email" required>
        <label>Branch:</label>
        <input type="text" name="branch" required>
        <label>Password:</label>
        <input type="password" name="password" required>
        <button type="submit">Register</button>
    </form>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Dashboard</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h2>Search Employee</h2>
    <form method="POST">
        <label>Enter Employee ID:</label>
        <input type="text" name="search_emp_id" required>
        <button type="submit">Search</button>
    </form>

    {% if employee %}
        <h3>Employee Details:</h3>
        <p>Employee ID: {{ employee[0] }}</p>
        <p>Name: {{ employee[1] }}</p>
        <p>Email: {{ employee[2] }}</p>
        <p>Branch: {{ employee[3] }}</p>
    {% endif %}

    <a href="{{ url_for('logout') }}">Logout</a>
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
pip install flask
python app.py 
http://127.0.0.1:5000/
