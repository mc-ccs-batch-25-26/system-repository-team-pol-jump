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

/* ===== BODY & BACKGROUND ===== */
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: 'Poppins', sans-serif;
}

.login-page {
  position: relative;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

/* Background image layer */
.login-page::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 120%;
  height: 120%;
  background: url('images/mchs.jpg') center/cover no-repeat;
  filter: blur(8px) brightness(0.6);
  transform: scale(1.1);
  animation: moveBackground 30s linear infinite alternate;
  z-index: -2;
}

/* Optional overlay for readability */
.login-page::after {
  content: "";
  position: absolute;
  top:0;
  left:0;
  width:100%;
  height:100%;
  background: rgba(0,0,0,0.25);
  z-index:-1;
}

/* Background animation */
@keyframes moveBackground {
  0% { transform: translate(0px,0px) scale(1.1); }
  50% { transform: translate(-20px,-10px) scale(1.1); }
  100% { transform: translate(0px,0px) scale(1.1); }
}

/* ===== LOGIN CARD ===== */
.login-card {
  position: relative;
  background: rgba(255,255,255,0.85);
  padding: 40px;
  width: 350px;
  border-radius: 12px;
  box-shadow: 0 15px 40px rgba(0,0,0,0.2);
  z-index: 10;
  text-align: center;
}

/* LOGO */
.login-logo i {
  font-size: 40px;
  color: #4f46e5;
  margin-bottom: 10px;
}

.login-logo h2 {
  margin: 5px 0;
}

.login-logo p {
  font-size: 14px;
  color: #555;
}

/* SWITCH BUTTON */
.login-switch{
  display:flex;
  border:1px solid #ddd;
  border-radius:6px;
  overflow:hidden;
  margin:20px 0;
}

.login-switch button{
  flex:1;
  padding:10px;
  border:none;
  background:#f5f5f5;
  cursor:pointer;
  font-weight:500;
  transition: 0.3s;
}

.login-switch button.active{
  background:#4f46e5;
  color:white;
}

/* INPUTS */
.input-group{
  margin-bottom:18px;
  text-align:left;
}

.input-group label{
  font-size:14px;
  display:block;
  margin-bottom:6px;
}

.input-group input{
  width:100%;
  padding:10px;
  border:1px solid #ddd;
  border-radius:6px;
  font-size:14px;
}

.input-group input:focus{
  outline:none;
  border-color:#4f46e5;
}

/* LOGIN BUTTON */
.login-btn{
  width:100%;
  padding:12px;
  border:none;
  background:#4f46e5;
  color:white;
  border-radius:6px;
  font-size:15px;
  cursor:pointer;
  margin-top:10px;
  transition:0.3s;
}

.login-btn:hover{
  background:#4338ca;
}

/* REGISTER TEXT */
.register-text{
  text-align:center;
  margin-top:15px;
  font-size:14px;
}

.register-text a{
  color:#4f46e5;
  text-decoration:none;
  font-weight:500;
}

/* PAGE TRANSITION */
/* PAGE TRANSITION */
/* ADVANCED PAGE TRANSITION */
body {
  opacity: 0;
  transform: translateX(40px) scale(0.98);
  filter: blur(6px);
  transition: 
    opacity 0.4s ease,
    transform 0.4s ease,
    filter 0.4s ease;
}

/* PAGE ENTER */
body.show {
  opacity: 1;
  transform: translateX(0) scale(1);
  filter: blur(0);
}

/* EXIT LEFT */
body.exit-left {
  opacity: 0;
  transform: translateX(-60px) scale(0.96);
  filter: blur(8px);
}

/* EXIT RIGHT */
body.exit-right {
  opacity: 0;
  transform: translateX(60px) scale(0.96);
  filter: blur(8px);
}

* {
  backface-visibility: hidden;
  -webkit-font-smoothing: antialiased;
}


/* CARD ANIMATION */
.login-card {
  opacity: 1;
  transform: translateX(40px) scale(0.95);
  transition: all 0.4s ease;
  transform: translateX(0);
  
}



.login-card.show {
  opacity: 1;
  transform: translateX(0) scale(1);
}

.login-card.exit-left {
  opacity: 0;
  transform: translateX(-60px) scale(0.9);
}

.login-card.exit-right {
  opacity: 0;
  transform: translateX(60px) scale(0.9);
}




