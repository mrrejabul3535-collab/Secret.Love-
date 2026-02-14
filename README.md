<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Secret Love</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap');
        :root { --primary: #ff0055; --dark: #050505; --input-bg: #1a1a1a; --text: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; user-select: none; -webkit-tap-highlight-color: transparent; }
        body { background-color: var(--dark); color: var(--text); height: 100vh; overflow: hidden; display: flex; flex-direction: column; }
        
        .screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 1000; display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--dark); display:none; }
        .app-logo { width: 120px; height: 120px; border-radius: 35px; margin-bottom: 25px; box-shadow: 0 0 30px rgba(255, 0, 85, 0.5); object-fit: cover; border: 3px solid var(--primary); }
        .input-container { width: 85%; margin-bottom: 15px; position: relative; }
        .input-box { background: var(--input-bg); border: 1px solid #333; color: white; padding: 15px 20px; width: 100%; border-radius: 50px; outline: none; font-size: 16px; }
        .btn-primary { background: var(--primary); color: white; border: none; padding: 15px 0; border-radius: 50px; font-size: 18px; font-weight: 600; box-shadow: 0 0 20px var(--primary); cursor: pointer; width: 85%; margin-bottom: 15px; text-align: center; }
        .btn-back { background: var(--input-bg); color: #ccc; border: 1px solid #333; padding: 15px 0; border-radius: 50px; font-size: 18px; width: 85%; cursor: pointer; }
        .fade-in { animation: fadeInAnimation 1s ease forwards; opacity: 0; }
        @keyframes fadeInAnimation { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        #main-app { display: none; height: 100%; flex-direction: column; width: 100%; }
        header { padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; background: #0a0a0a; border-bottom: 1px solid #222; }
        .user-thumb { width: 45px; height: 45px; border-radius: 50%; object-fit: cover; border: 2px solid var(--primary); }
        .partner-list { flex: 1; padding: 20px; overflow-y: auto; }
        .partner-item { background: #111; padding: 15px; border-radius: 20px; display: flex; align-items: center; margin-bottom: 12px; border: 1px solid #222; }
        
        #chat-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--dark); z-index: 2000; display: none; flex-direction: column; }
        .chat-body { flex: 1; padding: 20px; padding-bottom: 140px; overflow-y: auto; display: flex; flex-direction: column; gap: 15px; }
        .message { max-width: 75%; padding: 12px 18px; border-radius: 20px; font-size: 15px; word-wrap: break-word; }
        .my-msg { align-self: flex-end; background: var(--primary); color: white; border-bottom-right-radius: 4px; }
        .other-msg { align-self: flex-start; background: #222; color: white; border-bottom-left-radius: 4px; border: 1px solid #333; }
        .chat-footer { position: fixed; bottom: 0; left: 0; width: 100%; background: #0a0a0a; border-top: 1px solid #222; display: flex; flex-direction: column; }
        .chat-input-area { padding: 10px; display: flex; align-items: center; gap: 10px; }
        .chat-input { flex: 1; background: #222; border: none; padding: 12px; border-radius: 30px; color: white; outline: none; }
        .send-btn { background: var(--primary); border: none; color: white; width: 45px; height: 45px; border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center; }
        
        .modal { background: rgba(0,0,0,0.95); z-index: 3000; display: none; position: fixed; top:0; left:0; width:100%; height:100%; align-items:center; justify-content:center; }
        .modal-content { background: #111; width: 85%; padding: 30px; border-radius: 25px; text-align: center; border: 1px solid #333; position: relative; }
        .id-box { background: #111; border: 2px solid var(--primary); padding: 20px; border-radius: 15px; margin: 30px 0; font-family: monospace; font-size: 20px; color: var(--primary); text-align: center; width: 85%; }
    </style>
</head>
<body>

    <div id="splash-screen" class="screen" style="display: flex;">
        <img src="https://cdn-icons-png.flaticon.com/512/2107/2107952.png" class="app-logo">
        <h1>Secret Love</h1>
        <button id="splash-btn" class="btn-primary fade-in" style="animation-delay: 1.5s; display: none;" onclick="window.toLogin()">Get Started</button>
    </div>

    <div id="login-screen" class="screen">
        <h2 style="color: var(--primary);">Welcome</h2>
        <div class="input-container"><input type="email" id="email" class="input-box" placeholder="Email"></div>
        <div class="input-container">
            <input type="password" id="password" class="input-box" placeholder="Password">
            <i class="fas fa-eye" style="position: absolute; right: 20px; top: 50%; transform: translateY(-50%); color: #888; cursor: pointer;" onclick="window.togglePass()"></i>
        </div>
        <div style="display: flex; gap: 10px; width: 85%; margin-bottom: 15px;">
            <button class="btn-primary" style="flex: 1; margin: 0;" onclick="window.doLogin()">Login</button>
            <button class="btn-primary" style="flex: 1; margin: 0; background: transparent; border: 2px solid var(--primary);" onclick="window.doRegister()">Register</button>
        </div>
        <p style="color:#888; margin-bottom:20px; font-size:14px; cursor:pointer;" onclick="window.resetPassword()">Forgot Password?</p>
        <button class="btn-back" onclick="document.getElementById('login-screen').style.display='none'; document.getElementById('splash-screen').style.display='flex'">Back</button>
    </div>

    <div id="setup-screen" class="screen">
        <h2 style="color: var(--primary);">Setup Profile</h2>
        <div style="width:110px; height:110px; border-radius:50%; background:#222; margin-bottom:20px; overflow:hidden; border:3px solid var(--primary);" onclick="document.getElementById('setup-file').click()">
            <img id="setup-img" src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" style="width:100%; height:100%; object-fit:cover;">
        </div>
        <input type="file" id="setup-file" hidden accept="image/*" onchange="window.previewFile()">
        <div class="input-container"><input type="text" id="setup-name" class="input-box" placeholder="Enter Your Name"></div>
        <button class="btn-primary" onclick="window.finishSetup()">Start</button>
    </div>

    <div id="congrats-screen" class="screen">
        <h2 style="color: var(--primary); font-size: 28px;">Congratulations 🎉</h2>
        <p style="color: #ccc;">Your account is ready.</p>
        <div class="id-box" id="generated-id-display">Loading...</div>
        <button class="btn-primary" onclick="window.enterApp()">Next</button>
    </div>

    <div id="main-app">
        <header>
            <h2 style="color: var(--primary);">Secret Love</h2>
            <img id="header-avatar" src="" class="user-thumb">
        </header>
        <div class="partner-list" id="partner-list"></div>
        <div class="fab" onclick="document.getElementById('add-modal').style.display='flex'">+</div>
    </div>

    <div id="add-modal" class="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="document.getElementById('add-modal').style.display='none'">&times;</span>
            <h3>Add Partner</h3><br>
            <input type="text" id="pid-input" class="input-box" placeholder="Paste ID">
            <button class="btn-primary" onclick="window.addPartner()">CONNECT</button>
        </div>
    </div>

    <div id="chat-screen">
        <div class="chat-footer" style="top:0; bottom:auto; height:60px; justify-content:center; padding:0 20px; flex-direction:row; align-items:center; border-bottom:1px solid #333; border-top:none;">
            <i class="fas fa-arrow-left" style="font-size:24px; margin-right:15px;" onclick="window.closeChat()"></i>
            <h3 id="chat-name" style="flex:1;">Partner</h3>
        </div>
        <div class="chat-body" id="chat-body" style="padding-top:80px;"></div>
        <div class="chat-footer">
            <div class="chat-input-area">
                <input type="text" id="msg-input" class="chat-input" placeholder="Type a message...">
                <button class="send-btn" onclick="window.sendTxtMsg()"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut, sendPasswordResetEmail } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
        import { getDatabase, ref, set, get, push, onChildAdded } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

        const firebaseConfig = {
          apiKey: "AIzaSyDSzTThXe9Nw1RG1zvfEWwkI5frlNOHYzA",
          authDomain: "secret-love-c8d57.firebaseapp.com",
          databaseURL: "https://secret-love-c8d57-default-rtdb.firebaseio.com",
          projectId: "secret-love-c8d57",
          storageBucket: "secret-love-c8d57.firebasestorage.app",
          messagingSenderId: "249630233117",
          appId: "1:249630233117:web:7e3b3380d05deb59272cf4"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getDatabase(app);
        let myID = localStorage.getItem('sl_id');
        let currentChatPartner = null;
        let activeChatRef = null;

        window.onload = function() { setTimeout(() => { if(document.getElementById('splash-btn')) document.getElementById('splash-btn').style.display = 'block'; }, 1500); }
        window.toLogin = function() { document.getElementById('splash-screen').style.display = 'none'; document.getElementById('login-screen').style.display = 'flex'; }

        // --- AUTH ---
        window.doLogin = function() {
            const e = document.getElementById("email").value;
            const p = document.getElementById("password").value;
            if(!e || !p) { alert("Please enter Email & Password"); return; }
            const btn = event.target; btn.innerText = "Processing...";
            
            signInWithEmailAndPassword(auth, e, p)
                .then((userCredential) => { checkUserAndGo(userCredential.user); })
                .catch((error) => { btn.innerText = "Login"; alert("Login Failed: " + error.message); });
        }

        window.doRegister = function() {
            const e = document.getElementById("email").value;
            const p = document.getElementById("password").value;
            if(!e || !p) { alert("Email & Password required!"); return; }
            
            createUserWithEmailAndPassword(auth, e, p)
                .then(() => {
                    alert("Account Created! Setup Profile now.");
                    document.querySelectorAll('.screen').forEach(s => s.style.display='none');
                    document.getElementById('setup-screen').style.display = 'flex';
                })
                .catch((error) => { alert(error.message); });
        }

        window.resetPassword = function() {
            const email = document.getElementById("email").value;
            if (!email) { alert("Enter email first!"); return; }
            sendPasswordResetEmail(auth, email).then(() => alert("Check Spam folder!")).catch((e) => alert(e.message));
        }

        async function checkUserAndGo(user) {
            const userRef = ref(db, "users/" + user.uid);
            try {
                const snapshot = await get(userRef);
                if(snapshot.exists()) {
                    myID = snapshot.val().secretId;
                    localStorage.setItem('sl_id', myID);
                    window.enterApp();
                } else {
                    document.querySelectorAll('.screen').forEach(s => s.style.display='none');
                    document.getElementById('setup-screen').style.display = 'flex';
                }
            } catch (error) {
                alert("Database Locked! Go to Firebase Console -> Realtime Database -> Rules -> Set read/write to true.");
                document.getElementById('login-screen').querySelector('button').innerText = "Login";
            }
        }

        window.finishSetup = async function() {
            let name = document.getElementById('setup-name').value;
            if(!name) return alert("Name required");
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = 'SL-'; for ( let i = 0; i < 8; i++ ) result += chars.charAt(Math.floor(Math.random() * chars.length));
            myID = result;
            const user = auth.currentUser;
            if(user) {
                await set(ref(db, "users/"+user.uid), { email: user.email, secretId: myID, name: name });
                document.getElementById('generated-id-display').innerText = myID;
                document.querySelectorAll('.screen').forEach(s => s.style.display='none');
                document.getElementById('congrats-screen').style.display = 'flex';
            }
        }

        window.enterApp = function() {
            document.querySelectorAll('.screen').forEach(s => s.style.display='none');
            document.getElementById('main-app').style.display = 'flex';
            document.getElementById('header-avatar').src = "https://cdn-icons-png.flaticon.com/512/3135/3135715.png";
            renderPartners();
        }

        window.previewFile = function() {
            let file = document.getElementById('setup-file').files[0];
            if(file) { let reader = new FileReader(); reader.onload = e => document.getElementById('setup-img').src = e.target.result; reader.readAsDataURL(file); }
        }
        window.togglePass = function() { let x = document.getElementById("password"); x.type = x.type === "password" ? "text" : "password"; }

        // --- CHAT LOGIC ---
        function renderPartners() {
            let list = document.getElementById('partner-list'); list.innerHTML = "";
            let partners = JSON.parse(localStorage.getItem('sl_partners') || '[]');
            partners.forEach(p => {
                let div = document.createElement('div'); div.className = 'partner-item';
                div.innerHTML = `<img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" style="width:40px; margin-right:10px;"><div><b>${p.id}</b></div>`;
                div.onclick = () => window.openChat(p); list.appendChild(div);
            });
        }
        window.addPartner = function() {
            let pid = document.getElementById('pid-input').value.trim(); if(!pid) return;
            let partners = JSON.parse(localStorage.getItem('sl_partners') || '[]'); partners.push({ id: pid });
            localStorage.setItem('sl_partners', JSON.stringify(partners));
            document.getElementById('add-modal').style.display='none'; renderPartners();
        }
        window.openChat = function(p) {
            currentChatPartner = p; document.getElementById('chat-name').innerText = p.id;
            document.getElementById('chat-screen').style.display = 'flex'; document.getElementById('chat-body').innerHTML = "";
            if(activeChatRef) activeChatRef.off();
            let chatID = myID < p.id ? myID + "_" + p.id : p.id + "_" + myID;
            activeChatRef = ref(db, 'chats/' + chatID);
            onChildAdded(activeChatRef, (snapshot) => {
                let data = snapshot.val(); let div = document.createElement('div');
                div.className = 'message ' + (data.sender === myID ? 'my-msg' : 'other-msg');
                div.innerText = data.content; document.getElementById('chat-body').appendChild(div);
            });
        }
        window.sendTxtMsg = function() {
            let txt = document.getElementById('msg-input').value; if(!txt) return;
            let chatID = myID < currentChatPartner.id ? myID + "_" + currentChatPartner.id : currentChatPartner.id + "_" + myID;
            push(ref(db, 'chats/' + chatID), { sender: myID, content: txt }); document.getElementById('msg-input').value = "";
        }
        window.closeChat = function() { document.getElementById('chat-screen').style.display = 'none'; }
    </script>
</body>
</html>
