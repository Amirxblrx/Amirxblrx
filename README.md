<!DOCTYPE html>
<html lang="ar"fr">
<head>
<meta charset="UTF-8">
<title>   chahin mkadmi</title>
<style>
  body { font-family: Arial; background: #f0f2f5; margin:0; padding:0; }
  header { background:#1877f2; color:white; padding:15px; text-align:center; font-size:22px; }
  .container { width:95%; max-width:600px; margin:20px auto; }
  .card { background:white; padding:15px; margin-bottom:15px; border-radius:10px; box-shadow:0 2px 5px rgba(0,0,0,0.1);}
  input, textarea { width:100%; padding:10px; margin:5px 0; border-radius:8px; border:1px solid #ccc;}
  button { padding:10px 15px; border:none; border-radius:8px; cursor:pointer; font-weight:bold;}
  .btn-primary { background:#1877f2; color:white;}
  .btn-primary:hover { background:#145dbf;}
  .btn-like { background:#e4e6eb; margin-right:5px;}
  .btn-like:hover { background:#d8dadf;}
  .username { font-weight:bold; color:#1877f2; }
  .time { font-size:12px; color:gray;}
  .comment { padding:5px; border-left:2px solid #1877f2; margin-top:5px; background:#f0f2f5; border-radius:5px;}
</style>
</head>
<body>
<header >   MGADMI GROUPE    </header>

<div class="container" id="appSection">
  <div class="card">
    <p>اكتب اسمك 👤:</p>
    <input type="text" id="username" placeholder="اسمك">
    <textarea id="postContent" rows="3" placeholder="💬 بماذا تفكر؟"></textarea>
    <button class="btn-primary" onclick="addPost()">نشر</button>
  </div>
  <div id="posts"></div>
</div>

<script>
let posts = JSON.parse(localStorage.getItem("posts")) || [];

function addPost() {
  const user = document.getElementById("username").value.trim();
  const text = document.getElementById("postContent").value.trim();
  if (!user || !text) return alert("⚠️ اكتب اسمك والمنشور");

  posts.unshift({
    user: user,
    text: text,
    time: new Date().toLocaleString(),
    likes: 0,
    comments: []
  });
  localStorage.setItem("posts", JSON.stringify(posts));
  renderPosts();
  document.getElementById("postContent").value = "";
}

function likePost(index) {
  posts[index].likes++;
  localStorage.setItem("posts", JSON.stringify(posts));
  renderPosts();
}

function addComment(index) {
  const text = prompt("✍️ اكتب تعليقك:");
  if (!text) return;
  posts[index].comments.push({ user: document.getElementById("username").value, text });
  localStorage.setItem("posts", JSON.stringify(posts));
  renderPosts();
}

function renderPosts() {
  const postsDiv = document.getElementById("posts");
  postsDiv.innerHTML = "";
  posts.forEach((p,index)=>{
    const postEl = document.createElement("div");
    postEl.classList.add("card");
    postEl.innerHTML = `
      <span class="username">${p.user}</span> • <span class="time">${p.time}</span><br>
      ${p.text}
      <div style="margin-top:10px;">
        <button class="btn-like" onclick="likePost(${index})">👍 إعجاب (${p.likes})</button>
        <button class="btn-like" onclick="addComment(${index})">💬 تعليق</button>
      </div>
      ${p.comments.map(c=>`<div class="comment"><b>${c.user}:</b> ${c.text}</div>`).join("")}
    `;
    postsDiv.appendChild(postEl);
  });
}

renderPosts();
</script>
</body>
</html>
