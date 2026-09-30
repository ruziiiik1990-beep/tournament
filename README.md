
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Плей-офф турнира</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

body { margin:0; padding:0; background:transparent; font-family:'Inter',sans-serif; }

.tournament-wrapper {
  background-image:url('https://4ak4ak.moy.su/Tchak.jpg');
  background-size:cover; background-position:center center; background-repeat:no-repeat;
  padding:30px 0; border-radius:12px; position:relative; overflow:hidden;
}
.tournament-wrapper::before {
  content:''; position:absolute; top:0; left:0; width:100%; height:100%;
  background:rgba(0,0,0,0.5); z-index:0;
}
.tournament-wrapper > * { position:relative; z-index:1; }

.tournament-title {
  text-align:center; color:#fff; font-size:28px; font-weight:800;
  text-transform:uppercase; letter-spacing:2px; margin-bottom:24px;
  text-shadow:0 2px 8px rgba(0,0,0,0.8);
}

.bracket-row { display:flex; gap:16px; justify-content:center; align-items:stretch; flex-wrap:wrap; }
.bracket-col { flex:1; min-width:180px; max-width:22%; display:flex; flex-direction:column; }
.bracket-col.final-col { max-width:260px; }
.bracket-col.semifinal-col, .bracket-col.final-col { justify-content:center; }

