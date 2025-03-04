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


