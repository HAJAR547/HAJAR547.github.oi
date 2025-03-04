 // تسجيل الدخول
    let loginForm = document.getElementById("loginForm");
    if (loginForm) {
        loginForm.addEventListener("submit", function(event) {
            event.preventDefault();
            
            let emp_id = document.getElementById("emp_id").value;
            let password = document.getElementById("password").value;
            
            

