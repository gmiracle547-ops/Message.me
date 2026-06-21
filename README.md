<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
  <title>Connect · social</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" />
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', -apple-system, system-ui, sans-serif; }
    html, body { width: 100%; height: 100%; background: #0b1420; display: flex; justify-content: center; align-items: center; }
    .phone {
      width: 100%; height: 100%; max-width: 430px; max-height: 932px;
      background: #f0f4fa; display: flex; flex-direction: column;
      box-shadow: 0 0 40px rgba(0,0,0,0.6); overflow: hidden; position: relative;
    }
    @media (min-aspect-ratio: 9/16) { .phone { border-radius: 36px; height: 90vh; width: 40vh; } }

    ::-webkit-scrollbar { width: 4px; }
    ::-webkit-scrollbar-thumb { background: #c5d6ea; border-radius: 10px; }

    .auth-screen {
      flex: 1; display: flex; flex-direction: column; justify-content: center;
      padding: 30px 24px; background: #f5f9ff; gap: 14px;
    }
    .auth-screen h2 { font-size: 2rem; font-weight: 600; color: #142a41; }
    .auth-screen .sub { color: #3e5f7e; margin-bottom: 4px; }
    .auth-input {
      background: white; border: 1px solid #dae3ef; border-radius: 60px;
      padding: 16px 20px; font-size: 1rem; width: 100%; outline: none;
    }
    .auth-input:focus { border-color: #1f5a85; box-shadow: 0 0 0 3px rgba(30,90,150,0.1); }
    .auth-btn {
      background: #1a3f5e; border: none; color: white; font-weight: 600;
      padding: 16px; border-radius: 60px; font-size: 1.05rem; cursor: pointer;
      transition: 0.15s; width: 100%;
    }
    .auth-btn:active { background: #0f2e45; }
    .auth-btn-outline { background: transparent; border: 1px solid #b8cee5; color: #1a3f5e; }
    .auth-toggle { text-align: center; color: #2b5b81; font-weight: 500; cursor: pointer; margin-top: 4px; }
    .auth-error { color: #b84a4a; font-size: 0.9rem; padding-left: 8px; min-height: 24px; }
    .hidden { display: none !important; }

    .app-container { flex: 1; display: none; flex-direction: column; height: 100%; background: #f0f4fa; }
    .app-header {
      padding: 12px 16px 8px 16px; background: #f0f4fa;
      border-bottom: 1px solid rgba(180,200,220,0.2);
      display: flex; justify-content: space-between; align-items: center; flex-shrink: 0;
    }
    .app-header h1 { font-size: 1.4rem; font-weight: 650; color: #142a41; display: flex; align-items: center; gap: 6px; }
    .app-header h1 i { color: #2a6f9c; }
    .header-actions { display: flex; gap: 12px; align-items: center; }
    .header-actions button { background: transparent; border: none; font-size: 1.2rem; color: #3d5f7f; cursor: pointer; }

    .verify-banner {
      background: #ffedd5; padding: 8px 16px; display: flex; justify-content: space-between;
      align-items: center; border-bottom: 1px solid #f0d5a0; flex-shrink: 0;
    }
    .verify-banner span { font-size: 0.85rem; color: #8a6d3b; }
    .verify-banner button { background: #1a3f5e; color: white; border: none; padding: 4px 16px; border-radius: 60px; font-weight: 500; cursor: pointer; }

    .main-content { flex: 1; overflow-y: auto; padding: 10px 14px 8px 14px; display: flex; flex-direction: column; gap: 14px; }
    .page-section { display: none; flex-direction: column; gap: 14px; animation: fade 0.15s ease; }
    .page-section.active { display: flex; }
    @keyframes fade { 0% { opacity: 0.5; transform: translateY(4px); } 100% { opacity: 1; transform: translateY(0); } }

    .card {
      background: white; border-radius: 20px; padding: 14px 16px;
      border: 1px solid rgba(210,225,245,0.5); box-shadow: 0 2px 6px rgba(0,0,0,0.02);
    }
    .card-title { font-weight: 600; color: #1d334a; font-size: 1rem; margin-bottom: 10px; display: flex; align-items: center; gap: 8px; }
    .card-title i { color: #2a6f9c; width: 22px; }

    .post-item {
      background: white; border-radius: 20px; padding: 14px 16px; margin-bottom: 12px;
      border: 1px solid #e8eef6; box-shadow: 0 1px 4px rgba(0,0,0,0.02);
    }
    .post-header { display: flex; align-items: center; gap: 10px; margin-bottom: 6px; }
    .post-avatar {
      width: 40px; height: 40px; background: #dce7f5; border-radius: 40px;
      display: flex; align-items: center; justify-content: center; color: #1f4770; font-weight: 600;
    }
    .post-user { font-weight: 600; color: #142a41; }
    .post-time { font-size: 0.7rem; color: #6a85a3; }
    .post-content { margin: 6px 0 10px 0; word-break: break-word; color: #1b2c3f; }
    .post-media { margin: 8px 0; border-radius: 16px; overflow: hidden; max-height: 260px; background: #eef3fa; display: flex; align-items: center; justify-content: center; }
    .post-media img, .post-media video { width: 100%; max-height: 260px; object-fit: cover; }
    .post-actions { display: flex; gap: 16px; border-top: 1px solid #eef3fb; padding-top: 10px; margin-top: 4px; }
    .post-actions button {
      background: transparent; border: none; font-size: 0.85rem; color: #4f6f8f;
      display: flex; align-items: center; gap: 6px; cursor: pointer; padding: 4px 8px; border-radius: 30px;
      transition: 0.1s;
    }
    .post-actions button:active { background: #e8f0fa; }
    .post-actions .liked { color: #1a6bb0; }
    .post-actions .liked i { font-weight: 900; }

    .comment-section { margin-top: 10px; border-top: 1px solid #eef3fb; padding-top: 8px; }
    .comment-item { display: flex; gap: 8px; padding: 4px 0; font-size: 0.9rem; }
    .comment-item strong { color: #1a3f5e; }
    .comment-input-group { display: flex; gap: 6px; margin-top: 6px; }
    .comment-input-group input { flex:1; padding: 8px 14px; border-radius: 60px; border: 1px solid #d4dfee; background: white; outline: none; font-size: 0.85rem; }
    .comment-input-group button { background: #1a3f5e; color: white; border: none; padding: 6px 16px; border-radius: 60px; cursor: pointer; }

    .friend-item, .request-item {
      display: flex; justify-content: space-between; align-items: center;
      padding: 8px 4px 8px 10px; border-bottom: 1px solid #eef3fb;
    }
    .friend-item:last-child, .request-item:last-child { border-bottom: none; }
    .friend-info { display: flex; align-items: center; gap: 10px; }
    .friend-avatar {
      width: 36px; height: 36px; background: #dce7f5; border-radius: 40px;
      display: flex; align-items: center; justify-content: center; color: #1f4770; font-weight: 500;
    }
    .btn-sm {
      padding: 4px 14px; border-radius: 60px; border: none; font-weight: 500; font-size: 0.7rem;
      cursor: pointer; background: #eaf1fb; color: #1a4163; display: inline-flex; align-items: center; gap: 4px;
    }
    .btn-sm.primary { background: #1a3f5e; color: white; }
    .btn-sm.danger { background: #fee9e9; color: #b14141; }
    .btn-sm.success { background: #dff0e6; color: #1d6b4a; }
    .input-group { display: flex; gap: 8px; margin-top: 4px; }
    .input-group input { flex:1; padding: 10px 14px; border-radius: 60px; border: 1px solid #d4dfee; background: white; font-size: 0.9rem; outline: none; }
    .input-group input:focus { border-color: #3a7bb0; box-shadow: 0 0 0 3px rgba(50,110,180,0.08); }
    .btn { background: white; border: 1px solid #d0ddeb; padding: 8px 16px; border-radius: 60px; font-weight: 500; color: #1b3a57; cursor: pointer; font-size: 0.85rem; display: inline-flex; align-items: center; gap: 6px; }
    .btn-primary { background: #1a3f5e; border: 1px solid #1a3f5e; color: white; }

    .message-item {
      background: white; border-radius: 20px 20px 20px 6px; padding: 8px 14px; margin-bottom: 4px;
      border-left: 4px solid #3a7bb0; display: flex; justify-content: space-between; align-items: center;
    }
    .message-sender { font-weight: 600; color: #1b3b58; min-width: 60px; font-size: 0.9rem; }
    .message-text { flex:1; padding: 0 8px; color: #1b2c3f; font-size: 0.9rem; }
    .message-time { font-size: 0.6rem; background: #edf3fa; padding: 2px 10px; border-radius: 60px; color: #526f8f; }
    .empty-state { color: #6a7f9b; padding: 20px 0; text-align: center; font-style: italic; background: rgba(255,255,255,0.3); border-radius: 60px; font-size: 0.9rem; }

    .video-grid { display: flex; flex-direction: column; gap: 10px; }
    .video-card { background: white; border-radius: 20px; padding: 10px 14px; display: flex; align-items: center; gap: 12px; border: 1px solid rgba(200,215,235,0.4); }
    .video-thumb { width: 60px; height: 60px; background: #dce7f5; border-radius: 16px; display: flex; align-items: center; justify-content: center; color: #1d4b74; font-size: 1.6rem; }
    .upload-btn-big {
      background: #1a3f5e; color: white; border: none; padding: 14px; border-radius: 60px;
      font-size: 1.1rem; font-weight: 600; display: flex; align-items: center; justify-content: center; gap: 10px;
      cursor: pointer; width: 100%; margin: 8px 0;
    }
    .upload-btn-big:active { background: #0f2e45; }

    .bottom-nav {
      background: #ffffffdd; backdrop-filter: blur(16px); border-top: 1px solid rgba(180,200,220,0.3);
      display: flex; justify-content: space-around; padding: 6px 6px 12px 6px; flex-shrink: 0;
    }
    .nav-item {
      display: flex; flex-direction: column; align-items: center; background: transparent; border: none;
      color: #6b85a3; font-size: 0.6rem; font-weight: 500; gap: 1px; cursor: pointer; padding: 4px 10px; border-radius: 40px;
      transition: 0.1s;
    }
    .nav-item i { font-size: 1.4rem; }
    .nav-item.active { color: #1d4b77; background: #e3edfa; }
    .nav-item:active { transform: scale(0.92); }

    .modal-overlay {
      position: absolute; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.5);
      display: none; align-items: center; justify-content: center; z-index: 999; backdrop-filter: blur(4px);
    }
    .modal-overlay.show { display: flex; }
    .modal-box {
      background: white; border-radius: 28px; padding: 24px 20px; width: 90%; max-width: 360px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    .modal-box h3 { margin-bottom: 12px; color: #142a41; }
    .modal-box .friend-item { padding: 6px 0; cursor: pointer; border-bottom: 1px solid #eef3fb; }
    .modal-box .friend-item:active { background: #f0f6fe; }
    .modal-close { float: right; background: none; border: none; font-size: 1.4rem; color: #6b85a3; cursor: pointer; }

    .upload-modal .modal-box { padding: 20px; }
    .upload-modal textarea { width: 100%; padding: 12px; border-radius: 16px; border: 1px solid #d4dfee; resize: vertical; min-height: 80px; font-size: 0.95rem; outline: none; }
    .upload-modal input[type="file"] { margin: 10px 0; }
  </style>
</head>
<body>
<div class="phone" id="app">

  <!-- LOGIN SCREEN -->
  <div class="auth-screen" id="loginScreen">
    <h2><i class="fas fa-connectdevelop" style="color:#1f5a85;"></i> Connect</h2>
    <div class="sub">Sign in to your account</div>
    <input class="auth-input" id="loginUsername" placeholder="Username or Email" />
    <input class="auth-input" id="loginPassword" type="password" placeholder="Password" />
    <div id="loginError" class="auth-error"></div>
    <button class="auth-btn" id="loginBtn">Login</button>
    <div class="auth-toggle" id="gotoRegister">Don't have an account? <strong>Register</strong></div>
  </div>

  <!-- REGISTER SCREEN -->
  <div class="auth-screen hidden" id="registerScreen">
    <h2><i class="fas fa-user-plus" style="color:#1f5a85;"></i> Register</h2>
    <div class="sub">Create your account</div>
    <input class="auth-input" id="regUsername" placeholder="Username" />
    <input class="auth-input" id="regDob" placeholder="Date of Birth (YYYY-MM-DD)" />
    <input class="auth-input" id="regEmail" placeholder="Email or Phone" />
    <input class="auth-input" id="regPassword" type="password" placeholder="Password" />
    <div id="regError" class="auth-error"></div>
    <button class="auth-btn" id="regBtn">Register</button>
    <div class="auth-toggle" id="gotoLogin">Already have an account? <strong>Login</strong></div>
  </div>

  <!-- VERIFY MODAL -->
  <div class="modal-overlay" id="verifyModal">
    <div class="modal-box">
      <h3><i class="fas fa-envelope"></i> Verify Email</h3>
      <p style="font-size:0.9rem; color:#3e5f7e; margin-bottom:10px;">Enter the 6-digit code sent to your email.</p>
      <input class="auth-input" id="verifyCodeInput" placeholder="Code" style="margin-bottom:10px;" />
      <div id="verifyError" class="auth-error"></div>
      <button class="auth-btn" id="verifyBtn">Verify</button>
      <button class="auth-btn auth-btn-outline" id="resendCodeBtn" style="background:transparent; border:1px solid #b8cee5; color:#1a3f5e; margin-top:6px;">Resend Code</button>
    </div>
  </div>

  <!-- SHARE MODAL -->
  <div class="modal-overlay" id="shareModal">
    <div class="modal-box">
      <button class="modal-close" id="shareModalClose"><i class="fas fa-times"></i></button>
      <h3><i class="fas fa-share-alt"></i> Share</h3>
      <div id="shareFriendList"><div class="empty-state">No friends to share with</div></div>
      <button class="btn" id="copyLinkBtn" style="width:100%; justify-content:center; margin-top:8px;"><i class="fas fa-copy"></i> Copy link</button>
    </div>
  </div>

  <!-- POST UPLOAD MODAL -->
  <div class="modal-overlay upload-modal" id="uploadModal">
    <div class="modal-box">
      <button class="modal-close" id="uploadModalClose"><i class="fas fa-times"></i></button>
      <h3><i class="fas fa-plus-circle"></i> Create Post</h3>
      <textarea id="postCaption" placeholder="What's on your mind?"></textarea>
      <input type="file" id="postFile" accept="image/*,video/*" />
      <button class="auth-btn" id="postSubmitBtn" style="margin-top:10px;">Post</button>
    </div>
  </div>

  <!-- MAIN APP -->
  <div class="app-container" id="appContainer">
    <div class="app-header">
      <h1><i class="fas fa-connectdevelop"></i> Connect</h1>
      <div class="header-actions">
        <button id="postCreateBtn"><i class="fas fa-plus-circle"></i></button>
        <button id="logoutBtn"><i class="fas fa-sign-out-alt"></i></button>
      </div>
    </div>

    <div class="verify-banner hidden" id="verifyBanner">
      <span><i class="fas fa-exclamation-triangle"></i> Verify your email to access all features.</span>
      <button id="verifyBannerBtn">Verify</button>
    </div>

    <div class="main-content" id="mainContent">
      <!-- HOME -->
      <div class="page-section active" id="pageHome">
        <div id="postsContainer"></div>
      </div>

      <!-- FIND -->
      <div class="page-section" id="pageFind">
        <div class="card">
          <div class="card-title"><i class="fas fa-search"></i> Find friends</div>
          <div class="input-group">
            <input type="text" id="findInput" placeholder="Username" />
            <button class="btn btn-primary" id="findAddBtn"><i class="fas fa-user-plus"></i> Add</button>
          </div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-user-plus"></i> Requests</div>
          <div id="requestsContainer"><div class="empty-state">No requests</div></div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-list-ul"></i> All users</div>
          <div id="allUsersContainer"><div class="empty-state">No users</div></div>
        </div>
      </div>

      <!-- MESSAGES -->
      <div class="page-section" id="pageMessages">
        <div class="card">
          <div class="card-title"><i class="fas fa-comment-dots"></i> Chats</div>
          <div id="messageThread" style="max-height:300px; overflow-y:auto; display:flex; flex-direction:column; gap:4px;"></div>
          <div class="input-group" style="margin-top:10px;">
            <select id="msgRecipient" style="flex:0.7; min-width:80px; padding:8px 10px; border-radius:60px; border:1px solid #d4dfee; background:white; outline:none;"><option value="">— friend —</option></select>
            <input type="text" id="msgInput" placeholder="Type..." style="flex:1; padding:8px 14px; border-radius:60px; border:1px solid #d4dfee; outline:none;" />
            <button class="btn btn-primary" id="msgSendBtn"><i class="fas fa-paper-plane"></i></button>
          </div>
        </div>
      </div>

      <!-- VIDEOS -->
      <div class="page-section" id="pageVideos">
        <div class="card">
          <div class="card-title"><i class="fas fa-video"></i> Videos</div>
          <div id="videoList" class="video-grid"><div class="empty-state">No videos yet</div></div>
          <button class="upload-btn-big" id="uploadVideoBtn"><i class="fas fa-cloud-upload-alt"></i> Upload video</button>
        </div>
      </div>
    </div>

    <div class="bottom-nav">
      <button class="nav-item active" data-page="pageHome"><i class="fas fa-home"></i><span>Home</span></button>
      <button class="nav-item" data-page="pageFind"><i class="fas fa-search"></i><span>Find</span></button>
      <button class="nav-item" data-page="pageMessages"><i class="fas fa-comment-dots"></i><span>Messages</span></button>
      <button class="nav-item" data-page="pageVideos"><i class="fas fa-film"></i><span>Videos</span></button>
    </div>
  </div>
</div>

<script>
  (function() {
    // ----- DATA -----
    let users = [];
    let currentUser = null;
    let friends = [];
    let friendRequests = [];
    let messages = [];
    let posts = [];
    let videos = [];
    let verificationCode = null;
    let pendingVerifyUser = null;

    // ----- DOM -----
    const loginScreen = document.getElementById('loginScreen');
    const registerScreen = document.getElementById('registerScreen');
    const appContainer = document.getElementById('appContainer');
    const loginUsername = document.getElementById('loginUsername');
    const loginPassword = document.getElementById('loginPassword');
    const loginBtn = document.getElementById('loginBtn');
    const loginError = document.getElementById('loginError');
    const gotoRegister = document.getElementById('gotoRegister');

    const regUsername = document.getElementById('regUsername');
    const regDob = document.getElementById('regDob');
    const regEmail = document.getElementById('regEmail');
    const regPassword = document.getElementById('regPassword');
    const regBtn = document.getElementById('regBtn');
    const regError = document.getElementById('regError');
    const gotoLogin = document.getElementById('gotoLogin');

    const verifyModal = document.getElementById('verifyModal');
    const verifyCodeInput = document.getElementById('verifyCodeInput');
    const verifyBtn = document.getElementById('verifyBtn');
    const verifyError = document.getElementById('verifyError');
    const resendCodeBtn = document.getElementById('resendCodeBtn');
    const verifyBanner = document.getElementById('verifyBanner');
    const verifyBannerBtn = document.getElementById('verifyBannerBtn');

    const postsContainer = document.getElementById('postsContainer');
    const findInput = document.getElementById('findInput');
    const findAddBtn = document.getElementById('findAddBtn');
    const requestsContainer = document.getElementById('requestsContainer');
    const allUsersContainer = document.getElementById('allUsersContainer');
    const msgRecipient = document.getElementById('msgRecipient');
    const msgInput = document.getElementById('msgInput');
    const msgSendBtn = document.getElementById('msgSendBtn');
    const messageThread = document.getElementById('messageThread');
    const videoList = document.getElementById('videoList');
    const uploadVideoBtn = document.getElementById('uploadVideoBtn');
    const logoutBtn = document.getElementById('logoutBtn');
    const postCreateBtn = document.getElement    .auth-btn:active { background: #0f2e45; }
    .auth-toggle { text-align: center; color: #2b5b81; font-weight: 500; cursor: pointer; margin-top: 4px; }
    .auth-error { color: #b84a4a; font-size: 0.9rem; padding-left: 8px; min-height: 24px; }
    .hidden { display: none !important; }

    /* ----- MAIN APP ----- */
    .app-container { flex: 1; display: none; flex-direction: column; height: 100%; background: #f0f4fa; }
    .app-header {
      padding: 12px 16px 8px 16px; background: #f0f4fa;
      border-bottom: 1px solid rgba(180,200,220,0.2);
      display: flex; justify-content: space-between; align-items: center; flex-shrink: 0;
    }
    .app-header h1 { font-size: 1.4rem; font-weight: 650; color: #142a41; display: flex; align-items: center; gap: 6px; }
    .app-header h1 i { color: #2a6f9c; }
    .header-actions { display: flex; gap: 12px; align-items: center; }
    .header-actions button { background: transparent; border: none; font-size: 1.2rem; color: #3d5f7f; cursor: pointer; }

    /* verify banner */
    .verify-banner {
      background: #ffedd5; padding: 8px 16px; display: flex; justify-content: space-between;
      align-items: center; border-bottom: 1px solid #f0d5a0; flex-shrink: 0;
    }
    .verify-banner span { font-size: 0.85rem; color: #8a6d3b; }
    .verify-banner button { background: #1a3f5e; color: white; border: none; padding: 4px 16px; border-radius: 60px; font-weight: 500; cursor: pointer; }

    /* main scroll */
    .main-content { flex: 1; overflow-y: auto; padding: 10px 14px 8px 14px; display: flex; flex-direction: column; gap: 14px; }
    .page-section { display: none; flex-direction: column; gap: 14px; animation: fade 0.15s ease; }
    .page-section.active { display: flex; }
    @keyframes fade { 0% { opacity: 0.5; transform: translateY(4px); } 100% { opacity: 1; transform: translateY(0); } }

    .card {
      background: white; border-radius: 20px; padding: 14px 16px;
      border: 1px solid rgba(210,225,245,0.5); box-shadow: 0 2px 6px rgba(0,0,0,0.02);
    }
    .card-title { font-weight: 600; color: #1d334a; font-size: 1rem; margin-bottom: 10px; display: flex; align-items: center; gap: 8px; }
    .card-title i { color: #2a6f9c; width: 22px; }

    /* ----- POSTS (Home) ----- */
    .post-item {
      background: white; border-radius: 20px; padding: 14px 16px; margin-bottom: 12px;
      border: 1px solid #e8eef6; box-shadow: 0 1px 4px rgba(0,0,0,0.02);
    }
    .post-header { display: flex; align-items: center; gap: 10px; margin-bottom: 6px; }
    .post-avatar {
      width: 40px; height: 40px; background: #dce7f5; border-radius: 40px;
      display: flex; align-items: center; justify-content: center; color: #1f4770; font-weight: 600;
    }
    .post-user { font-weight: 600; color: #142a41; }
    .post-time { font-size: 0.7rem; color: #6a85a3; }
    .post-content { margin: 6px 0 10px 0; word-break: break-word; color: #1b2c3f; }
    .post-media { margin: 8px 0; border-radius: 16px; overflow: hidden; max-height: 260px; background: #eef3fa; display: flex; align-items: center; justify-content: center; }
    .post-media img, .post-media video { width: 100%; max-height: 260px; object-fit: cover; }
    .post-actions { display: flex; gap: 16px; border-top: 1px solid #eef3fb; padding-top: 10px; margin-top: 4px; }
    .post-actions button {
      background: transparent; border: none; font-size: 0.85rem; color: #4f6f8f;
      display: flex; align-items: center; gap: 6px; cursor: pointer; padding: 4px 8px; border-radius: 30px;
      transition: 0.1s;
    }
    .post-actions button:active { background: #e8f0fa; }
    .post-actions .liked { color: #1a6bb0; }
    .post-actions .liked i { font-weight: 900; }

    .comment-section { margin-top: 10px; border-top: 1px solid #eef3fb; padding-top: 8px; }
    .comment-item { display: flex; gap: 8px; padding: 4px 0; font-size: 0.9rem; }
    .comment-item strong { color: #1a3f5e; }
    .comment-input-group { display: flex; gap: 6px; margin-top: 6px; }
    .comment-input-group input { flex:1; padding: 8px 14px; border-radius: 60px; border: 1px solid #d4dfee; background: white; outline: none; font-size: 0.85rem; }
    .comment-input-group button { background: #1a3f5e; color: white; border: none; padding: 6px 16px; border-radius: 60px; cursor: pointer; }

    /* ----- FIND FRIENDS ----- */
    .friend-item, .request-item {
      display: flex; justify-content: space-between; align-items: center;
      padding: 8px 4px 8px 10px; border-bottom: 1px solid #eef3fb;
    }
    .friend-item:last-child, .request-item:last-child { border-bottom: none; }
    .friend-info { display: flex; align-items: center; gap: 10px; }
    .friend-avatar {
      width: 36px; height: 36px; background: #dce7f5; border-radius: 40px;
      display: flex; align-items: center; justify-content: center; color: #1f4770; font-weight: 500;
    }
    .btn-sm {
      padding: 4px 14px; border-radius: 60px; border: none; font-weight: 500; font-size: 0.7rem;
      cursor: pointer; background: #eaf1fb; color: #1a4163; display: inline-flex; align-items: center; gap: 4px;
    }
    .btn-sm.primary { background: #1a3f5e; color: white; }
    .btn-sm.danger { background: #fee9e9; color: #b14141; }
    .btn-sm.success { background: #dff0e6; color: #1d6b4a; }
    .input-group { display: flex; gap: 8px; margin-top: 4px; }
    .input-group input { flex:1; padding: 10px 14px; border-radius: 60px; border: 1px solid #d4dfee; background: white; font-size: 0.9rem; outline: none; }
    .input-group input:focus { border-color: #3a7bb0; box-shadow: 0 0 0 3px rgba(50,110,180,0.08); }
    .btn { background: white; border: 1px solid #d0ddeb; padding: 8px 16px; border-radius: 60px; font-weight: 500; color: #1b3a57; cursor: pointer; font-size: 0.85rem; display: inline-flex; align-items: center; gap: 6px; }
    .btn-primary { background: #1a3f5e; border: 1px solid #1a3f5e; color: white; }

    /* ----- MESSAGES ----- */
    .message-item {
      background: white; border-radius: 20px 20px 20px 6px; padding: 8px 14px; margin-bottom: 4px;
      border-left: 4px solid #3a7bb0; display: flex; justify-content: space-between; align-items: center;
    }
    .message-sender { font-weight: 600; color: #1b3b58; min-width: 60px; font-size: 0.9rem; }
    .message-text { flex:1; padding: 0 8px; color: #1b2c3f; font-size: 0.9rem; }
    .message-time { font-size: 0.6rem; background: #edf3fa; padding: 2px 10px; border-radius: 60px; color: #526f8f; }
    .empty-state { color: #6a7f9b; padding: 20px 0; text-align: center; font-style: italic; background: rgba(255,255,255,0.3); border-radius: 60px; font-size: 0.9rem; }

    /* ----- VIDEOS ----- */
    .video-grid { display: flex; flex-direction: column; gap: 10px; }
    .video-card { background: white; border-radius: 20px; padding: 10px 14px; display: flex; align-items: center; gap: 12px; border: 1px solid rgba(200,215,235,0.4); }
    .video-thumb { width: 60px; height: 60px; background: #dce7f5; border-radius: 16px; display: flex; align-items: center; justify-content: center; color: #1d4b74; font-size: 1.6rem; }
    .upload-btn-big {
      background: #1a3f5e; color: white; border: none; padding: 14px; border-radius: 60px;
      font-size: 1.1rem; font-weight: 600; display: flex; align-items: center; justify-content: center; gap: 10px;
      cursor: pointer; width: 100%; margin: 8px 0;
    }
    .upload-btn-big:active { background: #0f2e45; }

    /* ----- BOTTOM NAV ----- */
    .bottom-nav {
      background: #ffffffdd; backdrop-filter: blur(16px); border-top: 1px solid rgba(180,200,220,0.3);
      display: flex; justify-content: space-around; padding: 6px 6px 12px 6px; flex-shrink: 0;
    }
    .nav-item {
      display: flex; flex-direction: column; align-items: center; background: transparent; border: none;
      color: #6b85a3; font-size: 0.6rem; font-weight: 500; gap: 1px; cursor: pointer; padding: 4px 10px; border-radius: 40px;
      transition: 0.1s;
    }
    .nav-item i { font-size: 1.4rem; }
    .nav-item.active { color: #1d4b77; background: #e3edfa; }
    .nav-item:active { transform: scale(0.92); }

    /* share modal */
    .modal-overlay {
      position: absolute; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.5);
      display: none; align-items: center; justify-content: center; z-index: 999; backdrop-filter: blur(4px);
    }
    .modal-overlay.show { display: flex; }
    .modal-box {
      background: white; border-radius: 28px; padding: 24px 20px; width: 90%; max-width: 360px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    .modal-box h3 { margin-bottom: 12px; color: #142a41; }
    .modal-box .friend-item { padding: 6px 0; cursor: pointer; border-bottom: 1px solid #eef3fb; }
    .modal-box .friend-item:active { background: #f0f6fe; }
    .modal-close { float: right; background: none; border: none; font-size: 1.4rem; color: #6b85a3; cursor: pointer; }

    /* post upload modal */
    .upload-modal .modal-box { padding: 20px; }
    .upload-modal textarea { width: 100%; padding: 12px; border-radius: 16px; border: 1px solid #d4dfee; resize: vertical; min-height: 80px; font-size: 0.95rem; outline: none; }
    .upload-modal input[type="file"] { margin: 10px 0; }
  </style>
</head>
<body>
<div class="phone" id="app">

  <!-- AUTH -->
  <div class="auth-screen" id="authScreen">
    <h2><i class="fas fa-connectdevelop" style="color:#1f5a85;"></i> Connect</h2>
    <div class="sub" id="authSub">Sign in to your account</div>
    <input class="auth-input" id="authUsername" placeholder="Username" />
    <input class="auth-input" id="authPassword" type="password" placeholder="Password" />
    <div id="authError" class="auth-error"></div>
    <button class="auth-btn" id="authActionBtn">Login</button>
    <div class="auth-toggle" id="authToggle">Don't have an account? <strong>Register</strong></div>
  </div>

  <!-- REGISTER (hidden) -->
  <div class="auth-screen hidden" id="registerScreen">
    <h2><i class="fas fa-user-plus" style="color:#1f5a85;"></i> Register</h2>
    <div class="sub">Create your account</div>
    <input class="auth-input" id="regUsername" placeholder="Username" />
    <input class="auth-input" id="regDob" placeholder="Date of Birth (YYYY-MM-DD)" />
    <input class="auth-input" id="regEmail" placeholder="Email or Phone" />
    <input class="auth-input" id="regPassword" type="password" placeholder="Password" />
    <div id="regError" class="auth-error"></div>
    <button class="auth-btn" id="regActionBtn">Register</button>
    <div class="auth-toggle" id="regToggle">Already have an account? <strong>Login</strong></div>
  </div>

  <!-- VERIFY EMAIL (modal) -->
  <div class="modal-overlay" id="verifyModal">
    <div class="modal-box">
      <h3><i class="fas fa-envelope"></i> Verify Email</h3>
      <p style="font-size:0.9rem; color:#3e5f7e; margin-bottom:10px;">Enter the 6-digit code sent to your email.</p>
      <input class="auth-input" id="verifyCodeInput" placeholder="Code" style="margin-bottom:10px;" />
      <div id="verifyError" class="auth-error"></div>
      <button class="auth-btn" id="verifyBtn">Verify</button>
      <button class="auth-btn auth-btn-outline" id="resendCodeBtn" style="background:transparent; border:1px solid #b8cee5; color:#1a3f5e; margin-top:6px;">Resend Code</button>
    </div>
  </div>

  <!-- SHARE MODAL -->
  <div class="modal-overlay" id="shareModal">
    <div class="modal-box">
      <button class="modal-close" id="shareModalClose"><i class="fas fa-times"></i></button>
      <h3><i class="fas fa-share-alt"></i> Share</h3>
      <div id="shareFriendList"><div class="empty-state">No friends to share with</div></div>
      <button class="btn" id="copyLinkBtn" style="width:100%; justify-content:center; margin-top:8px;"><i class="fas fa-copy"></i> Copy link</button>
    </div>
  </div>

  <!-- POST UPLOAD MODAL -->
  <div class="modal-overlay upload-modal" id="uploadModal">
    <div class="modal-box">
      <button class="modal-close" id="uploadModalClose"><i class="fas fa-times"></i></button>
      <h3><i class="fas fa-plus-circle"></i> Create Post</h3>
      <textarea id="postCaption" placeholder="What's on your mind?"></textarea>
      <input type="file" id="postFile" accept="image/*,video/*" />
      <button class="auth-btn" id="postSubmitBtn" style="margin-top:10px;">Post</button>
    </div>
  </div>

  <!-- MAIN APP -->
  <div class="app-container" id="appContainer">
    <div class="app-header">
      <h1><i class="fas fa-connectdevelop"></i> Connect</h1>
      <div class="header-actions">
        <button id="postCreateBtn"><i class="fas fa-plus-circle"></i></button>
        <button id="logoutBtn"><i class="fas fa-sign-out-alt"></i></button>
      </div>
    </div>

    <!-- verify banner -->
    <div class="verify-banner hidden" id="verifyBanner">
      <span><i class="fas fa-exclamation-triangle"></i> Verify your email to access all features.</span>
      <button id="verifyBannerBtn">Verify</button>
    </div>

    <div class="main-content" id="mainContent">
      <!-- HOME -->
      <div class="page-section active" id="pageHome">
        <div id="postsContainer"></div>
      </div>

      <!-- FIND -->
      <div class="page-section" id="pageFind">
        <div class="card">
          <div class="card-title"><i class="fas fa-search"></i> Find friends</div>
          <div class="input-group">
            <input type="text" id="findInput" placeholder="Username" />
            <button class="btn btn-primary" id="findAddBtn"><i class="fas fa-user-plus"></i> Add</button>
          </div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-user-plus"></i> Requests</div>
          <div id="requestsContainer"><div class="empty-state">No requests</div></div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-list-ul"></i> All users</div>
          <div id="allUsersContainer"><div class="empty-state">No users</div></div>
        </div>
      </div>

      <!-- MESSAGES -->
      <div class="page-section" id="pageMessages">
        <div class="card">
          <div class="card-title"><i class="fas fa-comment-dots"></i> Chats</div>
          <div id="messageThread" style="max-height:300px; overflow-y:auto; display:flex; flex-direction:column; gap:4px;"></div>
          <div class="input-group" style="margin-top:10px;">
            <select id="msgRecipient" style="flex:0.7; min-width:80px; padding:8px 10px; border-radius:60px; border:1px solid #d4dfee; background:white; outline:none;"><option value="">— friend —</option></select>
            <input type="text" id="msgInput" placeholder="Type..." style="flex:1; padding:8px 14px; border-radius:60px; border:1px solid #d4dfee; outline:none;" />
            <button class="btn btn-primary" id="msgSendBtn"><i class="fas fa-paper-plane"></i></button>
          </div>
        </div>
      </div>

      <!-- VIDEOS -->
      <div class="page-section" id="pageVideos">
        <div class="card">
          <div class="card-title"><i class="fas fa-video"></i> Videos</div>
          <div id="videoList" class="video-grid"><div class="empty-state">No videos yet</div></div>
          <button class="upload-btn-big" id="uploadVideoBtn"><i class="fas fa-cloud-upload-alt"></i> Upload video</button>
        </div>
      </div>
    </div>

    <!-- bottom nav -->
    <div class="bottom-nav">
      <button class="nav-item active" data-page="pageHome"><i class="fas fa-home"></i><span>Home</span></button>
      <button class="nav-item" data-page="pageFind"><i class="fas fa-search"></i><span>Find</span></button>
      <button class="nav-item" data-page="pageMessages"><i class="fas fa-comment-dots"></i><span>Messages</span></button>
      <button class="nav-item" data-page="pageVideos"><i class="fas fa-film"></i><span>Videos</span></button>
    </div>
  </div>
</div>

<script>
  (function() {
    // ----- DATA -----
    let users = [];          // { username, password, dob, email, verified }
    let currentUser = null;
    let friends = [];        // usernames
    let friendRequests = []; // { from, to, status }
    let messages = [];       // { from, to, text, timestamp }
    let posts = [];          // { id, author, caption, mediaType, mediaData, timestamp, likes: [], comments: [{author, text, timestamp}] }
    let videos = [];         // { title, uploadedBy, timestamp }
    let verificationCode = null;
    let pendingVerifyUser = null;

    // ----- DOM -----
    const authScreen = document.getElementById('authScreen');
    const registerScreen = document.getElementById('registerScreen');
    const appContainer = document.getElementById('appContainer');
    const authUsername = document.getElementById('authUsername');
    const authPassword = document.getElementById('authPassword');
    const authActionBtn = document.getElementById('authActionBtn');
    const authToggle = document.getElementById('authToggle');
    const authSub = document.getElementById('authSub');
    const authError = document.getElementById('authError');

    const regUsername = document.getElementById('regUsername');
    const regDob = document.getElementById('regDob');
    const regEmail = document.getElementById('regEmail');
    const regPassword = document.getElementById('regPassword');
    const regActionBtn = document.getElementById('regActionBtn');
    const regToggle = document.getElementById('regToggle');
    const regError = document.getElementById('regError');

    const verifyModal = document.getElementById('verifyModal');
    const verifyCodeInput = document.getElementById('verifyCodeInput');
    const verifyBtn = document.getElementById('verifyBtn');
    const verifyError = document.getElementById('verifyError');
    const resendCodeBtn = document.getElementById('resendCodeBtn');
    const verifyBanner = document.getElementById('verifyBanner');
    const verifyBannerBtn = document.getElementById('verifyBannerBtn');

    const postsContainer = document.getElementById('postsContainer');
    const findInput = document.getE      border: 1px solid #dae3ef;
      border-radius: 60px;
      padding: 16px 20px;
      font-size: 1rem;
      width: 100%;
      outline: none;
      transition: 0.15s;
    }

    .auth-input:focus {
      border-color: #1f5a85;
      box-shadow: 0 0 0 3px rgba(30, 90, 150, 0.1);
    }

    .auth-btn {
      background: #1a3f5e;
      border: none;
      color: white;
      font-weight: 600;
      padding: 16px;
      border-radius: 60px;
      font-size: 1.05rem;
      cursor: pointer;
      transition: 0.15s;
      margin-top: 6px;
      width: 100%;
    }

    .auth-btn:active { background: #0f2e45; }
    .auth-btn-outline {
      background: transparent;
      border: 1px solid #b8cee5;
      color: #1a3f5e;
    }

    .auth-toggle {
      text-align: center;
      color: #2b5b81;
      font-weight: 500;
      margin-top: 6px;
      cursor: pointer;
    }

    .auth-error {
      color: #b84a4a;
      font-size: 0.9rem;
      padding-left: 8px;
    }

    /* --- main app --- */
    .app-container {
      flex: 1;
      display: none;
      flex-direction: column;
      height: 100%;
      background: #f5f9ff;
    }

    .app-header {
      padding: 18px 20px 10px 20px;
      background: #f5f9ff;
      border-bottom: 1px solid rgba(180, 200, 220, 0.2);
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-shrink: 0;
    }

    .app-header h1 {
      font-size: 1.5rem;
      font-weight: 650;
      color: #142a41;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .app-header h1 i { color: #2a6f9c; }

    .logout-btn {
      background: transparent;
      border: none;
      color: #4f6f8f;
      font-size: 1.2rem;
      padding: 6px 10px;
      cursor: pointer;
    }

    /* main scroll */
    .main-content {
      flex: 1;
      overflow-y: auto;
      padding: 12px 16px 8px 16px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .page-section {
      display: none;
      flex-direction: column;
      gap: 16px;
      animation: fade 0.15s ease;
    }

    .page-section.active { display: flex; }

    @keyframes fade { 0% { opacity: 0.5; transform: translateY(4px); } 100% { opacity: 1; transform: translateY(0); } }

    .card {
      background: white;
      border-radius: 28px;
      padding: 16px 18px;
      border: 1px solid rgba(210, 225, 245, 0.5);
      box-shadow: 0 2px 8px rgba(0,0,0,0.02);
    }

    .card-title {
      font-weight: 600;
      color: #1d334a;
      font-size: 1.05rem;
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .card-title i { color: #2a6f9c; width: 24px; }

    .friend-item, .request-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 4px 10px 12px;
      border-bottom: 1px solid #eef3fb;
    }

    .friend-item:last-child, .request-item:last-child { border-bottom: none; }

    .friend-info {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .friend-avatar {
      width: 40px;
      height: 40px;
      background: #dce7f5;
      border-radius: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #1f4770;
      font-weight: 500;
    }

    .btn-sm {
      padding: 6px 14px;
      border-radius: 60px;
      border: none;
      font-weight: 500;
      font-size: 0.75rem;
      cursor: pointer;
      background: #eaf1fb;
      color: #1a4163;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    .btn-sm.primary { background: #1a3f5e; color: white; }
    .btn-sm.danger { background: #fee9e9; color: #b14141; }
    .btn-sm.success { background: #dff0e6; color: #1d6b4a; }

    .input-group {
      display: flex;
      gap: 8px;
      margin-top: 4px;
    }

    .input-group input, .input-group select {
      flex: 1;
      padding: 12px 14px;
      border-radius: 60px;
      border: 1px solid #d4dfee;
      background: white;
      font-size: 0.95rem;
      outline: none;
    }

    .input-group input:focus, .input-group select:focus {
      border-color: #3a7bb0;
      box-shadow: 0 0 0 3px rgba(50, 110, 180, 0.08);
    }

    .btn {
      background: white;
      border: 1px solid #d0ddeb;
      padding: 10px 18px;
      border-radius: 60px;
      font-weight: 500;
      color: #1b3a57;
      cursor: pointer;
      transition: 0.1s;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      font-size: 0.9rem;
    }

    .btn-primary { background: #1a3f5e; border: 1px solid #1a3f5e; color: white; }
    .btn-primary i { color: white; }
    .btn-primary:active { background: #0f2e45; }

    /* messages */
    .message-item {
      background: white;
      border-radius: 24px 24px 24px 6px;
      padding: 10px 14px;
      margin-bottom: 6px;
      border-left: 4px solid #3a7bb0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-shadow: 0 1px 4px rgba(0,0,0,0.02);
    }

    .message-sender { font-weight: 600; color: #1b3b58; min-width: 60px; }
    .message-text { flex: 1; padding: 0 8px; color: #1b2c3f; }
    .message-time { font-size: 0.6rem; background: #edf3fa; padding: 3px 10px; border-radius: 60px; color: #526f8f; }

    .empty-state {
      color: #6a7f9b;
      padding: 20px 0;
      text-align: center;
      font-style: italic;
      background: rgba(255,255,255,0.3);
      border-radius: 60px;
      font-size: 0.9rem;
    }

    /* video upload */
    .video-grid {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .video-card {
      background: white;
      border-radius: 24px;
      padding: 12px 16px;
      display: flex;
      align-items: center;
      gap: 14px;
      border: 1px solid rgba(200, 215, 235, 0.4);
    }

    .video-thumb {
      width: 70px;
      height: 70px;
      background: #dce7f5;
      border-radius: 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #1d4b74;
      font-size: 1.8rem;
    }

    .upload-btn-big {
      background: #1a3f5e;
      color: white;
      border: none;
      padding: 18px;
      border-radius: 60px;
      font-size: 1.2rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
      cursor: pointer;
      margin: 12px 0;
      transition: 0.1s;
      width: 100%;
    }

    .upload-btn-big:active { background: #0f2e45; }

    /* bottom nav */
    .bottom-nav {
      background: #ffffffdd;
      backdrop-filter: blur(16px);
      border-top: 1px solid rgba(180, 200, 220, 0.3);
      display: flex;
      justify-content: space-around;
      padding: 8px 8px 14px 8px;
      flex-shrink: 0;
    }

    .nav-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      background: transparent;
      border: none;
      color: #6b85a3;
      font-size: 0.65rem;
      font-weight: 500;
      gap: 2px;
      cursor: pointer;
      padding: 4px 12px;
      border-radius: 40px;
      transition: 0.1s;
    }

    .nav-item i { font-size: 1.5rem; }
    .nav-item.active { color: #1d4b77; background: #e3edfa; }
    .nav-item:active { transform: scale(0.92); }

    /* scroll */
    .main-content::-webkit-scrollbar { width: 4px; }
    .main-content::-webkit-scrollbar-thumb { background: #c5d6ea; border-radius: 10px; }

    .hidden { display: none !important; }
  </style>
</head>
<body>
<div class="phone" id="app">

  <!-- AUTH: login/register -->
  <div class="auth-screen" id="authScreen">
    <h2><i class="fas fa-connectdevelop" style="color:#1f5a85;"></i> Connect</h2>
    <div class="sub" id="authSub">Sign in to your account</div>
    <input class="auth-input" id="authUsername" placeholder="Username" />
    <input class="auth-input" id="authPassword" type="password" placeholder="Password" />
    <div id="authError" class="auth-error"></div>
    <button class="auth-btn" id="authActionBtn">Login</button>
    <div class="auth-toggle" id="authToggle">Don't have an account? <strong>Register</strong></div>
  </div>

  <!-- MAIN APP -->
  <div class="app-container" id="appContainer">
    <div class="app-header">
      <h1><i class="fas fa-connectdevelop"></i> Connect</h1>
      <button class="logout-btn" id="logoutBtn"><i class="fas fa-sign-out-alt"></i></button>
    </div>

    <div class="main-content" id="mainContent">
      <!-- HOME -->
      <div class="page-section active" id="pageHome">
        <div class="card">
          <div class="card-title"><i class="fas fa-home"></i> Home</div>
          <div style="display:flex; align-items:center; gap:12px; background:#eaf1fb; border-radius:60px; padding:14px 18px;">
            <i class="fas fa-user-circle" style="font-size:2.2rem; color:#2f6490;"></i>
            <div><strong id="homeUserDisplay">User</strong><br><span style="font-size:0.8rem; color:#345a7a;" id="homeFriendCount">0 friends</span></div>
          </div>
          <div style="margin-top:10px; background:white; border-radius:28px; padding:12px 14px; border:1px solid #e0ecfa;">
            <i class="fas fa-comment-dots" style="color:#2f6b9a;"></i> <span style="font-weight:500; margin-left:6px;">Latest</span>
            <div id="homeMessagePreview" style="margin-top:4px; color:#34587a; font-size:0.9rem;">No messages</div>
          </div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-user-friends"></i> Friends</div>
          <div id="homeFriendPreview" style="display:flex; flex-wrap:wrap; gap:6px;">—</div>
        </div>
      </div>

      <!-- FIND -->
      <div class="page-section" id="pageFind">
        <div class="card">
          <div class="card-title"><i class="fas fa-search"></i> Find friends</div>
          <div class="input-group">
            <input type="text" id="findInput" placeholder="Username" />
            <button class="btn btn-primary" id="findAddBtn"><i class="fas fa-user-plus"></i> Add</button>
          </div>
          <div style="margin-top:8px; display:flex; gap:6px; flex-wrap:wrap;">
            <button class="btn btn-sm" id="findSampleBtn"><i class="fas fa-users"></i> Add samples</button>
          </div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-user-plus"></i> Friend requests</div>
          <div id="requestsContainer"><div class="empty-state">No requests</div></div>
        </div>
        <div class="card">
          <div class="card-title"><i class="fas fa-list-ul"></i> All users</div>
          <div id="allUsersContainer"><div class="empty-state">No users yet</div></div>
        </div>
      </div>

      <!-- MESSAGES -->
      <div class="page-section" id="pageMessages">
        <div class="card">
          <div class="card-title"><i class="fas fa-comment-dots"></i> Chats</div>
          <div id="messageThread" style="max-height:300px; overflow-y:auto; display:flex; flex-direction:column; gap:4px;"></div>
          <div class="input-group" style="margin-top:12px;">
            <select id="msgRecipient" style="flex:0.7; min-width:80px;"><option value="">— friend —</option></select>
            <input type="text" id="msgInput" placeholder="Type..." />
            <button class="btn btn-primary" id="msgSendBtn"><i class="fas fa-paper-plane"></i></button>
          </div>
        </div>
      </div>

      <!-- VIDEOS -->
      <div class="page-section" id="pageVideos">
        <div class="card">
          <div class="card-title"><i class="fas fa-video"></i> Videos</div>
          <div id="videoList" class="video-grid">
            <div class="empty-state">No videos yet</div>
          </div>
          <button class="upload-btn-big" id="uploadVideoBtn"><i class="fas fa-cloud-upload-alt"></i> Upload video</button>
        </div>
      </div>
    </div>

    <!-- bottom nav -->
    <div class="bottom-nav">
      <button class="nav-item active" data-page="pageHome"><i class="fas fa-home"></i><span>Home</span></button>
      <button class="nav-item" data-page="pageFind"><i class="fas fa-search"></i><span>Find</span></button>
      <button class="nav-item" data-page="pageMessages"><i class="fas fa-comment-dots"></i><span>Messages</span></button>
      <button class="nav-item" data-page="pageVideos"><i class="fas fa-film"></i><span>Videos</span></button>
    </div>
  </div>
</div>

<script>
  (function(){
    // ----- data store -----
    let users = [];           // { username, password }
    let currentUser = null;
    let friends = [];         // usernames
    let friendRequests = [];  // { from, to, status: 'pending' }
    let messages = [];        // { from, to, text, timestamp }
    let videos = [];          // { title, uploadedBy, url, timestamp }

    // ----- DOM refs -----
    const authScreen = document.getElementById('authScreen');
    const appContainer = document.getElementById('appContainer');
    const authUsername = document.getElementById('authUsername');
    const authPassword = document.getElementById('authPassword');
    const authActionBtn = document.getElementById('authActionBtn');
    const authToggle = document.getElementById('authToggle');
    const authSub = document.getElementById('authSub');
    const authError = document.getElementById('authError');
    const logoutBtn = document.getElementById('logoutBtn');

    // home
    const homeUserDisplay = document.getElementById('homeUserDisplay');
    const homeFriendCount = document.getElementById('homeFriendCount');
    const homeMessagePreview = document.getElementById('homeMessagePreview');
    const homeFriendPreview = document.getElementById('homeFriendPreview');

    // find
    const findInput = document.getElementById('findInput');
    const findAddBtn = document.getElementById('findAddBtn');
    const findSampleBtn = document.getElementById('findSampleBtn');
    const requestsContainer = document.getElementById('requestsContainer');
    const allUsersContainer = document.getElementById('allUsersContainer');

    // messages
    const msgRecipient = document.getElementById('msgRecipient');
    const msgInput = document.getElementById('msgInput');
    const msgSendBtn = document.getElementById('msgSendBtn');
    const messageThread = document.getElementById('messageThread');

    // videos
    const videoList = document.getElementById('videoList');
    const uploadBtn = document.getElementById('uploadVideoBtn');

    // nav
    const navItems = document.querySelectorAll('.nav-item');
    const pages = {
      home: document.getElementById('pageHome'),
      find: document.getElementById('pageFind'),
      messages: document.getElementById('pageMessages'),
      videos: document.getElementById('pageVideos')
    };

    // ----- storage -----
    function saveAll() {
      localStorage.setItem('social_users', JSON.stringify(users));
      localStorage.setItem('social_friends', JSON.stringify(friends));
      localStorage.setItem('social_requests', JSON.stringify(friendRequests));
      localStorage.setItem('social_messages', JSON.stringify(messages));
      localStorage.setItem('social_videos', JSON.stringify(videos));
      if (currentUser) localStorage.setItem('social_session', currentUser);
      else localStorage.removeItem('social_session');
    }

    function loadAll() {
      const u = localStorage.getItem('social_users');
      const f = localStorage.getItem('social_friends');
      const r = localStorage.getItem('social_requests');
      const m = localStorage.getItem('social_messages');
      const v = localStorage.getItem('social_videos');
      const session = localStorage.getItem('social_session');

      if (u) try { users = JSON.parse(u); } catch(e){ users = []; }
      else users = [{ username: 'alice', password: '123' }, { username: 'bob', password: '123' }];

      if (f) try { friends = JSON.parse(f); } catch(e){ friends = []; }
      else friends = ['alice', 'bob'];

      if (r) try { friendRequests = JSON.parse(r); } catch(e){ friendRequests = []; }
      else friendRequests = [];

      if (m) try { messages = JSON.parse(m); } catch(e){ messages = []; }
      else messages = [];

      if (v) try { videos = JSON.parse(v); } catch(e){ videos = []; }
      else videos = [];

      if (session && users.find(u => u.username === session)) {
        currentUser = session;
        showApp();
      } else {
        showAuth();
      }
    }

    // ----- UI switch -----
    function showAuth() {
      authScreen.style.display = 'flex';
      appContainer.style.display = 'none';
      currentUser = null;
    }

    function showApp() {
      authScreen.style.display = 'none';
      appContainer.style.display = 'flex';
      renderAll();
      navigateTo('pageHome');
    }

    // ----- render -----
    function renderAll() {
      renderHome();
      renderFind();
      renderMessages();
      renderVideos();
      updateRecipients();
    }

    function renderHome() {
      homeUserDisplay.textContent = currentUser || 'User';
      const count = friends.length;
      homeFriendCount.textContent = count + (count === 1 ? ' friend' : ' friends');
      if (messages.length) {
        const last = messages[messages.length-1];
        homeMessagePreview.textContent = `${last.from}: ${last.text.substring(0,30)}${last.text.length>30?'…':''}`;
      } else homeMessagePreview.textContent = 'No messages';
      if (friends.length) {
        const names = friends.slice(0,4).map(n => `<span style="background:#dce7f5;padding:4px 14px;border-radius:60px;font-size:0.8rem;">${n}</span>`).join('');
        homeFriendPreview.innerHTML = names + (friends.length>4 ? ` <span style="color:#4a6f92;">+${friends.length-4}</span>` : '');
      } else homeFriendPreview.innerHTML = '<span class="empty-state" style="padding:0;font-style:normal;">No friends yet</span>';
    }

    function renderFind() {
      // all users (except current and friends)
      const allUsers = users.filter(u => u.username !== currentUser && !friends.includes(u.username) && !friendRequests.some(r => r.from === u.username && r.to === currentUser && r.status === 'pending'));
      if (allUsers.length === 0) {
        allUsersContainer.innerHTML = '<div class="empty-state">No users to add</div>';
      } else {
        let html = '';
       color: #142a41;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .app-header h1 i {
      color: #2a6f9c;
      font-size: 1.7rem;
    }

    .header-actions {
      display: flex;
      gap: 12px;
      color: #2b4f72;
    }

    .header-actions i {
      font-size: 1.3rem;
      opacity: 0.7;
    }

    /* main scroll content */
    .main-content {
      flex: 1;
      overflow-y: auto;
      padding: 12px 18px 8px 18px;
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    /* each "page" (section) – shown/hidden */
    .page-section {
      display: none;
      flex-direction: column;
      gap: 16px;
      animation: fadeUp 0.2s ease;
    }

    .page-section.active {
      display: flex;
    }

    @keyframes fadeUp {
      0% { opacity: 0.5; transform: translateY(6px); }
      100% { opacity: 1; transform: translateY(0); }
    }

    /* ----- cards & components ----- */
    .card {
      background: white;
      border-radius: 28px;
      padding: 18px 18px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02), 0 1px 3px rgba(0,0,0,0.02);
      border: 1px solid rgba(210, 225, 245, 0.4);
    }

    .card-title {
      font-weight: 600;
      color: #1d334a;
      font-size: 1.05rem;
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .card-title i {
      color: #2a6f9c;
      width: 24px;
    }

    /* friend list */
    .friend-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 4px 10px 12px;
      border-bottom: 1px solid #eef3fb;
    }

    .friend-item:last-child {
      border-bottom: none;
    }

    .friend-info {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .friend-avatar {
      width: 40px;
      height: 40px;
      background: #dce7f5;
      border-radius: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #1f4770;
      font-weight: 500;
      font-size: 1rem;
    }

    .friend-name {
      font-weight: 500;
      color: #1c3147;
    }

    .friend-action-btn {
      background: transparent;
      border: none;
      padding: 6px 12px;
      border-radius: 30px;
      background: #edf4fd;
      color: #1e4b73;
      font-size: 0.75rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 4px;
      cursor: pointer;
      transition: 0.1s;
    }

    .friend-action-btn i {
      font-size: 0.7rem;
    }

    .friend-action-btn.danger {
      background: #fee9e9;
      color: #b14141;
    }

    /* input groups */
    .input-group {
      display: flex;
      gap: 8px;
      margin-top: 6px;
    }

    .input-group input, 
    .input-group select {
      flex: 1;
      padding: 12px 14px;
      border-radius: 60px;
      border: 1px solid #d4dfee;
      background: white;
      font-size: 0.95rem;
      outline: none;
      transition: 0.15s;
    }

    .input-group input:focus,
    .input-group select:focus {
      border-color: #3a7bb0;
      box-shadow: 0 0 0 3px rgba(50, 110, 180, 0.08);
    }

    .btn {
      background: white;
      border: 1px solid #d0ddeb;
      padding: 10px 18px;
      border-radius: 60px;
      font-weight: 500;
      color: #1b3a57;
      cursor: pointer;
      transition: 0.15s;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      white-space: nowrap;
      font-size: 0.9rem;
    }

    .btn-primary {
      background: #1a3f5e;
      border: 1px solid #1a3f5e;
      color: white;
    }

    .btn-primary i {
      color: white;
    }

    .btn-primary:active {
      background: #0f2e45;
    }

    .btn-outline {
      background: transparent;
      border: 1px solid #c6d6ec;
    }

    /* message bubbles */
    .message-item {
      background: white;
      border-radius: 24px 24px 24px 6px;
      padding: 12px 16px;
      margin-bottom: 8px;
      border-left: 4px solid #3a7bb0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-shadow: 0 1px 4px rgba(0,0,0,0.02);
    }

    .message-sender {
      font-weight: 600;
      color: #1b3b58;
      min-width: 70px;
    }

    .message-text {
      flex: 1;
      padding: 0 8px;
      color: #1b2c3f;
    }

    .message-time {
      font-size: 0.65rem;
      background: #edf3fa;
      padding: 4px 10px;
      border-radius: 60px;
      color: #526f8f;
    }

    .empty-state {
      color: #6a7f9b;
      padding: 20px 0;
      text-align: center;
      font-style: italic;
      background: rgba(255,255,255,0.3);
      border-radius: 60px;
      font-size: 0.9rem;
    }

    /* video cards (dummy) */
    .video-grid {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .video-card {
      background: white;
      border-radius: 24px;
      padding: 12px 16px;
      display: flex;
      align-items: center;
      gap: 14px;
      border: 1px solid rgba(200, 215, 235, 0.4);
      box-shadow: 0 2px 6px rgba(0,0,0,0.01);
    }

    .video-thumb {
      width: 70px;
      height: 70px;
      background: linear-gradient(145deg, #c5d7ed, #dce8f7);
      border-radius: 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #1d4b74;
      font-size: 1.8rem;
    }

    .video-info h4 {
      font-size: 0.95rem;
      color: #1b334b;
    }

    .video-info p {
      font-size: 0.75rem;
      color: #5c7493;
    }

    /* ----- bottom navigation (phone style) ----- */
    .bottom-nav {
      background: #ffffffdd;
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border-top: 1px solid rgba(180, 200, 220, 0.3);
      display: flex;
      justify-content: space-around;
      padding: 10px 10px 12px 10px;
      box-shadow: 0 -4px 12px rgba(0,0,0,0.02);
    }

    .nav-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      background: transparent;
      border: none;
      color: #6b85a3;
      font-size: 0.7rem;
      font-weight: 500;
      gap: 2px;
      cursor: pointer;
      transition: 0.15s;
      padding: 4px 14px;
      border-radius: 40px;
    }

    .nav-item i {
      font-size: 1.6rem;
      margin-bottom: 2px;
      transition: 0.1s;
    }

    .nav-item.active {
      color: #1d4b77;
      background: #e3edfa;
    }

    .nav-item:active {
      transform: scale(0.94);
    }

    /* utils */
    .mt-1 { margin-top: 6px; }
    .mb-1 { margin-bottom: 6px; }
    .flex { display: flex; align-items: center; gap: 8px; }
    .gap-2 { gap: 8px; }
    .text-small { font-size: 0.8rem; opacity: 0.7; }

    /* scroll styling */
    .main-content::-webkit-scrollbar {
      width: 4px;
    }
    .main-content::-webkit-scrollbar-thumb {
      background: #c5d6ea;
      border-radius: 10px;
    }
  </style>
</head>
<body>
<div class="phone-frame">

  <!-- header -->
  <div class="app-header">
    <h1><i class="fas fa-connectdevelop"></i> Connect</h1>
    <div class="header-actions">
      <i class="fas fa-search"></i>
      <i class="fas fa-bell"></i>
    </div>
  </div>

  <!-- main content : pages -->
  <div class="main-content" id="mainContent">

    <!-- HOME page -->
    <div class="page-section active" id="pageHome">
      <div class="card">
        <div class="card-title"><i class="fas fa-home"></i> Home feed</div>
        <div style="display: flex; flex-direction: column; gap: 16px;">
          <div style="background: #eaf1fb; border-radius: 60px; padding: 16px 18px; display: flex; align-items: center; gap: 12px;">
            <i class="fas fa-user-circle" style="font-size: 2.2rem; color: #2f6490;"></i>
            <div><strong>Welcome back!</strong><br><span style="font-size:0.8rem; color:#345a7a;">You have <span id="homeFriendCount">0</span> friends</span></div>
          </div>
          <div style="background: white; border-radius: 28px; padding: 14px 16px; border:1px solid #e0ecfa;">
            <i class="fas fa-comment-dots" style="color:#2f6b9a;"></i> 
            <span style="font-weight:500; margin-left:6px;">Latest messages</span>
            <div id="homeMessagePreview" style="margin-top: 6px; color:#34587a; font-size:0.9rem;">No messages yet</div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-title"><i class="fas fa-user-friends"></i> Quick friends</div>
        <div id="homeFriendPreview" style="display: flex; flex-wrap: wrap; gap: 8px;">—</div>
      </div>
    </div>

    <!-- FIND FRIENDS page -->
    <div class="page-section" id="pageFind">
      <div class="card">
        <div class="card-title"><i class="fas fa-user-plus"></i> Find & add friends</div>
        <div class="input-group">
          <input type="text" id="findFriendInput" placeholder="Enter name..." />
          <button class="btn btn-primary" id="findAddBtn"><i class="fas fa-plus"></i> Add</button>
        </div>
        <div style="margin-top: 12px; display: flex; gap: 6px; flex-wrap: wrap;">
          <button class="btn btn-outline" id="findSampleBtn" style="font-size:0.75rem;"><i class="fas fa-users"></i> Add samples</button>
          <button class="btn btn-outline" id="findClearBtn" style="font-size:0.75rem; color:#a04a4a;"><i class="fas fa-user-slash"></i> Clear all</button>
        </div>
      </div>
      <div class="card">
        <div class="card-title"><i class="fas fa-list-ul"></i> Your friends</div>
        <div id="findFriendList"><div class="empty-state">No friends yet</div></div>
      </div>
    </div>

    <!-- MESSAGES page -->
    <div class="page-section" id="pageMessages">
      <div class="card">
        <div class="card-title"><i class="fas fa-envelope"></i> Messages</div>
        <div id="messageThread" style="max-height: 240px; overflow-y: auto; display: flex; flex-direction: column; gap: 4px;">
          <!-- dynamic messages -->
        </div>
        <div class="input-group" style="margin-top: 14px;">
          <select id="msgRecipientSelect" style="flex: 0.7; min-width: 90px;">
            <option value="">— friend —</option>
          </select>
          <input type="text" id="msgInput" placeholder="Type..." />
          <button class="btn btn-primary" id="msgSendBtn"><i class="fas fa-paper-plane"></i></button>
        </div>
        <div style="margin-top: 6px; display: flex; justify-content: flex-end;">
          <button class="btn btn-outline" id="msgClearBtn" style="font-size:0.7rem; padding:4px 14px;"><i class="fas fa-eraser"></i> clear</button>
        </div>
      </div>
    </div>

    <!-- VIDEOS page -->
    <div class="page-section" id="pageVideos">
      <div class="card">
        <div class="card-title"><i class="fas fa-video"></i> Video feed</div>
        <div class="video-grid">
          <div class="video-card">
            <div class="video-thumb"><i class="fas fa-play-circle"></i></div>
            <div class="video-info"><h4>Sunset vlog</h4><p>2 min ago · 12 views</p></div>
          </div>
          <div class="video-card">
            <div class="video-thumb"><i class="fas fa-play-circle"></i></div>
            <div class="video-info"><h4>Tech unboxing</h4><p>1 hour ago · 8 views</p></div>
          </div>
          <div class="video-card">
            <div class="video-thumb"><i class="fas fa-play-circle"></i></div>
            <div class="video-info"><h4>Funny cats</h4><p>3 hours ago · 24 views</p></div>
          </div>
          <div class="video-card">
            <div class="video-thumb"><i class="fas fa-play-circle"></i></div>
            <div class="video-info"><h4>Workout tips</h4><p>Yesterday · 5 views</p></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- bottom navigation -->
  <div class="bottom-nav" id="bottomNav">
    <button class="nav-item active" data-page="pageHome"><i class="fas fa-home"></i><span>Home</span></button>
    <button class="nav-item" data-page="pageFind"><i class="fas fa-search"></i><span>Find</span></button>
    <button class="nav-item" data-page="pageMessages"><i class="fas fa-comment-dots"></i><span>Messages</span></button>
    <button class="nav-item" data-page="pageVideos"><i class="fas fa-film"></i><span>Videos</span></button>
  </div>
</div>

<script>
  (function() {
    // ----- data (localStorage) -----
    let friends = [];
    let messages = [];

    // ----- DOM refs -----
    const homeFriendCount = document.getElementById('homeFriendCount');
    const homeMessagePreview = document.getElementById('homeMessagePreview');
    const homeFriendPreview = document.getElementById('homeFriendPreview');

    const findFriendInput = document.getElementById('findFriendInput');
    const findAddBtn = document.getElementById('findAddBtn');
    const findSampleBtn = document.getElementById('findSampleBtn');
    const findClearBtn = document.getElementById('findClearBtn');
    const findFriendList = document.getElementById('findFriendList');

    const msgRecipientSelect = document.getElementById('msgRecipientSelect');
    const msgInput = document.getElementById('msgInput');
    const msgSendBtn = document.getElementById('msgSendBtn');
    const msgClearBtn = document.getElementById('msgClearBtn');
    const messageThread = document.getElementById('messageThread');

    // nav items
    const navItems = document.querySelectorAll('.nav-item');
    const pages = {
      home: document.getElementById('pageHome'),
      find: document.getElementById('pageFind'),
      messages: document.getElementById('pageMessages'),
      videos: document.getElementById('pageVideos')
    };

    // ----- storage -----
    function saveState() {
      localStorage.setItem('mobile_friends', JSON.stringify(friends));
      localStorage.setItem('mobile_messages', JSON.stringify(messages));
    }

    function loadState() {
      const f = localStorage.getItem('mobile_friends');
      const m = localStorage.getItem('mobile_messages');
      if (f) try { friends = JSON.parse(f); } catch(e){ friends = []; }
      else friends = ['Lena', 'Marcus', 'Sophie'];
      if (m) try { messages = JSON.parse(m); } catch(e){ messages = []; }
      else {
        messages = [
          { from: 'Lena', text: 'Hey, how are you?', timestamp: Date.now() - 7200000 },
          { from: 'Marcus', text: 'Game later?', timestamp: Date.now() - 3600000 },
          { from: 'You', text: 'Sure, I\'m in!', timestamp: Date.now() - 1800000 }
        ];
      }
    }

    // ----- render -----
    function renderAll() {
      renderFriendsList();
      renderMessages();
      renderHome();
      updateRecipients();
    }

    function renderFriendsList() {
      const container = findFriendList;
      if (!friends.length) {
        container.innerHTML = '<div class="empty-state">No friends yet</div>';
        return;
      }
      let html = '';
      friends.slice().sort().forEach(name => {
        html += `
          <div class="friend-item">
            <div class="friend-info">
              <div class="friend-avatar">${name.charAt(0).toUpperCase()}</div>
              <span class="friend-name">${name}</span>
            </div>
            <div>
              <button class="friend-action-btn remove-friend-btn" data-name="${name}"><i class="fas fa-user-minus"></i> remove</button>
            </div>
          </div>
        `;
      });
      container.innerHTML = html;

      // remove handlers
      container.querySelectorAll('.remove-friend-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const name = this.dataset.name;
          if (confirm(`Remove "${name}" from friends?`)) {
            friends = friends.filter(f => f !== name);
            saveState();
            renderAll();
          }
        });
      });
    }

    function renderMessages() {
      if (!messages.length) {
        messageThread.innerHTML = `<div class="empty-state">No messages</div>`;
        return;
      }
      const sorted = [...messages].sort((a,b) => a.timestamp - b.timestamp);
      let html = '';
      sorted.forEach(msg => {
        const isYou = msg.from === 'You';
        const time = new Date(msg.timestamp).toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
        html += `
          <div class="message-item" style="${isYou ? 'border-left-color: #6a9bcb;' : ''}">
            <span class="message-sender">${isYou ? 'You' : msg.from}</span>
            <span class="message-text">${escapeHtml(msg.text)}</span>
            <span class="message-time">${time}</span>
          </div>
        `;
      });
      messageThread.innerHTML = html;
      messageThread.scrollTop = messageThread.scrollHeight;
    }

    function renderHome() {
      homeFriendCount.textContent = friends.length;
      // preview last message
      if (messages.length) {
        const last = messages[messages.length-1];
        const preview = `${last.from}: ${last.text.substring(0, 30)}${last.text.length > 30 ? '…' : ''}`;
        homeMessagePreview.textContent = preview;
      } else {
        homeMessagePreview.textContent = 'No messages yet';
      }
      // friend preview (first 4)
      if (friends.length) {
        const names = friends.slice(0, 4).map(n => `<span style="background: #dce7f5; padding:4px 14px; border-radius:60px; font-size:0.8rem;">${n}</span>`).join('');
        homeFriendPreview.innerHTML = names + (friends.length > 4 ? ` <span style="background:transparent; color:#4a6f92;">+${friends.length-4} more</span>` : '');
      } else {
        homeFriendPreview.innerHTML = '<span class="text-small">No friends yet</span>';
      }
    }

    function updateRecipients() {
      const current = msgRecipientSelect.value;
      msgRecipientSelect.innerHTML = '<option value="">— friend —</option>';
      friends.slice().sort().forEach(name => {
        const opt = document.createElement('option');
        opt.value = name;
        opt.textContent = name;
        msgRecipientSelect.appendChild(opt);
      });
      if (current && friends.includes(current)) msgRecipientSelect.value = current;
    }

    function escapeHtml(text) {
      const d = document.createElement('div');
      d.textContent = text;
      return d.innerHTML;
    }

    // ----- actions -----
    function addFriend(name) {
      const trimmed = name.trim();
      if (!trimmed) return alert('Enter a name.');
      if (friends.includes(trimmed)) return alert(`"${trimmed}" already added.`);
      fri            padding-bottom: 0.9rem;
        }

        h1 i {
            color: #3a6ea5;
            font-size: 2rem;
        }

        .grid-2col {
            display: grid;
            grid-template-columns: 1fr 1.2fr;
            gap: 2rem;
        }

        @media (max-width: 700px) {
            .grid-2col {
                grid-template-columns: 1fr;
                gap: 2rem;
            }
            .app-card {
                padding: 1.5rem;
            }
        }

        /* ----- panel style ----- */
        .panel {
            background: rgba(255, 255, 255, 0.5);
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
            border-radius: 1.8rem;
            padding: 1.6rem 1.5rem;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02);
            border: 1px solid rgba(255,255,255,0.7);
        }

        .panel h2 {
            font-size: 1.3rem;
            font-weight: 500;
            color: #1f3349;
            display: flex;
            align-items: center;
            gap: 0.6rem;
            margin-bottom: 1.2rem;
            letter-spacing: -0.2px;
        }

        .panel h2 i {
            color: #3a6ea5;
            width: 1.8rem;
            font-size: 1.2rem;
        }

        /* ----- friends list ----- */
        .friend-list {
            display: flex;
            flex-direction: column;
            gap: 0.65rem;
            margin-bottom: 1.5rem;
            max-height: 240px;
            overflow-y: auto;
            padding-right: 0.3rem;
        }

        .friend-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: white;
            padding: 0.7rem 1rem 0.7rem 1.2rem;
            border-radius: 60px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.02);
            border: 1px solid rgba(200, 212, 230, 0.4);
            transition: all 0.15s;
        }

        .friend-item:hover {
            border-color: #8aacd0;
            background: #f9fcff;
        }

        .friend-name {
            font-weight: 500;
            color: #1d2f44;
        }

        .friend-actions {
            display: flex;
            gap: 0.4rem;
        }

        .btn-icon {
            background: transparent;
            border: none;
            font-size: 1rem;
            padding: 0.3rem 0.6rem;
            border-radius: 30px;
            color: #3d5f7f;
            cursor: pointer;
            transition: 0.15s;
        }

        .btn-icon:hover {
            background: #e3ebf5;
            color: #1a3d5e;
        }

        .btn-icon.danger:hover {
            background: #ffe6e6;
            color: #b13e3e;
        }

        .btn-icon.primary:hover {
            background: #d4e2f5;
            color: #1f4c7a;
        }

        .add-friend-form {
            display: flex;
            gap: 0.5rem;
            margin-top: 0.25rem;
        }

        .add-friend-form input {
            flex: 1;
            padding: 0.7rem 1rem;
            border: 1px solid #d0dae8;
            border-radius: 40px;
            background: white;
            outline: none;
            font-size: 0.95rem;
            transition: 0.2s;
        }

        .add-friend-form input:focus {
            border-color: #4f83b9;
            box-shadow: 0 0 0 3px rgba(60, 110, 180, 0.15);
        }

        .btn {
            background: white;
            border: 1px solid #c8d6e8;
            padding: 0.6rem 1.2rem;
            border-radius: 40px;
            font-weight: 500;
            color: #1f3b57;
            cursor: pointer;
            transition: 0.15s;
            display: inline-flex;
            align-items: center;
            gap: 0.4rem;
            white-space: nowrap;
        }

        .btn-primary {
            background: #1f3b57;
            border: 1px solid #1f3b57;
            color: white;
        }

        .btn-primary:hover {
            background: #143049;
            border-color: #143049;
        }

        .btn-primary i {
            color: white;
        }

        .btn-outline {
            background: transparent;
            border: 1px solid #bccae0;
        }

        .btn-outline:hover {
            background: #eaf0f9;
        }

        /* ----- messages area ----- */
        .message-thread {
            background: rgba(255, 255, 255, 0.4);
            border-radius: 1.4rem;
            padding: 1rem 0.2rem 0.2rem 0.2rem;
            margin-bottom: 1rem;
        }

        .message-list {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            max-height: 260px;
            overflow-y: auto;
            padding: 0 0.2rem 0.2rem 0.2rem;
        }

        .message-bubble {
            background: white;
            padding: 0.7rem 1rem 0.7rem 1.2rem;
            border-radius: 30px 30px 30px 6px;
            box-shadow: 0 1px 4px rgba(0, 0, 0, 0.02);
            border-left: 4px solid #4f83b9;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 0.8rem;
            font-size: 0.95rem;
            word-break: break-word;
        }

        .message-bubble .msg-sender {
            font-weight: 600;
            color: #1d3a57;
            min-width: 70px;
        }

        .message-bubble .msg-text {
            flex: 1;
            color: #1d2f3f;
        }

        .message-bubble .msg-time {
            font-size: 0.7rem;
            color: #6b7e99;
            background: #eef3fa;
            padding: 0.2rem 0.6rem;
            border-radius: 60px;
            white-space: nowrap;
        }

        .empty-state {
            color: #6b7f9a;
            font-style: italic;
            padding: 1.2rem 0.8rem;
            text-align: center;
            background: rgba(255,255,255,0.2);
            border-radius: 60px;
            font-size: 0.95rem;
        }

        .send-message-form {
            display: flex;
            gap: 0.5rem;
            margin-top: 0.8rem;
            align-items: center;
        }

        .send-message-form select {
            padding: 0.6rem 0.8rem;
            border-radius: 40px;
            border: 1px solid #d0dae8;
            background: white;
            font-size: 0.9rem;
            outline: none;
            min-width: 110px;
        }

        .send-message-form input {
            flex: 1;
            padding: 0.7rem 1rem;
            border: 1px solid #d0dae8;
            border-radius: 40px;
            background: white;
            outline: none;
            font-size: 0.95rem;
        }

        .send-message-form input:focus {
            border-color: #4f83b9;
            box-shadow: 0 0 0 3px rgba(60, 110, 180, 0.1);
        }

        .btn-send {
            background: #1f3b57;
            border: none;
            color: white;
            padding: 0.7rem 1.2rem;
            border-radius: 40px;
            font-weight: 500;
            cursor: pointer;
            transition: 0.15s;
        }

        .btn-send:hover {
            background: #143049;
        }

        .btn-send i {
            margin-right: 0.3rem;
        }

        .friend-count-badge {
            background: #d4e2f5;
            color: #1f3b57;
            font-size: 0.75rem;
            font-weight: 600;
            padding: 0.1rem 0.7rem;
            border-radius: 40px;
            margin-left: 0.5rem;
        }

        .footer-hint {
            margin-top: 1.2rem;
            font-size: 0.8rem;
            color: #5b7393;
            text-align: right;
            border-top: 1px solid rgba(60, 80, 120, 0.1);
            padding-top: 0.8rem;
            opacity: 0.7;
        }

        /* scroll */
        ::-webkit-scrollbar {
            width: 5px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.02);
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #b8cbdf;
            border-radius: 10px;
        }
    </style>
</head>
<body>

<div class="app-card">
    <h1>
        <i class="fas fa-comment-dots"></i> 
        Message · Friends
        <span style="font-size: 0.9rem; font-weight: 400; margin-left: auto; background: #dce6f2; padding: 0.2rem 1rem; border-radius: 60px;">
            <i class="fas fa-user-friends"></i> <span id="friendCountDisplay">0</span>
        </span>
    </h1>

    <div class="grid-2col">
        <!-- LEFT: Friends panel -->
        <div class="panel">
            <h2><i class="fas fa-user-plus"></i> Friends</h2>
            <div class="friend-list" id="friendListContainer">
                <!-- dynamic friend items -->
            </div>

            <div class="add-friend-form">
                <input type="text" id="friendNameInput" placeholder="Enter friend name..." />
                <button class="btn btn-primary" id="addFriendBtn"><i class="fas fa-plus"></i> Add</button>
            </div>
            <div style="margin-top: 0.8rem; display: flex; gap: 0.4rem; flex-wrap: wrap;">
                <button class="btn btn-outline" id="addSampleFriendsBtn" style="font-size:0.8rem;"><i class="fas fa-user-plus"></i> Add samples</button>
                <button class="btn btn-outline" id="clearAllFriendsBtn" style="font-size:0.8rem; color:#8f4a4a;"><i class="fas fa-user-slash"></i> Clear all</button>
            </div>
        </div>

        <!-- RIGHT: Messages panel -->
        <div class="panel">
            <h2><i class="fas fa-envelope"></i> Messages</h2>
            <div class="message-thread">
                <div class="message-list" id="messageListContainer">
                    <!-- dynamic messages -->
                </div>
            </div>

            <div class="send-message-form">
                <select id="messageRecipientSelect">
                    <option value="">— select friend —</option>
                </select>
                <input type="text" id="messageInput" placeholder="Type a message..." />
                <button class="btn-send" id="sendMessageBtn"><i class="fas fa-paper-plane"></i> Send</button>
            </div>
            <div style="margin-top: 0.6rem; display: flex; justify-content: flex-end;">
                <button class="btn btn-outline" id="clearMessagesBtn" style="font-size:0.75rem; padding:0.3rem 1rem;"><i class="fas fa-eraser"></i> Clear chat</button>
            </div>
        </div>
    </div>
    <div class="footer-hint">
        <i class="fas fa-database"></i> Data stored locally · messages & friends
    </div>
</div>

<script>
    (function() {
        // ----- state -----
        let friends = [];
        let messages = [];

        // ----- DOM refs -----
        const friendListEl = document.getElementById('friendListContainer');
        const messageListEl = document.getElementById('messageListContainer');
        const friendCountDisplay = document.getElementById('friendCountDisplay');
        const recipientSelect = document.getElementById('messageRecipientSelect');

        const friendNameInput = document.getElementById('friendNameInput');
        const addFriendBtn = document.getElementById('addFriendBtn');
        const addSampleBtn = document.getElementById('addSampleFriendsBtn');
        const clearAllFriendsBtn = document.getElementById('clearAllFriendsBtn');

        const messageInput = document.getElementById('messageInput');
        const sendMessageBtn = document.getElementById('sendMessageBtn');
        const clearMessagesBtn = document.getElementById('clearMessagesBtn');

        // ----- helpers: storage -----
        function saveState() {
            localStorage.setItem('friends_data', JSON.stringify(friends));
            localStorage.setItem('messages_data', JSON.stringify(messages));
        }

        function loadState() {
            const storedFriends = localStorage.getItem('friends_data');
            const storedMessages = localStorage.getItem('messages_data');
            if (storedFriends) {
                try { friends = JSON.parse(storedFriends); } catch(e){ friends = []; }
            } else {
                // default friends
                friends = ['Alice', 'Bob', 'Charlie'];
            }
            if (storedMessages) {
                try { messages = JSON.parse(storedMessages); } catch(e){ messages = []; }
            } else {
                // sample messages
                messages = [
                    { from: 'Alice', text: 'Hey! How are you?', timestamp: Date.now() - 3600000 },
                    { from: 'Bob', text: 'Meeting at 5 pm?', timestamp: Date.now() - 1800000 },
                    { from: 'You', text: 'Sure, I\'ll be there', timestamp: Date.now() - 600000 }
                ];
            }
        }

        // ----- render functions -----
        function renderFriends() {
            // sort alphabetically
            const sorted = [...friends].sort((a,b) => a.localeCompare(b));
            if (sorted.length === 0) {
                friendListEl.innerHTML = `<div class="empty-state" style="padding:0.8rem;"><i class="fas fa-user-plus"></i> No friends yet</div>`;
            } else {
                let html = '';
                sorted.forEach(name => {
                    html += `
                        <div class="friend-item">
                            <span class="friend-name"><i class="fas fa-user-circle" style="margin-right:8px; color:#3a6ea5;"></i>${name}</span>
                            <div class="friend-actions">
                                <button class="btn-icon primary send-to-friend" data-name="${name}" title="Send message"><i class="fas fa-comment"></i></button>
                                <button class="btn-icon danger remove-friend" data-name="${name}" title="Remove friend"><i class="fas fa-user-minus"></i></button>
                            </div>
                        </div>
                    `;
                });
                friendListEl.innerHTML = html;
            }

            // update friend count badge
            friendCountDisplay.textContent = friends.length;

            // update recipient dropdown
            updateRecipientSelect();

            // attach event listeners for friend actions (remove + send)
            document.querySelectorAll('.remove-friend').forEach(btn => {
                btn.addEventListener('click', function(e) {
                    const name = this.dataset.name;
                    removeFriend(name);
                });
            });

            document.querySelectorAll('.send-to-friend').forEach(btn => {
                btn.addEventListener('click', function(e) {
                    const name = this.dataset.name;
                    // select in dropdown and focus message input
                    if (recipientSelect) {
                        recipientSelect.value = name;
                        messageInput.focus();
                    }
                });
            });
        }

        function renderMessages() {
            if (messages.length === 0) {
                messageListEl.innerHTML = `<div class="empty-state"><i class="fas fa-comment-slash"></i> No messages yet</div>`;
                return;
            }

            // show latest first (but we want chronological, let's show newest at bottom)
            // we'll display in order (oldest first) but scroll to bottom
            const sorted = [...messages].sort((a,b) => a.timestamp - b.timestamp);
            let html = '';
            sorted.forEach(msg => {
                const sender = msg.from || 'Unknown';
                const isYou = (sender === 'You');
                const timeStr = new Date(msg.timestamp).toLocaleTimeString([], { hour:'2-digit', minute:'2-digit' });
                const bubbleClass = isYou ? 'message-bubble' : 'message-bubble';
                html += `
                    <div class="${bubbleClass}" style="${isYou ? 'border-left-color: #6f9bcb;' : ''}">
                        <span class="msg-sender">${isYou ? 'You' : sender}</span>
                        <span class="msg-text">${escapeHtml(msg.text)}</span>
                        <span class="msg-time">${timeStr}</span>
                    </div>
                `;
            });
            messageListEl.innerHTML = html;
            // auto-scroll to bottom
            const container = messageListEl;
            container.scrollTop = container.scrollHeight;
        }

        // simple escape
        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        function updateRecipientSelect() {
            const currentVal = recipientSelect.value;
            recipientSelect.innerHTML = '<option value="">— select friend —</option>';
            const sorted = [...friends].sort((a,b) => a.localeCompare(b));
            sorted.forEach(name => {
                const opt = document.createElement('option');
                opt.value = name;
                opt.textContent = name;
                recipientSelect.appendChild(opt);
            });
            // restore if still valid
            if (currentVal && friends.includes(currentVal)) {
                recipientSelect.value = currentVal;
            }
        }

        // ----- actions -----
        function addFriend(name) {
            const trimmed = name.trim();
            if (!trimmed) return alert('Please enter a name.');
            if (friends.includes(trimmed)) {
                alert(`"${trimmed}" is already in your friends.`);
                return;
            }
            friends.push(trimmed);
            saveState();
            renderFriends();
            renderMessages(); // re-render messages in case of name change? no, but keep
        }

        function removeFriend(name) {
            if (!confirm(`Remove "${name}" from friends?`)) return;
            friends = friends.filter(f => f !== name);
            // remove all messages from this friend? we keep them (but we could filter)
            // we keep messages for history, but you can clear manually
            saveState();
            renderFriends();
            renderMessages();
        }

        function sendMessage() {
            const recipient = recipientSelect.value;
            if (!recipient) {
  
