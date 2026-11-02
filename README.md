
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FaceWorld Social</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f0f2f5;font-family:Arial,sans-serif;color:#1c1e21}
header{background:#1877f2;color:white;padding:16px;position:sticky;top:0}
header h2{margin:0;font-size:25px}
header p{margin:5px 0 0;font-size:12px}
nav{display:flex;justify-content:space-around;background:white;padding:15px 3px;position:sticky;top:65px;box-shadow:0 2px 4px #ddd}
nav button{border:0;background:none;color:#1877f2;font-weight:bold;font-size:12px}
main{max-width:600px;margin:auto;padding:12px}
.card{background:white;padding:16px;border-radius:12px;margin-bottom:14px;box-shadow:0 1px 3px #ddd}
input,textarea{width:100%;padding:12px;border:1px solid #ddd;border-radius:9px;margin:7px 0;font:inherit}
textarea{height:85px;resize:vertical}
.primary{background:#1877f2;color:white;border:0;padding:12px 20px;border-radius:8px;font-weight:bold}
.post-name{font-weight:bold}
.muted{color:#777;font-size:12px;margin-top:5px}
.post-body{white-space:pre-wrap;line-height:1.5}
.actions{display:flex;gap:10px;margin-top:14px}
.actions button{flex:1;padding:11px;border:0;border-radius:8px;background:#f0f2f5;color:#1877f2;font-weight:bold}
.hidden{display:none}
footer{text-align:center;color:#777;padding:20px;font-size:12px}
</style>
</head>
<body>
<header>
<h2>🌍 FaceWorld Social</h2>
<p>Connect • Share • Engage</p>
</header>
<nav>
<button onclick="page('home')">🏠 Home</button>
<button onclick="page('friends')">👥 Friends</button>
<button onclick="page('messages')">💬 Messages</button>
<button onclick="page('profile')">👤 Profile</button>
</nav>
<main>
<section id="home">
<div class="card">
<h3>What's on your mind?</h3>
<textarea id="postText" placeholder="Share something with friends..."></textarea>
<button class="primary" onclick="publish()">Publish Post</button>
<p id="notice" class="muted"></p>
</div>
<h3>News Feed</h3>
<div id="feed"></div>
</section>
<section id="friends" class="hidden">
<div class="card"><h3>👥 Friends</h3><p>Find and connect with friends on FaceWorld.</p></div>
</section>
<section id="messages" class="hidden">
<div class="card"><h3>💬 Messages</h3><p>Your conversations will appear here.</p></div>
</section>
<section id="profile" class="hidden">
<div class="card">
<h3>👤 My Profile</h3>
<input id="username" placeholder="Your name">
<button class="primary" onclick="saveName()">Save Profile</button>
<p id="profileNotice"></p>
</div>
</section>
</main>
<footer>FaceWorld Social © 2026</footer>
<script>
let posts=JSON.parse(localStorage.getItem('fw_posts')||'[]');
function publish(){
let input=document.getElementById('postText');
let value=input.value.trim();
if(!value){document.getElementById('notice').textContent='Write something first!';return;}
posts.unshift({id:Date.now(),text:value,likes:0,comments:[]});
input.value='';
document.getElementById('notice').textContent='Post published!';
save();render();
}
function save(){localStorage.setItem('fw_posts',JSON.stringify(posts));}
function like(id){
let post=posts.find(p=>p.id===id);
post.likes++;
save();render();
}
function comment(id){
let input=document.getElementById('comment'+id);
if(!input.value.trim())return;
let post=posts.find(p=>p.id===id);
post.comments.push(input.value.trim());
save();render();
}
function render(){
let feed=document.getElementById('feed');
if(!posts.length){feed.innerHTML='<div class="card">Welcome to FaceWorld! Be the first to share something. 🌍</div>';return;}
feed.innerHTML=posts.map(p=>`
<div class="card">
<div class="post-name">🌍 FaceWorld Member</div>
<div class="muted">Community post</div>
<p class="post-body">${escapeHTML(p.text)}</p>
<div class="muted">${p.likes} likes · ${p.comments.length} comments</div>
<div class="actions">
<button onclick="like(${p.id})">👍 Like</button>
<button onclick="document.getElementById('comment${p.id}').focus()">💬 Comment</button>
</div>
${p.comments.map(c=>'<p class="muted">💬 '+escapeHTML(c)+'</p>').join('')}
<input id="comment${p.id}" placeholder="Write a comment...">
<button class="primary" onclick="comment(${p.id})">Send</button>
</div>`).join('');
}
function escapeHTML(s){
return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function page(id){
['home','friends','messages','profile'].forEach(x=>document.getElementById(x).classList.toggle('hidden',x!==id));
}
function saveName(){
let name=document.getElementById('username').value.trim();
if(name){localStorage.setItem('fw_name',name);document.getElementById('profileNotice').textContent='Profile saved!';}
}
document.getElementById('username').value=localStorage.getItem('fw_name')||'';
render();
</script>
</body>
</html>
