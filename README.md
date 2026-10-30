

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1">

<title>Royal Ace Demo</title>

<style>

*{box-sizing:border-box}

body{margin:0;background:#10131d;color:white;font-family:Arial,sans-serif}

header{background:#191f2d;padding:20px;text-align:center;color:#f5c45e;font-size:27px;font-weight:bold}

.container{max-width:480px;margin:25px auto;padding:18px}

.card{background:#1c2433;padding:22px;border-radius:16px;margin-bottom:18px;border:1px solid #343e50}

h2{color:#f5c45e;margin-top:0}

input{width:100%;padding:14px;margin:8px 0;border:1px solid #46536a;border-radius:9px;background:#101725;color:white;font-size:16px}

button{padding:13px 18px;border:0;border-radius:9px;background:#e6ad42;color:#171717;font-weight:bold;font-size:15px;margin:5px;cursor:pointer}

.secondary{background:#344258;color:white}

.hidden{display:none}

.coin{font-size:30px;color:#f5c45e;font-weight:bold}

small{color:#aeb8ca}

</style>

</head>

<body>

<header>👑 ROYAL ACE</header>

<div class="container">

<div class="card" id="auth">

<h2>Welcome Player</h2>

<div id="register">

<h3>Create Account</h3>

<input id="newname" placeholder="Your name">

<input id="newuser" placeholder="Username">

<input id="newpass" type="password" placeholder="Password">

<button onclick="register()">Register</button>

<p>Already registered?</p>

<button class="secondary" onclick="showLogin()">Login</button>

</div>

<div id="login" class="hidden">

<h3>Player Login</h3>

<input id="username" placeholder="Username">

<input id="password" type="password" placeholder="Password">

<button onclick="login()">Login</button>

<button class="secondary" onclick="showRegister()">Create Account</button>

</div>

</div>

<div class="card hidden" id="player">

<h2>Player Dashboard</h2>

<p>Welcome, <b id="playername"></b></p>

<p>Virtual Coins</p>

<div class="coin" id="balance">0</div>

<button onclick="bonus()">🎁 Daily Bonus</button>

<button class="secondary" onclick="logout()">Logout</button>

<p><small>Virtual entertainment only. Coins have no cash value.</small></p>

</div>

<div class="card hidden" id="admin">

<h2>👑 Admin Dashboard</h2>

<p>Total registered players: <b id="total">0</b></p>

<h3>Registered Users</h3>

<div id="users"></div>

<button class="secondary" onclick="logout()">Logout</button>

</div>

</div>

<script>

let users=JSON.parse(localStorage.getItem("royalUsers")||"[]");

let current=null;

function save(){

 localStorage.setItem("royalUsers",JSON.stringify(users));

}

function showLogin(){

 document.getElementById("register").classList.add("hidden");

 document.getElementById("login").classList.remove("hidden");

}

function showRegister(){

 document.getElementById("login").classList.add("hidden");

 document.getElementById("register").classList.remove("hidden");

}

function register(){

 let name=document.getElementById("newname").value.trim();

 let user=document.getElementById("newuser").value.trim();

 let pass=document.getElementById("newpass").value;

 if(!name||!user||!pass)return alert("Fill all fields");

 if(user.toLowerCase()==="admin")return alert("Username unavailable");

 if(users.some(x=>x.user.toLowerCase()===user.toLowerCase()))

 return alert("Username already exists");

 users.push({name,user,pass,coins:100,lastBonus:""});

 save();

 alert("Account created! Please login.");

 showLogin();

}

function login(){

 let u=document.getElementById("username").value.trim();

 let p=document.getElementById("password").value;

 if(u==="admin"&&p==="Admin123!"){

 document.getElementById("auth").classList.add("hidden");

 document.getElementById("admin").classList.remove("hidden");

 renderUsers();return;

 }

 let found=users.find(x=>x.user===u&&x.pass===p);

 if(!found)return alert("Wrong username or password");

 current=found;

 document.getElementById("auth").classList.add("hidden");

 document.getElementById("player").classList.remove("hidden");

 updatePlayer();

}

function updatePlayer(){

 document.getElementById("playername").textContent=current.name;

 document.getElementById("balance").textContent=current.coins;

}

function bonus(){

 let today=new Date().toDateString();

 if(current.lastBonus===today)return alert("Bonus already collected today");

 current.coins+=50;

 current.lastBonus=today;

 save();updatePlayer();

 alert("50 virtual coins added!");

}

function renderUsers(){

 document.getElementById("total").textContent=users.length;

 document.getElementById("users").innerHTML=users.length?

 users.map(x=>"<p>"+safe(x.name)+" (@"+safe(x.user)+") — "+x.coins+" coins</p>").join(""):

 "<p>No players registered yet.</p>";

}

function safe(s){

 return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));

}

function logout(){

 current=null;

 document.getElementById("player").classList.add("hidden");

 document.getElementById("admin").classList.add("hidden");

 document.getElementById("auth").classList.remove("hidden");

}

</script>

</body>

</html>