.conf-label {
  text-align:center; font-size:16px; font-weight:700; text-transform:uppercase;
  letter-spacing:1px; margin-bottom:12px; padding-bottom:8px;
  border-bottom:1px solid rgba(255,255,255,0.2);
}
.conf-label.west { color:#6fb3ff; }
.conf-label.east { color:#ff8a65; }

.round-label {
  text-align:center; color:rgba(255,255,255,0.55); font-size:12px;
  font-weight:600; text-transform:uppercase; letter-spacing:0.8px; margin-bottom:8px;
}

.match {
  display:flex; flex-direction:column; background:rgba(255,255,255,0.06);
  border-radius:6px; overflow:hidden; margin:0 auto 8px; width:100%;
  border:1px solid rgba(255,255,255,0.08); box-sizing:border-box;
  cursor:pointer; transition:border-color 0.2s, background 0.2s, transform 0.15s;
}
.match:hover { border-color:rgba(255,215,0,0.4); background:rgba(255,255,255,0.1); transform:translateY(-2px); }

.match-row {
  display:flex; justify-content:space-between; align-items:center;
  padding:8px 12px; color:#fff; font-size:13px; font-weight:500; transition:background 0.2s;
}
.match-row.winner { background:rgba(76,175,80,0.3); font-weight:700; }
.match-row.winner::after { content:'\2713'; color:#66bb6a; margin-left:6px; }
.match-team { display:flex; align-items:center; gap:6px; }
.team-seed {
  background:rgba(255,255,255,0.12); border-radius:3px; padding:2px 5px;
  font-size:11px; font-weight:600; color:rgba(255,255,255,0.7); min-width:18px; text-align:center;
}
.match-score { font-weight:700; font-size:14px; min-width:22px; text-align:center; }
.match-divider { height:1px; background:rgba(255,255,255,0.08); }

.winner-logo { display:flex; justify-content:center; padding:4px 0 6px; background:rgba(76,175,80,0.15); }
.winner-logo img { width:24px; height:24px; border-radius:50%; border:2px solid #66bb6a; box-shadow:0 0 8px rgba(102,187,106,0.6); }

.final-box {
  background:rgba(0,0,0,0.55); border-radius:12px; padding:20px;
  border:2px solid rgba(255,215,0,0.4); text-align:center; width:100%; box-sizing:border-box;
}
.final-title {
  color:#ffd700; font-size:18px; font-weight:800; text-transform:uppercase;
  letter-spacing:1.5px; margin-bottom:14px; text-shadow:0 2px 8px rgba(0,0,0,0.8);
}
.final-match { background:rgba(255,215,0,0.08); border:1px solid rgba(255,215,0,0.2); }
.final-match:hover { border-color:rgba(255,215,0,0.6); }

/* Join + Admin */
.join-section { text-align:center; margin:24px 0 20px; }
.btn-join {
  display:inline-block; padding:14px 40px; font-size:17px; font-weight:700;
  color:#fff; background:linear-gradient(135deg,#0a2a6b,#1a4a8b);
  border:2px solid rgba(255,255,255,0.3); border-radius:50px; cursor:pointer;
  transition:transform 0.2s, box-shadow 0.2s, background 0.3s;
  box-shadow:0 4px 14px rgba(10,42,107,0.4); font-family:'Inter',sans-serif;
}
.btn-join:hover { transform:translateY(-2px); box-shadow:0 8px 22px rgba(10,42,107,0.6); background:linear-gradient(135deg,#1a4a8b,#2a6abb); }
.joined-msg { text-align:center; color:#4a9eff; font-weight:700; font-size:17px; margin-bottom:20px; text-shadow:0 0 10px rgba(74,158,255,0.5); }
.guest-warning { text-align:center; margin-bottom:20px; }
.guest-warning-text {
  font-weight:800; background:linear-gradient(180deg,transparent 45%,#0a2a6bcc 45%);
  padding:3px 10px; border-radius:5px; color:#fff;
  text-shadow:0 0 5px #0a2a6b, 0 0 10px #0a2a6b; display:inline-block; font-size:15px;
}
.admin-login-row { text-align:center; margin:20px 0; display:flex; justify-content:center; gap:10px; flex-wrap:wrap; }
.admin-login-row input {
  padding:12px 18px; border:1px solid rgba(255,255,255,0.2); border-radius:8px;
  background:rgba(0,0,0,0.4); color:#fff; font-size:14px; width:200px; font-family:'Inter',sans-serif;
}
.admin-login-row input::placeholder { color:rgba(255,255,255,0.4); }
.admin-panel {
  max-width:850px; margin:0 auto 20px; padding:22px;
  background:rgba(10,42,107,0.3); backdrop-filter:blur(8px); -webkit-backdrop-filter:blur(8px);
  border:1px solid rgba(74,158,255,0.35); border-radius:14px; box-shadow:0 4px 20px rgba(10,42,107,0.3);
}
.admin-panel h3 { font-size:18px; margin-bottom:12px; color:#4a9eff; text-shadow:0 0 10px rgba(74,158,255,0.5); font-weight:700; }
.admin-hint { font-size:13px; color:rgba(255,255,255,0.5); }
.admin-active-badge {
  display:inline-block; background:linear-gradient(135deg,#27ae60,#2ecc71); color:#fff;
  font-size:12px; padding:4px 12px; border-radius:20px; font-weight:700;
  margin-left:10px; box-shadow:0 2px 8px rgba(39,174,96,0.4);
}

/* Modal */
.team-modal-overlay {
  display:none; position:fixed; top:0; left:0; width:100%; height:100%;
  background:rgba(0,0,0,0.75); z-index:9999; justify-content:center; align-items:center;
}
.team-modal-overlay.active { display:flex; }
.team-modal {
  background:#1a1a2e; border:1px solid rgba(255,255,255,0.15); border-radius:14px;
  padding:28px 32px; width:780px; max-width:92vw; max-height:88vh; overflow-y:auto;
  box-shadow:0 12px 40px rgba(0,0,0,0.6); animation:modalIn 0.25s ease;
}
@keyframes modalIn { from { transform:scale(0.92); opacity:0; } to { transform:scale(1); opacity:1; } }
.team-modal-header {
  display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;
  padding-bottom:14px; border-bottom:1px solid rgba(255,255,255,0.15);
}
.team-modal-matchup { font-size:18px; font-weight:800; color:#fff; text-transform:uppercase; letter-spacing:1px; }
.team-modal-close {
  background:rgba(255,255,255,0.1); border:none; color:#fff; font-size:22px;
  cursor:pointer; width:34px; height:34px; border-radius:50%;
  display:flex; align-items:center; justify-content:center; transition:background 0.2s; line-height:1;
}
.team-modal-close:hover { background:rgba(255,80,80,0.4); }

.participants-grid { display:flex; gap:16px; justify-content:space-between; align-items:flex-start; }
.participants-col { flex:1; min-width:0; display:flex; flex-direction:column; }
.participants-col-title {
  font-size:16px; font-weight:700; text-transform:uppercase; letter-spacing:1px;
  text-align:center; margin-bottom:14px; padding-bottom:10px;
  border-bottom:1px solid rgba(255,255,255,0.15);
}
.participants-col.left .participants-col-title { color:#6fb3ff; }
.participants-col.right .participants-col-title { color:#ff8a65; }

.participant-list { list-style:none; padding:0; margin:0 0 16px 0; display:flex; flex-direction:column; gap:8px; }
.participant-item {
  display:flex; align-items:center; gap:8px;
  background:rgba(255,255,255,0.06); border-radius:8px; padding:8px 10px;
  color:#fff; font-size:13px; font-weight:500;
}
.participant-avatar {
  width:32px; height:32px; border-radius:50%;
  display:flex; align-items:center; justify-content:center;
  font-size:14px; font-weight:700; color:#fff; flex-shrink:0;
}
.participants-col.left .participant-avatar { background:linear-gradient(135deg,#4a90d9,#6fb3ff); }
.participants-col.right .participant-avatar { background:linear-gradient(135deg,#e07845,#ff8a65); }
.participant-num { color:rgba(255,255,255,0.45); font-size:11px; font-weight:600; }

/* Score input */
.score-input {
  width:50px; padding:4px 6px; border:1px solid rgba(255,255,255,0.2); border-radius:4px;
  background:rgba(0,0,0,0.4); color:#fff; font-size:13px; text-align:center; font-family:'Inter',sans-serif;
}
.score-input::placeholder { color:rgba(255,255,255,0.3); }
.score-confirmed { border-color:#66bb6a; background:rgba(76,175,80,0.2); }
.confirm-btn {
  width:24px; height:24px; border-radius:50%; border:2px solid rgba(255,255,255,0.3);
  cursor:pointer; padding:0; overflow:hidden; flex-shrink:0; background:transparent;
  transition:border-color 0.2s, box-shadow 0.2s;
}
.confirm-btn img { width:100%; height:100%; object-fit:cover; border-radius:50%; }
.confirm-btn:hover { border-color:#4a9eff; box-shadow:0 0 8px rgba(74,158,255,0.5); }
.confirm-btn.confirmed { border-color:#66bb6a; box-shadow:0 0 8px rgba(102,187,106,0.5); }
.del-participant-btn {
  background:rgba(255,80,80,0.2); border:none; color:#ff6b6b; font-size:14px;
  cursor:pointer; width:22px; height:22px; border-radius:50%;
  display:none; align-items:center; justify-content:center; line-height:1; flex-shrink:0;
}
.participant-item:hover .del-participant-btn { display:flex; }
.del-participant-btn:hover { background:rgba(255,80,80,0.4); }

.admin-add-row { display:flex; gap:8px; margin-top:8px; }
.admin-add-row input {
  flex:1; padding:6px 10px; border:1px solid rgba(255,255,255,0.15); border-radius:6px;
  background:rgba(0,0,0,0.3); color:#fff; font-size:12px; font-family:'Inter',sans-serif;
}
.admin-add-row input::placeholder { color:rgba(255,255,255,0.3); }
.admin-add-row button {
  padding:6px 12px; border:none; border-radius:6px; font-size:12px; font-weight:600;
  cursor:pointer; white-space:nowrap; font-family:'Inter',sans-serif;
}
.admin-add-row button.add { background:#4a9eff; color:#fff; }
.admin-add-row button.add:hover { opacity:0.85; }

.winner-btn-row { display:flex; gap:10px; justify-content:center; margin-top:16px; padding-top:14px; border-top:1px solid rgba(255,255,255,0.1); }
.winner-btn {
  padding:10px 20px; border:2px solid rgba(255,215,0,0.4); border-radius:8px;
  background:rgba(255,215,0,0.1); color:#ffd700; font-size:13px; font-weight:700;
  cursor:pointer; transition:background 0.2s, border-color 0.2s; font-family:'Inter',sans-serif;
}
.winner-btn:hover { background:rgba(255,215,0,0.2); border-color:rgba(255,215,0,0.6); }
.winner-btn.left { border-color:rgba(111,179,255,0.4); color:#6fb3ff; background:rgba(111,179,255,0.1); }
.winner-btn.left:hover { background:rgba(111,179,255,0.2); border-color:rgba(111,179,255,0.6); }
.winner-btn.right { border-color:rgba(255,138,101,0.4); color:#ff8a65; background:rgba(255,138,101,0.1); }
.winner-btn.right:hover { background:rgba(255,138,101,0.2); border-color:rgba(255,138,101,0.6); }

.map-center { flex:0 0 auto; width:140px; display:flex; flex-direction:column; align-items:center; gap:10px; padding-top:40px; }
.map-label { font-size:11px; font-weight:700; color:rgba(255,215,0,0.7); text-transform:uppercase; letter-spacing:1px; text-align:center; }
.map-image { width:120px; height:120px; border-radius:10px; object-fit:cover; border:2px solid rgba(255,215,0,0.3); box-shadow:0 4px 16px rgba(0,0,0,0.4); }
.map-vs { font-size:22px; font-weight:800; color:rgba(255,215,0,0.6); text-align:center; }
.map-name { font-size:12px; font-weight:600; color:rgba(255,255,255,0.8); text-align:center; text-transform:uppercase; letter-spacing:0.5px; }
.map-placeholder {
  width:120px; height:120px; border-radius:10px; border:2px dashed rgba(255,255,255,0.2);
  display:flex; align-items:center; justify-content:center; text-align:center;
  font-size:11px; color:rgba(255,255,255,0.4); line-height:1.4; padding:10px;
}
.map-name.empty { color:rgba(255,255,255,0.3); }

/* Chat */
.common-chat-container { width:100%; margin-top:20px; border-top:1px solid rgba(255,255,255,0.1); padding-top:16px; }
.chat-title { font-size:14px; font-weight:700; text-transform:uppercase; letter-spacing:0.8px; margin-bottom:12px; color:#fff; text-align:center; }
.chat-messages { max-height:200px; overflow-y:auto; display:flex; flex-direction:column; gap:8px; margin-bottom:16px; padding:4px 0; }
.chat-messages::-webkit-scrollbar { width:6px; }
.chat-messages::-webkit-scrollbar-track { background:rgba(255,255,255,0.05); }
.chat-messages::-webkit-scrollbar-thumb { background:rgba(255,255,255,0.2); border-radius:3px; }
.chat-msg {
  background:rgba(255,255,255,0.08); border-radius:10px; padding:10px 14px;
  font-size:13px; line-height:1.4; word-break:break-word; position:relative;
  max-width:85%; align-self:flex-start; box-shadow:0 1px 3px rgba(0,0,0,0.2);
}
.chat-msg.team-west { background:rgba(74,144,217,0.15); border-left:3px solid #6fb3ff; align-self:flex-start; }
.chat-msg.team-east { background:rgba(224,120,69,0.15); border-right:3px solid #ff8a65; align-self:flex-end; }
.chat-msg.team-admin { background:rgba(255,215,0,0.15); border-left:3px solid #ffd700; align-self:center; max-width:90%; }
.chat-msg-author { font-size:11px; font-weight:700; margin-bottom:4px; display:block; }
.chat-msg-author.west { color:#6fb3ff; }
.chat-msg-author.east { color:#ff8a65; }
.chat-msg-author.admin { color:#ffd700; }
.chat-msg-text { font-weight:400; color:rgba(255,255,255,0.9); }
.chat-msg-time { font-size:9px; color:rgba(255,255,255,0.3); margin-top:4px; }
.chat-msg-delete {
  position:absolute; top:6px; right:6px; background:rgba(255,80,80,0.2);
  border:none; color:#ff6b6b; font-size:14px; cursor:pointer;
  width:24px; height:24px; border-radius:50%; display:none;
  align-items:center; justify-content:center; line-height:1;
}
.chat-msg:hover .chat-msg-delete { display:flex; }
.chat-msg-delete:hover { background:rgba(255,80,80,0.4); }
.chat-input-row { display:flex; gap:10px; }
.chat-input {
  flex:1; background:rgba(255,255,255,0.08); border:1px solid rgba(255,255,255,0.1);
  border-radius:8px; padding:12px; color:#fff; font-size:14px; font-family:'Inter',sans-serif;
  outline:none; transition:border-color 0.2s;
}
.chat-input:focus { border-color:rgba(255,215,0,0.4); }
.chat-input::placeholder { color:rgba(255,255,255,0.3); }
.chat-send {
  background:rgba(255,215,0,0.15); border:1px solid rgba(255,215,0,0.3); border-radius:8px;
  padding:0 20px; color:#ffd700; font-size:14px; font-weight:600; cursor:pointer;
  transition:background 0.2s; white-space:nowrap; font-family:'Inter',sans-serif;
}
.chat-send:hover { background:rgba(255,215,0,0.25); }
.admin-badge {
  display:inline-block; background:rgba(255,215,0,0.2); color:#ffd700;
  font-size:9px; font-weight:700; padding:1px 5px; border-radius:3px; margin-left:4px; vertical-align:middle;
}
.chat-loading { text-align:center; font-size:12px; color:rgba(255,255,255,0.3); padding:16px; }

@media (max-width:900px) {
  .bracket-row { flex-direction:column; align-items:center; }
  .bracket-col { max-width:100%; width:100%; }
  .tournament-title { font-size:22px; }
  .match { max-width:100%; }
  .participants-grid { flex-direction:column; align-items:center; gap:20px; }
  .map-center { padding-top:0; }
  .team-modal { width:100%; padding:20px; }
  .chat-msg { max-width:95%; }
}
</style>
</head>
<body>

<div class="tournament-wrapper">
  <div class="tournament-title">Плей-офф турнира</div>
  <div class="bracket-row" id="bracketRow"></div>
</div>

<div class="join-section" id="joinSection"></div>

<div class="admin-login-row">
  <input type="password" id="adminPassInput" placeholder="Админ-пароль" onkeydown="if(event.key==='Enter') toggleAdmin()">
  <button class="btn-join" style="padding:12px 26px; font-size:14px;" onclick="toggleAdmin()">Войти как админ</button>
</div>

<div class="admin-panel" id="adminPanel" style="display:none;">
  <h3>Админ-панель <span class="admin-active-badge">АКТИВЕН</span></h3>
  <div class="admin-hint">Открой любой матч кликом — там появятся кнопки добавления/удаления участников и подтверждения победителя.</div>
</div>

<div class="team-modal-overlay" id="teamModalOverlay">
  <div class="team-modal" id="teamModal">
    <div class="team-modal-header">
      <span class="team-modal-matchup" id="teamModalMatchup">Матч</span>
      <button class="team-modal-close" id="teamModalClose">&times;</button>
    </div>
    <div class="participants-grid">
      <div class="participants-col left">
        <div class="participants-col-title" id="leftTeamName">Команда 1</div>
        <ul class="participant-list" id="leftParticipantList"></ul>
        <div class="admin-add-row" id="leftAddRow" style="display:none;">
          <input type="text" id="leftAddInput" placeholder="Ник игрока" maxlength="20" onkeydown="if(event.key==='Enter') addParticipant('left')">
          <button class="add" onclick="addParticipant('left')">Добавить</button>
        </div>
      </div>
      <div class="map-center">
        <div class="map-label">Карта</div>
        <div class="map-vs">VS</div>
        <div id="mapContainer">
          <img class="map-image" id="mapImage" src="" alt="Карта" style="display:none;">
          <div class="map-placeholder" id="mapPlaceholder">Карта будет выбрана, когда обе команды укомплектованы</div>
        </div>
        <div class="map-name" id="mapName"></div>
      </div>
      <div class="participants-col right">
        <div class="participants-col-title" id="rightTeamName">Команда 2</div>
        <ul class="participant-list" id="rightParticipantList"></ul>
        <div class="admin-add-row" id="rightAddRow" style="display:none;">
          <input type="text" id="rightAddInput" placeholder="Ник игрока" maxlength="20" onkeydown="if(event.key==='Enter') addParticipant('right')">
          <button class="add" onclick="addParticipant('right')">Добавить</button>
        </div>
      </div>
    </div>
    <div class="winner-btn-row" id="winnerBtnRow" style="display:none;">
      <button class="winner-btn left" id="winnerLeftBtn" onclick="confirmWinner('left')">Подтвердить победу: <span id="winnerLeftName"></span></button>
      <button class="winner-btn right" id="winnerRightBtn" onclick="confirmWinner('right')">Подтвердить победу: <span id="winnerRightName"></span></button>
    </div>
    <div class="common-chat-container">
      <div class="chat-title">Общий чат матча</div>
      <div class="chat-messages" id="commonChatMessages">
        <div class="chat-loading">Загрузка сообщений...</div>
      </div>
      <div class="chat-input-row">
        <input type="text" class="chat-input" id="commonChatInput" placeholder="Напишите сообщение..." maxlength="200">
        <button class="chat-send" id="commonChatSend">Отправить</button>
      </div>
    </div>
  </div>
</div>

<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-database-compat.js"></script>

<script>
(function() {
  var firebaseConfig = {
    apiKey: "AIzaSyCNQ0WFAiQjnISQjXJnHoln-wI64G2BqWs",
    authDomain: "ak4ak-d948e.firebaseapp.com",
    databaseURL: "https://ak4ak-d948e-default-rtdb.firebaseio.com",
    projectId: "ak4ak-d948e",
    storageBucket: "ak4ak-d948e.firebasestorage.app",
    messagingSenderId: "787151252619",
    appId: "1:787151252619:web:05eff65dc74b01d6e8f88e",
    measurementId: "G-DBB4YBNF2Q"
  };
  firebase.initializeApp(firebaseConfig);
  var db = firebase.database();

  var ADMIN_PASSWORD = '12$sacreD';
  var REQUIRED = 5;
  var LOGO_URL = 'https://4ak4ak.moy.su/logo1.jpg';
  var isAdmin = false;
  var myNick = null;
  var currentMatchId = null;
  var chatListenerRef = null;
  var matchData = {};

  var maps = [
    { url:'https://4ak4ak.moy.su/dust2.png', name:'Dust 2' },
    { url:'https://4ak4ak.moy.su/360fx360f.png', name:'Nuke' },
    { url:'https://4ak4ak.moy.su/ancient.jpg', name:'Ancient' },
    { url:'https://4ak4ak.moy.su/anubis.png', name:'Anubis' },
    { url:'https://4ak4ak.moy.su/cache.png', name:'Cache' },
    { url:'https://4ak4ak.moy.su/inferno.png', name:'Inferno' },
    { url:'https://4ak4ak.moy.su/mirage.png', name:'Mirage' },
    { url:'https://4ak4ak.moy.su/vertigo.png', name:'Vertigo' }
  ];

  var MATCH_DEFS = {
    'QF-W1': { leftSeed:'1', rightSeed:'4', leftDef:'Команда A', rightDef:'Команда B', conf:'west', next:'SF-W', nextSide:'left' },
    'QF-W2': { leftSeed:'2', rightSeed:'3', leftDef:'Команда C', rightDef:'Команда D', conf:'west', next:'SF-W', nextSide:'right' },
    'QF-E1': { leftSeed:'1', rightSeed:'4', leftDef:'Команда E', rightDef:'Команда F', conf:'east', next:'SF-E', nextSide:'left' },
    'QF-E2': { leftSeed:'2', rightSeed:'3', leftDef:'Команда G', rightDef:'Команда H', conf:'east', next:'SF-E', nextSide:'right' },
    'SF-W':  { leftSeed:'', rightSeed:'', leftDef:'', rightDef:'', conf:'west', next:'F', nextSide:'left' },
    'SF-E':  { leftSeed:'', rightSeed:'', leftDef:'', rightDef:'', conf:'east', next:'F', nextSide:'right' },
    'F':     { leftSeed:'', rightSeed:'', leftDef:'', rightDef:'', conf:null, next:null, nextSide:null }
  };

  var QF_IDS = ['QF-W1','QF-W2','QF-E1','QF-E2'];

  function getUrlParam(name) {
    var url = new URL(window.location.href);
    return url.searchParams.get(name);
  }

  myNick = getUrlParam('user');

  function escapeHtml(text) {
    if (!text) return '';
    return String(text).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;").replace(/'/g,"&#039;");
  }

  function formatTime(ts) {
    var d = new Date(ts);
    return String(d.getHours()).padStart(2,'0') + ':' + String(d.getMinutes()).padStart(2,'0');
  }

  // Initialize matches in Firebase if not present
  function initMatches() {
    var ref = db.ref('playoff/matches');
    ref.once('value').then(function(snap) {
      var existing = snap.val() || {};
      var updates = {};
      Object.keys(MATCH_DEFS).forEach(function(mid) {
        if (!existing[mid]) {
          updates[mid] = {
            leftTeam: MATCH_DEFS[mid].leftDef,
            rightTeam: MATCH_DEFS[mid].rightDef,
            leftSeed: MATCH_DEFS[mid].leftSeed,
            rightSeed: MATCH_DEFS[mid].rightSeed,
            leftParticipants: [],
            rightParticipants: [],
            leftScores: {},
            rightScores: {},
            leftConfirmed: [],
            rightConfirmed: [],
            winner: null,
            map: null
          };
        }
      });
      if (Object.keys(updates).length > 0) {
        ref.update(updates);
      }
    });
  }

  initMatches();

  // Listen to all match data
  db.ref('playoff/matches').on('value', function(snap) {
    matchData = snap.val() || {};
    renderBracket();
    if (currentMatchId && matchData[currentMatchId]) {
      renderModalContent(currentMatchId);
    }
    updateJoinSection();
  });

  function renderBracket() {
    var row = document.getElementById('bracketRow');
    var html = '';

    // West QF column
    html += '<div class="bracket-col">';
    html += '<div class="conf-label west">Запад</div>';
    html += '<div class="round-label">Четвертьфиналы</div>';
    html += renderMatch('QF-W1');
    html += renderMatch('QF-W2');
    html += '</div>';

    // West SF column
    html += '<div class="bracket-col semifinal-col">';
    html += '<div class="round-label">Полуфинал</div>';
    html += renderMatch('SF-W');
    html += '</div>';

    // Final column
    html += '<div class="bracket-col final-col">';
    html += '<div class="final-box">';
    html += '<div class="final-title">Финал</div>';
    html += renderMatch('F', true);
    html += '</div>';
    html += '</div>';

    // East SF column
    html += '<div class="bracket-col semifinal-col">';
    html += '<div class="round-label">Полуфинал</div>';
    html += renderMatch('SF-E');
    html += '</div>';

    // East QF column
    html += '<div class="bracket-col">';
    html += '<div class="conf-label east">Восток</div>';
    html += '<div class="round-label">Четвертьфиналы</div>';
    html += renderMatch('QF-E1');
    html += renderMatch('QF-E2');
    html += '</div>';

    row.innerHTML = html;

    // Attach click handlers
    var matches = row.querySelectorAll('.match');
    for (var i = 0; i < matches.length; i++) {
      matches[i].addEventListener('click', function(e) {
        e.preventDefault();
        openModal(this.getAttribute('data-match-id'));
      });
    }
  }

  function renderMatch(matchId, isFinal) {
    var md = matchData[matchId] || {};
    var def = MATCH_DEFS[matchId];
    var leftTeam = md.leftTeam || def.leftDef || 'TBD';
    var rightTeam = md.rightTeam || def.rightDef || 'TBD';
    var leftSeed = md.leftSeed || def.leftSeed || '';
    var rightSeed = md.rightSeed || def.rightSeed || '';
    var winner = md.winner;

    var cls = isFinal ? 'match final-match' : 'match';

    var leftScore = '';
    var rightScore = '';
    if (md.leftScore !== undefined && md.rightScore !== undefined && (md.leftScore || md.rightScore)) {
      leftScore = '<div class="match-score">' + (md.leftScore || 0) + '</div>';
      rightScore = '<div class="match-score">' + (md.rightScore || 0) + '</div>';
    }

    var leftWinClass = winner === 'left' ? ' winner' : '';
    var rightWinClass = winner === 'right' ? ' winner' : '';

    var html = '<div class="' + cls + '" data-match-id="' + matchId + '">';
    html += '<div class="match-row' + leftWinClass + '"><div class="match-team">' + (leftSeed ? '<span class="team-seed">' + leftSeed + '</span> ' : '') + escapeHtml(leftTeam) + '</div>' + leftScore + '</div>';
    html += '<div class="match-divider"></div>';
    html += '<div class="match-row' + rightWinClass + '"><div class="match-team">' + (rightSeed ? '<span class="team-seed">' + rightSeed + '</span> ' : '') + escapeHtml(rightTeam) + '</div>' + rightScore + '</div>';

    // Winner logo
    if (winner === 'left' || winner === 'right') {
      html += '<div class="winner-logo"><img src="' + LOGO_URL + '" alt="Победитель"></div>';
    }

    html += '</div>';
    return html;
  }

  function updateJoinSection() {
    var section = document.getElementById('joinSection');
    if (!myNick || myNick === 'null' || myNick === '') {
      section.innerHTML = '<p class="guest-warning"><span class="guest-warning-text">Войдите на сайт, чтобы участвовать в турнире</span></p>';
      return;
    }

    // Check if already in any QF team
    var alreadyIn = false;
    var myTeamName = '';
    for (var i = 0; i < QF_IDS.length; i++) {
      var md = matchData[QF_IDS[i]];
      if (!md) continue;
      var lp = md.leftParticipants || [];
      var rp = md.rightParticipants || [];
      for (var j = 0; j < lp.length; j++) {
        if (lp[j] && lp[j].toLowerCase() === myNick.toLowerCase()) { alreadyIn = true; myTeamName = md.leftTeam || MATCH_DEFS[QF_IDS[i]].leftDef; }
      }
      for (var j = 0; j < rp.length; j++) {
        if (rp[j] && rp[j].toLowerCase() === myNick.toLowerCase()) { alreadyIn = true; myTeamName = md.rightTeam || MATCH_DEFS[QF_IDS[i]].rightDef; }
      }
    }

    if (alreadyIn) {
      section.innerHTML = '<p class="joined-msg">Ты в игре! Команда: ' + escapeHtml(myTeamName) + '</p>';
    } else {
      // Check if there's space
      var hasSpace = false;
      for (var i = 0; i < QF_IDS.length; i++) {
        var md = matchData[QF_IDS[i]];
        if (!md) continue;
        if (!md.leftParticipants || md.leftParticipants.length < REQUIRED) hasSpace = true;
        if (!md.rightParticipants || md.rightParticipants.length < REQUIRED) hasSpace = true;
      }
      if (hasSpace) {
        section.innerHTML = '<button class="btn-join" onclick="joinTournament()">Участвовать</button>';
      } else {
        section.innerHTML = '<p class="joined-msg">Все команды заполнены!</p>';
      }
    }
  }

  // Join: randomly assign to a QF team with space
  window.joinTournament = function() {
    if (!myNick) { alert('Войдите на сайт!'); return; }

    // Collect available slots
    var slots = [];
    for (var i = 0; i < QF_IDS.length; i++) {
      var md = matchData[QF_IDS[i]];
      if (!md) continue;
      var lp = md.leftParticipants || [];
      var rp = md.rightParticipants || [];
      if (lp.length < REQUIRED) slots.push({ matchId: QF_IDS[i], side: 'left' });
      if (rp.length < REQUIRED) slots.push({ matchId: QF_IDS[i], side: 'right' });
    }

    if (slots.length === 0) { alert('Все команды заполнены!'); return; }

    // Random pick
    var pick = slots[Math.floor(Math.random() * slots.length)];
    var ref = db.ref('playoff/matches/' + pick.matchId + '/' + pick.side + 'Participants');
    var arr = matchData[pick.matchId][pick.side + 'Participants'] || [];
    arr.push(myNick);
    ref.set(arr);
  };

  // Modal
  var overlay = document.getElementById('teamModalOverlay');
  var matchupEl = document.getElementById('teamModalMatchup');
  var leftNameEl = document.getElementById('leftTeamName');
  var rightNameEl = document.getElementById('rightTeamName');
  var leftListEl = document.getElementById('leftParticipantList');
  var rightListEl = document.getElementById('rightParticipantList');
  var mapImgEl = document.getElementById('mapImage');
  var mapPlaceholderEl = document.getElementById('mapPlaceholder');
  var mapNameEl = document.getElementById('mapName');
  var closeBtn = document.getElementById('teamModalClose');
  var commonChatInput = document.getElementById('commonChatInput');
  var commonChatSend = document.getElementById('commonChatSend');

  function openModal(matchId) {
    currentMatchId = matchId;
    renderModalContent(matchId);

    // Chat
    if (chatListenerRef) chatListenerRef.off();
    chatListenerRef = db.ref('playoff/chats/' + matchId);
    chatListenerRef.on('value', renderChat);

    overlay.classList.add('active');
  }

  function renderModalContent(matchId) {
    var md = matchData[matchId];
    if (!md) return;
    var def = MATCH_DEFS[matchId];
    var leftTeam = md.leftTeam || def.leftDef || 'TBD';
    var rightTeam = md.rightTeam || def.rightDef || 'TBD';

    matchupEl.textContent = leftTeam + ' vs ' + rightTeam;
    leftNameEl.textContent = leftTeam;
    rightNameEl.textContent = rightTeam;

    // Participants
    leftListEl.innerHTML = buildParticipantList(md.leftParticipants || [], md.leftScores || {}, md.leftConfirmed || [], 'left', matchId);
    rightListEl.innerHTML = buildParticipantList(md.rightParticipants || [], md.rightScores || {}, md.rightConfirmed || [], 'right', matchId);

    // Admin add rows
    document.getElementById('leftAddRow').style.display = isAdmin ? 'flex' : 'none';
    document.getElementById('rightAddRow').style.display = isAdmin ? 'flex' : 'none';

    // Winner buttons
    var winnerRow = document.getElementById('winnerBtnRow');
    if (isAdmin && !md.winner) {
      winnerRow.style.display = 'flex';
      document.getElementById('winnerLeftName').textContent = leftTeam;
      document.getElementById('winnerRightName').textContent = rightTeam;
    } else {
      winnerRow.style.display = 'none';
    }

    // Map
    checkAndAssignMap(matchId, md);
    var map = md.map;
    if (map && map.url) {
      mapImgEl.src = map.url; mapImgEl.alt = map.name; mapImgEl.style.display = 'block';
      mapPlaceholderEl.style.display = 'none'; mapNameEl.textContent = map.name; mapNameEl.classList.remove('empty');
    } else {
      mapImgEl.style.display = 'none'; mapPlaceholderEl.style.display = 'flex';
      mapNameEl.textContent = 'Не выбрана'; mapNameEl.classList.add('empty');
    }
  }

  function buildParticipantList(parts, scores, confirmed, side, matchId) {
    var html = '';
    var isLeft = side === 'left';

    for (var i = 0; i < REQUIRED; i++) {
      var nick = parts[i] || '';
      var num = i + 1;
      var score = scores[nick] || '';
      var isConfirmed = confirmed.indexOf(nick) !== -1;
      var isMe = nick && myNick && nick.toLowerCase() === myNick.toLowerCase();

      if (nick) {
        var avatarBg = isLeft ? 'linear-gradient(135deg,#4a90d9,#6fb3ff)' : 'linear-gradient(135deg,#e07845,#ff8a65)';
        var delBtn = isAdmin ? '<button class="del-participant-btn" onclick="removeParticipant(\'' + side + '\',' + i + ')">&times;</button>' : '';
        var confirmBtn = isMe
          ? '<button class="confirm-btn' + (isConfirmed ? ' confirmed' : '') + '" onclick="confirmScore(\'' + side + '\',\'' + escapeHtml(nick) + '\')"><img src="' + LOGO_URL + '" alt="OK"></button>'
          : '';
        var scoreInput = isMe
          ? '<input type="number" class="score-input' + (isConfirmed ? ' score-confirmed' : '') + '" placeholder="0" value="' + escapeHtml(score) + '" onchange="enterScore(\'' + side + '\',\'' + escapeHtml(nick) + '\',this.value)"' + (isConfirmed ? ' disabled' : '') + '>'
          : (score !== '' ? '<span style="color:#fff;font-weight:700;font-size:14px;">' + escapeHtml(score) + '</span>' : '');

        // Left team: score on RIGHT; Right team: score on LEFT
        if (isLeft) {
          html += '<li class="participant-item">' + delBtn + '<div class="participant-avatar" style="background:' + avatarBg + '">' + num + '</div><span class="participant-num">#' + num + '</span> ' + escapeHtml(nick) + '<div style="flex:1;"></div>' + scoreInput + confirmBtn + '</li>';
        } else {
          html += '<li class="participant-item">' + confirmBtn + scoreInput + '<div style="flex:1;"></div><div class="participant-avatar" style="background:' + avatarBg + '">' + num + '</div><span class="participant-num">#' + num + '</span> ' + escapeHtml(nick) + delBtn + '</li>';
        }
      } else {
        var avatarBg2 = isLeft ? 'linear-gradient(135deg,#333,#444)' : 'linear-gradient(135deg,#333,#444)';
        html += '<li class="participant-item" style="opacity:0.4;"><div class="participant-avatar" style="background:' + avatarBg2 + '">' + num + '</div><span class="participant-num">#' + num + '</span> <span style="color:rgba(255,255,255,0.3);">Свободное место</span></li>';
      }
    }
    return html;
  }

  function checkAndAssignMap(matchId, md) {
    var lp = md.leftParticipants || [];
    var rp = md.rightParticipants || [];
    if (lp.length >= REQUIRED && rp.length >= REQUIRED && !md.map) {
      var idx = Math.floor(Math.random() * maps.length);
      var mapData = maps[idx];
      db.ref('playoff/matches/' + matchId + '/map').set(mapData);
      md.map = mapData;
    }
  }

  // Score functions
  window.enterScore = function(side, nick, value) {
    if (!currentMatchId) return;
    var scoresRef = db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Scores');
    var scores = {};
    scores[nick] = parseInt(value) || 0;
    scoresRef.update(scores);
  };

  window.confirmScore = function(side, nick) {
    if (!currentMatchId || !myNick) return;
    if (nick.toLowerCase() !== myNick.toLowerCase()) return;
    var confRef = db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Confirmed');
    var conf = matchData[currentMatchId][side + 'Confirmed'] || [];
    if (conf.indexOf(nick) === -1) {
      conf.push(nick);
      confRef.set(conf);
    } else {
      conf = conf.filter(function(n) { return n !== nick; });
      confRef.set(conf);
    }
  };

  // Admin: add participant
  window.addParticipant = function(side) {
    if (!isAdmin || !currentMatchId) return;
    var inputId = side === 'left' ? 'leftAddInput' : 'rightAddInput';
    var name = document.getElementById(inputId).value.trim();
    if (!name) { alert('Введите ник!'); return; }
    var md = matchData[currentMatchId];
    var arr = (md[side + 'Participants'] || []).slice();
    if (arr.length >= REQUIRED) { alert('Команда заполнена!'); return; }
    arr.push(name);
    db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Participants').set(arr);
    document.getElementById(inputId).value = '';
  };

  // Admin: remove participant
  window.removeParticipant = function(side, index) {
    if (!isAdmin || !currentMatchId) return;
    var md = matchData[currentMatchId];
    var arr = (md[side + 'Participants'] || []).slice();
    if (index < 0 || index >= arr.length) return;
    var removed = arr.splice(index, 1)[0];
    db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Participants').set(arr);
    // Clean up scores/confirmed
    var scores = md[side + 'Scores'] || {};
    delete scores[removed];
    db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Scores').set(scores);
    var conf = (md[side + 'Confirmed'] || []).filter(function(n) { return n !== removed; });
    db.ref('playoff/matches/' + currentMatchId + '/' + side + 'Confirmed').set(conf);
  };

  // Admin: confirm winner
  window.confirmWinner = function(side) {
    if (!isAdmin || !currentMatchId) return;
    var md = matchData[currentMatchId];
    var def = MATCH_DEFS[currentMatchId];

    db.ref('playoff/matches/' + currentMatchId + '/winner').set(side);

    var winningTeam, winningParts, losingParts;
    if (side === 'left') {
      winningTeam = md.leftTeam || def.leftDef;
      winningParts = md.leftParticipants || [];
      losingParts = md.rightParticipants || [];
    } else {
      winningTeam = md.rightTeam || def.rightDef;
      winningParts = md.rightParticipants || [];
      losingParts = md.leftParticipants || [];
    }

    // Advance winning team to next match
    if (def.next) {
      var nextRef = db.ref('playoff/matches/' + def.next);
      var nextMd = matchData[def.next] || {};
      var updates = {};
      updates[def.nextSide + 'Team'] = winningTeam;
      updates[def.nextSide + 'Participants'] = winningParts.slice();
      updates[def.nextSide + 'Scores'] = {};
      updates[def.nextSide + 'Confirmed'] = [];
      nextRef.update(updates);
    }

    // Distribute losing players to SF teams randomly (only for QF matches)
    if (QF_IDS.indexOf(currentMatchId) !== -1) {
      var sfSlots = [];
      ['SF-W','SF-E'].forEach(function(sfId) {
        var sfMd = matchData[sfId] || {};
        var lp = sfMd.leftParticipants || [];
        var rp = sfMd.rightParticipants || [];
        if (lp.length < REQUIRED) sfSlots.push({ matchId: sfId, side: 'left', space: REQUIRED - lp.length });
        if (rp.length < REQUIRED) sfSlots.push({ matchId: sfId, side: 'right', space: REQUIRED - rp.length });
      });

      // Randomly distribute losing players
      var remainingLosers = losingParts.slice();
      // Shuffle slots
      sfSlots.sort(function() { return Math.random() - 0.5; });

      for (var s = 0; s < sfSlots.length && remainingLosers.length > 0; s++) {
        var slot = sfSlots[s];
        var sfMd = matchData[slot.matchId] || {};
        var arr = (sfMd[slot.side + 'Participants'] || []).slice();
        while (slot.space > 0 && remainingLosers.length > 0) {
          arr.push(remainingLosers.shift());
          slot.space--;
        }
        db.ref('playoff/matches/' + slot.matchId + '/' + slot.side + 'Participants').set(arr);
      }
    }

    alert('Победа подтверждена! Команда "' + winningTeam + '" проходит дальше.');
  };

  // Admin toggle
  window.toggleAdmin = function() {
    var pass = document.getElementById('adminPassInput').value;
    if (!isAdmin) {
      if (pass === ADMIN_PASSWORD) {
        isAdmin = true;
        document.getElementById('adminPassInput').value = '';
        document.getElementById('adminPanel').style.display = 'block';
        alert('Админ-режим включён!');
        if (currentMatchId) renderModalContent(currentMatchId);
      } else {
        alert('Неверный пароль!');
      }
    } else {
      isAdmin = false;
      document.getElementById('adminPanel').style.display = 'none';
      alert('Админ-режим выключен.');
      if (currentMatchId) renderModalContent(currentMatchId);
    }
  };

  // Chat
  function renderChat(snapshot) {
    var container = document.getElementById('commonChatMessages');
    if (!snapshot || !snapshot.exists()) {
      container.innerHTML = '<div style="font-size:12px;color:rgba(255,255,255,0.3);text-align:center;padding:16px;">Здесь пока тихо. Напишите что-нибудь!</div>';
      return;
    }
    var data = snapshot.val();
    var keys = Object.keys(data).sort(function(a, b) { return (data[a].time || 0) - (data[b].time || 0); });
    var html = '';
    for (var i = 0; i < keys.length; i++) {
      var msgKey = keys[i];
      var msg = data[msgKey];
      var authorClass = '', authorColorClass = '', badge = '';
      if (msg.isAdmin) { authorClass = 'team-admin'; authorColorClass = 'admin'; badge = '<span class="admin-badge">ADMIN</span>'; }
      else if (msg.side === 'left') { authorClass = 'team-west'; authorColorClass = 'west'; }
      else if (msg.side === 'right') { authorClass = 'team-east'; authorColorClass = 'east'; }
      var deleteBtn = isAdmin ? '<button class="chat-msg-delete" data-msg-key="' + escapeHtml(msgKey) + '">&times;</button>' : '';
      html += '<div class="chat-msg ' + authorClass + '">' + deleteBtn
        + '<div class="chat-msg-author ' + authorColorClass + '">' + escapeHtml(msg.author) + badge + '</div>'
        + '<div class="chat-msg-text">' + escapeHtml(msg.text) + '</div>'
        + '<div class="chat-msg-time">' + formatTime(msg.time) + '</div></div>';
    }
    container.innerHTML = html;
    container.scrollTop = container.scrollHeight;
    if (isAdmin) {
      var delBtns = container.querySelectorAll('.chat-msg-delete');
      for (var j = 0; j < delBtns.length; j++) {
        delBtns[j].addEventListener('click', function(e) {
          e.stopPropagation();
          var msgKey = this.getAttribute('data-msg-key');
          db.ref('playoff/chats/' + currentMatchId + '/' + msgKey).remove();
        });
      }
    }
  }

  function sendMessage() {
    var text = commonChatInput.value.trim();
    if (!text || !currentMatchId) return;
    var author = myNick || 'Гость';
    var side = 'left';
    // Determine side based on which team user is in
    var md = matchData[currentMatchId];
    if (md) {
      var lp = md.leftParticipants || [];
      var rp = md.rightParticipants || [];
      for (var i = 0; i < rp.length; i++) {
        if (rp[i] && rp[i].toLowerCase() === (myNick || '').toLowerCase()) { side = 'right'; break; }
      }
      for (var i = 0; i < lp.length; i++) {
        if (lp[i] && lp[i].toLowerCase() === (myNick || '').toLowerCase()) { side = 'left'; break; }
      }
    }
    db.ref('playoff/chats/' + currentMatchId).push({ author: author, text: text, time: Date.now(), isAdmin: isAdmin, side: side });
    commonChatInput.value = '';
  }

  commonChatSend.addEventListener('click', function(e) { e.stopPropagation(); sendMessage(); });
  commonChatInput.addEventListener('keydown', function(e) { if (e.key === 'Enter') { e.stopPropagation(); sendMessage(); } });

  closeBtn.addEventListener('click', function() {
    overlay.classList.remove('active');
    currentMatchId = null;
    if (chatListenerRef) { chatListenerRef.off(); chatListenerRef = null; }
  });
  overlay.addEventListener('click', function(e) { if (e.target === overlay) closeBtn.click(); });
  document.addEventListener('keydown', function(e) { if (e.key === 'Escape') closeBtn.click(); });

  var modalEl = document.querySelector('.team-modal');
  if (modalEl) modalEl.addEventListener('click', function(e) { e.stopPropagation(); });
})();
</script>
</body>
</html>
