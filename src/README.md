# Source Code

Place the MVP implementation in this folder.

Keep the structure consistent with the selected technology stack and make sure major modules can be traced back to the stories, architecture notes, and validation evidence referenced in `docs/`.


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CyberGuard Login</title>

<link rel="stylesheet" href="login.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
</head>
<body class="login-page">

<div class="login-card">

  <div class="login-logo">
    <i class="fa-solid fa-shield-halved"></i>
    <h2 id="loginTitle">Student Login</h2>
    <p>CyberGuard Reporting System</p>
  </div>

  <!-- SWITCH BUTTON -->
  <div class="login-switch">
    <button id="studentBtn" class="active" onclick="setLogin('student')">Student</button>
    <button id="adminBtn" onclick="setLogin('admin')">Admin</button>
  </div>

  <!-- INPUTS -->
  <div class="input-group">
    <label>Username</label>
    <input type="text" id="username" placeholder="Enter username">
  </div>

  <div class="input-group">
    <label>Password</label>
    <input type="password" id="password" placeholder="Enter password">
  </div>

  <button class="login-btn" onclick="login()">Login</button>

  <p class="register-text">
    No account? <a href="#" onclick="goToPage('register.html', 'right')">Create one</a>
  </p>

</div>

<script>
/* LOGIN TYPE */
let loginType="student";

function setLogin(type){
  loginType=type;
  let studentBtn=document.getElementById("studentBtn");
  let adminBtn=document.getElementById("adminBtn");

  studentBtn.classList.remove("active");
  adminBtn.classList.remove("active");

  if(type==="student"){
    studentBtn.classList.add("active");
    document.getElementById("loginTitle").innerText="Student Login";
  } else {
    adminBtn.classList.add("active");
    document.getElementById("loginTitle").innerText="Admin Login";
  }
}

/*  SAFE GET USERS */
function getUsers(){
  try{
    let data = localStorage.getItem("users");
    return data ? JSON.parse(data) : [];
  }catch(e){
    console.log("Corrupted users data, resetting safely...");
    return [];
  }
}

/*  SAVE USERS */
function saveUsers(users){
  localStorage.setItem("users", JSON.stringify(users));
}

/*  CREATE DEFAULT ADMIN (SAFE) */
(function(){
  let users = getUsers();

  if(!users.find(u => u.username === "admin")){
    users.push({
      username:"admin",
      password:"admin123",
      role:"admin"
    });
  }

  const investigators = [
    { username:"adrian", password:"123456", name:"Det. Adrian Cruz" },
    { username:"maria", password:"123456", name:"Det. Maria Santos" },
    { username:"joshua", password:"123456", name:"Det. Joshua Reyes" },
    { username:"andrea", password:"123456", name:"Det. Andrea Lim" },
    { username:"carlo", password:"123456", name:"Det. Carlo Mendoza" }
  ];

  investigators.forEach(inv => {
    if(!users.find(u => u.username === inv.username)){
      users.push({
        username: inv.username,
        password: inv.password,
        role:"investigator",
        name: inv.name // REQUIRED
      });
    }
  });

  saveUsers(users);
})();

/* LOGIN */
function login(){
  let username = document.getElementById("username").value;
  let password = document.getElementById("password").value;

  let users = getUsers();

  let user = users.find(u =>
    u.username === username && u.password === password
  );

  if(!user){
    alert("Invalid login");
    return;
  }

  // save full session (IMPORTANT)
  localStorage.setItem("loggedUser", JSON.stringify(user));

  // routing by role
  if(user.role === "student" || !user.role){
  user.role = "student"; // fallback safety
  localStorage.setItem("loggedUser", JSON.stringify(user));
  window.location.href = "homepage3.html";
}

  else if(user.role === "admin"){
    window.location.href = "admin-dashboard.html";
  }

  else if(user.role === "investigator"){
    window.location.href = "investigator.html";
  }

  else {
    alert("Unknown role");
  }
}
/* PAGE LOAD ANIMATION */
window.addEventListener("DOMContentLoaded", () => {
  document.body.classList.add("show");
});

/* NAVIGATION */
function goToPage(page, direction = "right"){
  if(direction === "left"){
    document.body.classList.add("exit-left");
  } else {
    document.body.classList.add("exit-right");
  }

  setTimeout(() => {
    window.location.href = page;
  }, 350);
}
</script>

</body>
</html>







