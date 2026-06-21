<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Social · messages & friends</title>
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
        }

        body {
            background: linear-gradient(145deg, #f5f7fc 0%, #e9ecf3 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1.5rem;
        }

        .app-card {
            max-width: 1000px;
            width: 100%;
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 2.5rem;
            box-shadow: 0 20px 40px rgba(0, 10, 30, 0.2), 0 8px 16px rgba(0,0,0,0.05);
            padding: 2rem 2rem 2rem 2rem;
            border: 1px solid rgba(255,255,255,0.5);
            transition: all 0.2s ease;
        }

        h1 {
            font-weight: 600;
            font-size: 1.9rem;
            color: #1b2a41;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            letter-spacing: -0.3px;
            margin-bottom: 1.5rem;
            border-bottom: 2px solid rgba(60, 80, 120, 0.15);
            padding-bottom: 0.9rem;
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
  
