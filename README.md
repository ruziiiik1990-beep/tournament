<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Турнир</title>
<style>
@import url('https://googleapis.com');

body { margin: 0; padding: 0; background: transparent; font-family: 'Inter', sans-serif; overflow-y: auto; scrollbar-width: none; -ms-overflow-style: none; }
body::-webkit-scrollbar { display: none; }

.tournament-selector-row { max-width: 850px; margin: 0 auto 16px; padding: 0 20px; display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
.tournament-selector-row label { color: #fff; font-size: 14px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
.tournament-selector-row select { flex: 1; min-width: 200px; padding: 10px 14px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px; background: rgba(10,42,107,0.55); color: #fff; font-size: 14px; font-family: 'Inter', sans-serif; cursor: pointer; }
.tournament-selector-row select option { background: #1a1a2e; color: #fff; }
.tournament-create-row { max-width: 850px; margin: 0 auto 16px; padding: 0 20px; display: none; align-items: center; gap: 10px; flex-wrap: wrap; }
.tournament-create-row.visible { display: flex; }
.tournament-create-row input { flex: 1; min-width: 200px; padding: 10px 14px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px; background: rgba(10,42,107,0.55); color: #fff; font-size: 14px; font-family: 'Inter', sans-serif; }
.tournament-create-row input::placeholder { color: rgba(255,255,255,0.4); }
.btn-create { padding: 10px 24px; font-size: 14px; font-weight: 700; color: #fff; background: linear-gradient(135deg, #27ae60, #2ecc71); border: 2px solid rgba(255,255,255,0.3); border-radius: 50px; cursor: pointer; font-family: 'Inter', sans-serif; transition: transform 0.2s, box-shadow 0.2s; }
.btn-create:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(39,174,96,0.5); }
.btn-delete-tournament { padding: 10px 24px; font-size: 14px; font-weight: 700; color: #fff; background: linear-gradient(135deg, #c0392b, #e74c3c); border: 2px solid rgba(255,255,255,0.2); border-radius: 8px; cursor: pointer; font-family: 'Inter', sans-serif; transition: transform 0.2s, box-shadow 0.2s; display: none; }
.btn-delete-tournament.visible { display: inline-block; }
.btn-delete-tournament:hover { transform: translateY(-2px); box-shadow: 0 6px 22px rgba(192,57,43,0.5); }

.tournament-wrapper { background-image: url('https://moy.su'); background-size: cover; background-position: center center; background-repeat: no-repeat; padding: 30px 0; border-radius: 12px; position: relative; overflow: hidden; max-width: 1100px; margin: 0 auto; }
.tournament-wrapper::before { content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 0; }
.tournament-wrapper > * { position: relative; z-index: 1; }
.tournament-title { text-align: center; color: #fff; font-size: 28px; font-weight: 800; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 24px; text-shadow: 0 2px 8px rgba(0,0,0,0.8); }
.no-tournament-msg { text-align: center; color: rgba(255,255,255,0.5); font-size: 18px; font-weight: 600; padding: 60px 20px; max-width: 600px; margin: 0 auto; }

.bracket-row { display: flex; gap: 16px; justify-content: center; align-items: stretch; flex-wrap: wrap; }
.bracket-col { flex: 1; min-width: 180px; max-width: 22%; display: flex; flex-direction: column; }
.bracket-col.final-col { max-width: 260px; }
.bracket-col.semifinal-col, .bracket-col.final-col { justify-content: center; }

.conf-label { text-align: center; font-size: 16px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid rgba(255,255,255,0.2); }
.conf-label.west { color: #6fb3ff; }
.conf-label.east { color: #ff8a65; }
.round-label { text-align: center; color: rgba(255,255,255,0.55); font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 8px; }

.match { display: flex; flex-direction: column; background: rgba(255,255,255,0.06); border-radius: 6px; overflow: hidden; margin: 0 auto 8px; width: 100%; border: 1px solid rgba(255,255,255,0.08); box-sizing: border-box; cursor: pointer; transition: border-color 0.2s, background 0.2s, transform 0.15s; }
.match:hover { border-color: rgba(255,215,0,0.4); background: rgba(255,255,255,0.1); transform: translateY(-2px); }
.match.is-mine { border-color: rgba(74,158,255,0.6); box-shadow: 0 0 12px rgba(74,158,255,0.4); }

.match-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 12px; color: #fff; font-size: 13px; font-weight: 500; transition: background 0.2s; }
.match-row.winner { background: rgba(76,175,80,0.3); font-weight: 700; }
.match-row.winner::after { content: '\2713'; color: #66bb6a; margin-left: 6px; }
.match-row.is-mine-row { background: rgba(74,158,255,0.2); }
.match-team { display: flex; align-items: center; gap: 6px; }
.team-seed { background: rgba(255,255,255,0.12); border-radius: 3px; padding: 2px 5px; font-size: 11px; font-weight: 600; color: rgba(255,255,255,0.7); min-width: 18px; text-align: center; }
.match-score { font-weight: 700; font-size: 14px; min-width: 22px; text-align: center; }
.match-divider { height: 1px; background: rgba(255,255,255,0.08); }

.final-box { background: rgba(0,0,0,0.55); border-radius: 12px; padding: 20px; border: 2px solid rgba(255,215,0,0.4); text-align: center; width: 100%; box-sizing: border-box; }
.final-title { color: #ffd700; font-size: 18px; font-weight: 800; text-transform: uppercase; letter-spacing: 1.5px; margin-bottom: 14px; text-shadow: 0 2px 8px rgba(0,0,0,0.8); }
.final-match { background: rgba(255,215,0,0.08); border: 1px solid rgba(255,215,0,0.2); }
.final-match:hover { border-color: rgba(255,215,0,0.6); }

.join-section { text-align: center; margin: 24px 0 20px; max-width: 850px; margin-left: auto; margin-right: auto; padding: 0 20px; }
.btn-join { display: inline-block; padding: 14px 40px; font-size: 17px; font-weight: 700; color: #fff; background: linear-gradient(135deg, #0a2a6b, #1a4a8b); border: 2px solid rgba(255,255,255,0.3); border-radius: 50px; cursor: pointer; transition: transform 0.2s, box-shadow 0.2s, background 0.3s; box-shadow: 0 4px 14px rgba(10,42,107,0.4); font-family: 'Inter', sans-serif; }
.btn-join:hover { transform: translateY(-2px); box-shadow: 0 8px 22px rgba(10,42,107,0.6); background: linear-gradient(135deg, #1a4a8b, #2a6abb); }
.btn-join:disabled { opacity: 0.5; cursor: not-allowed; transform: none; box-shadow: none; }
.joined-msg { text-align: center; color: #4a9eff; font-weight: 700; font-size: 17px; margin-bottom: 20px; text-shadow: 0 0 10px rgba(74,158,255,0.5); }
.guest-warning { text-align: center; margin-bottom: 20px; }
.guest-warning-text { font-weight: 800; background: linear-gradient(180deg, transparent 45%, #0a2a6bcc 45%); padding: 3px 10px; border-radius: 5px; color: #fff; text-shadow: 0 0 5px #0a2a6b, 0 0 10px #0a2a6b; display: inline-block; font-size: 15px; }
.tournament-full-msg { text-align: center; color: rgba(255,215,0,0.7); font-weight: 700; font-size: 15px; margin-bottom: 20px; text-shadow: 0 0 10px rgba(255,215,0,0.3); }

.admin-login-row { text-align: center; margin: 20px 0; display: none; justify-content: center; gap: 10px; flex-wrap: wrap; max-width: 850px; margin-left: auto; margin-right: auto; padding: 0 20px; }
.admin-login-row.visible { display: flex; }
.admin-login-row input { padding: 12px 18px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px; background: rgba(0,0,0,0.4); color: #fff; font-size: 14px; width: 200px; font-family: 'Inter', sans-serif; }
.admin-login-row input::placeholder { color: rgba(255,255,255,0.4); }
.admin-panel { max-width: 850px; margin: 0 auto 20px; padding: 22px; background: rgba(10,42,107,0.3); backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px); border: 1px solid rgba(74,158,255,0.35); border-radius: 14px; box-shadow: 0 4px 20px rgba(10,42,107,0.3); display: none; }
.admin-panel h3 { font-size: 18px; margin-bottom: 12px; color: #4a9eff; text-shadow: 0 0 10px rgba(74,158,255,0.5); font-weight: 700; }
.admin-hint { font-size: 13px; color: rgba(255,255,255,0.5); }
.admin-active-badge { display: inline-block; background: linear-gradient(135deg, #27ae60, #2ecc71); color: #fff; font-size: 12px; padding: 4px 12px; border-radius: 20px; font-weight: 700; margin-left: 10px; box-shadow: 0 2px 8px rgba(39,174,96,0.4); }

.team-modal-overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.75); z-index: 9999; justify-content: center; align-items: center; }
.team-modal-overlay.active { display: flex; }
.team-modal { background: #1a1a2e; border: 1px solid rgba(255,255,255,0.15); border-radius: 14px; padding: 28px 32px; width: 800px; max-width: 92vw; max-height: 88vh; overflow-y: auto; box-shadow: 0 12px 40px rgba(0,0,0,0.6); animation: modalIn 0.25s ease; }
@keyframes modalIn { from { transform: scale(0.92); opacity: 0; } to { transform: scale(1); opacity: 1; } }
.team-modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 14px; border-bottom: 1px solid rgba(255,255,255,0.15); }
.team-modal-matchup { font-size: 18px; font-weight: 800; color: #fff; text-transform: uppercase; letter-spacing: 1px; }
.team-modal-close { background: rgba(255,255,255,0.1); border: none; color: #fff; font-size: 22px; cursor: pointer; width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center; transition: background 0.2s; line-height: 1; }
.team-modal-close:hover { background: rgba(255,80,80,0.4); }

<script>
(function() {
  var firebaseConfig = {
    apiKey: "AIzaSyCNQ0WFAiQjnISQjXJnHoln-wI64G2BqWs",
    authDomain: "://firebaseapp.com",
    databaseURL: "https://firebaseio.com",
    projectId: "ak4ak-d948e",
    storageBucket: "ak4ak-d948e.firebasestorage.app",
    messagingSenderId: "787151252619",
    appId: "1:787151252619:web:05eff65dc74b01d6e8f88e",
    measurementId: "G-DBB4YBNF2Q"
  };
  firebase.initializeApp(firebaseConfig);
  var db = firebase.database();

  var ADMIN_PASSWORD = '12\$sacreD';
  var MAX_PLAYERS = 5;
  var WIN_LOGO = 'https://moy.su';
  var myNick = new URL(window.location.href).searchParams.get('user');
  var isAdmin = false;
  var currentTournamentId = null;
  var currentMatchId = null;
  var chatRef = null;
  var allMatches = {};
  var tournaments = {};
  var prizesEnabled = false;
  var qfMatchIds = ['qf_w1','qf_w2','qf_e1','qf_e2'];
  var matchOrder = ['qf_w1','qf_w2','sf_w','final','sf_e','qf_e1','qf_e2'];

  var PRIZE_SKINS_WINNER = [
    { url: 'https://moy.su', name: 'AK-47 Полёт журавля' },
    { url: 'https://moy.su', name: 'M4A1-S Дикий тусовщик' }
  ];
  var PRIZE_SKINS_FINALIST = [
    { url: 'https://moy.su', name: 'AK-47 Колымага' },
    { url: 'https://moy.su', name: 'M4A1-S Взгляд в прошлое' }
  ];
  var maps = [
    { url: 'https://moy.su', name: 'Dust 2' },
    { url: 'https://moy.su', name: 'Mirage' },
    { url: 'https://moy.su', name: 'Inferno' }
  ];

  var qfTeams = {
    'qf_w1': { left: 'Команда A', right: 'Команда B' },
    'qf_w2': { left: 'Команда C', right: 'Команда D' },
    'qf_e1': { left: 'Команда E', right: 'Команда F' },
    'qf_e2': { left: 'Команда G', right: 'Команда H' }
  };
  var sfTeamNames = { 'sf_w': {left:'Команда I', right:'Команда J'}, 'sf_e': {left:'Команда K', right:'Команда L'} };
  var finalTeamNames = { left: 'Команда M', right: 'Команда N' };

  var sfSlots = [{sfId:'sf_w', sfSide:'left'}, {sfId:'sf_w', sfSide:'right'}, {sfId:'sf_e', sfSide:'left'}, {sfId:'sf_e', sfSide:'right'}];
  var finalSlots = [{finalSide:'left'}, {finalSide:'right'}];

  function tPath(sub) { return 'playoff/tournaments/' + currentTournamentId + (sub ? '/' + sub : ''); }
  db.ref('playoff/adminNicks').once('value').then(snap => { if (snap.val() && snap.val().indexOf(myNick) !== -1) document.getElementById('adminLoginRow').style.display = 'flex'; });

  function loadTournaments() {
    db.ref('playoff/tournaments').once('value').then(snap => {
      tournaments = snap.val() || {}; var sorted = Object.keys(tournaments);
      var sel = document.getElementById('tournamentSelect');
      sel.innerHTML = sorted.map(id => `<option value="${id}">${escapeHtml(tournaments[id].name)}</option>`).join('');
      if (sorted.length > 0) { currentTournamentId = sorted[0]; switchTournament(); }
    });
  }

  window.switchTournament = function() {
    currentTournamentId = document.getElementById('tournamentSelect').value; if (!currentTournamentId) return;
    document.getElementById('tournamentTitle').textContent = tournaments[currentTournamentId].name;
    document.getElementById('tournamentWrapper').style.display = 'block'; buildBracketHtml();
    db.ref(tPath('matches')).on('value', s => { allMatches = s.val() || {}; renderBracket(); updateJoinSection(); if (currentMatchId) updateModalContent(); });
  };

  window.createTournament = function() {
    var name = document.getElementById('newTournamentName').value.trim(); if (!name) return;
    var id = db.ref('playoff/tournaments').push().key;
    db.ref('playoff/tournaments/' + id).set({ name: name, createdAt: Date.now() }).then(loadTournaments);
  };

  window.toggleAdmin = function() {
    if (document.getElementById('adminPassInput').value === ADMIN_PASSWORD) {
      isAdmin = true;
      document.getElementById('adminPanel').style.display = 'block';
      document.getElementById('tournamentCreateRow').style.display = 'flex';
      document.getElementById('btnDeleteTournament').style.display = 'inline-block';
      renderBracket();
    }
  };

  function buildBracketHtml() {
    document.getElementById('bracketRow').innerHTML = matchOrder.map(id => `
      <div class="match" onclick="openModal('${id}')">
        <div class="match-row"><div>${id} Left</div><div class="match-score" id="score-L-${id}"></div></div>
        <div class="match-row"><div>${id} Right</div><div class="match-score" id="score-R-${id}"></div></div>
      </div>`).join('');
  }

  function renderBracket() {
    matchOrder.forEach(id => {
      var m = allMatches[id] || {};
      document.getElementById('score-L-' + id).textContent = m.winner === 'left' ? 'W' : '';
      document.getElementById('score-R-' + id).textContent = m.winner === 'right' ? 'W' : '';
    });
  }

  function updateJoinSection() {
    var s = document.getElementById('joinSection'); if (!myNick) { s.innerHTML = '<p class="guest-warning-text">Авторизуйтесь для участия</p>'; return; }
    s.innerHTML = '<button class="btn-join" onclick="joinTournament()">Участвовать</button>';
  }

  window.joinTournament = function() {
    var avail = qfMatchIds.filter(id => !allMatches[id] || !(allMatches[id].left && allMatches[id].left.length >= MAX_PLAYERS));
    if (avail.length === 0) return; var mid = avail[0]; var m = allMatches[mid] || {left:[], right:[]};
    var side = (!m.left || m.left.length < MAX_PLAYERS) ? 'left' : 'right'; m[side] = m[side] || []; m[side].push(myNick);
    db.ref(tPath('matches/' + mid + '/' + side)).set(m[side]);
  };

  window.openModal = function(mid) {
    currentMatchId = mid;
    updateModalContent();
    document.getElementById('teamModalOverlay').classList.add('active');
  };

  function updateModalContent() {
    if (!currentMatchId) return;
    var m = allMatches[currentMatchId] || {left:[], right:[]};
    document.getElementById('teamModalMatchup').textContent = currentMatchId.toUpperCase();
    
    // Ссылки на uCoz профили игроков для кликабельности
    document.getElementById('leftParticipantList').innerHTML = (m.left || []).map(n => `<li><a class="participant-link" href="https://moy.su{n}" target="_blank">${escapeHtml(n)}</a></li>`).join('');
    document.getElementById('rightParticipantList').innerHTML = (m.right || []).map(n => `<li><a class="participant-link" href="https://moy.su{n}" target="_blank">${escapeHtml(n)}</a></li>`).join('');
    
    var bothFull = m.left && m.left.length >= MAX_PLAYERS && m.right && m.right.length >= MAX_PLAYERS;
    if (bothFull && !m.map) { db.ref(tPath('matches/' + currentMatchId + '/map')).set(maps[Math.floor(Math.random()*maps.length)]); }
    
    document.getElementById('mapImage').style.display = m.map ? 'block' : 'none';
    if (m.map) document.getElementById('mapImage').src = m.map.url;
    document.getElementById('mapName').textContent = m.map ? m.map.name : 'Ожидание игроков';
    
    // Обновление кнопок CONNECT и START
    var btnStart = document.getElementById('btnStartServer');
    var btnConnect = document.getElementById('btnConnectServer');
    btnStart.style.display = (isAdmin && !m.serverStarted) ? 'inline-block' : 'none';
    btnConnect.style.display = m.serverStarted ? 'inline-block' : 'none';
    if (m.serverStarted && m.serverIp) { btnConnect.href = 'steam://connect/' + m.serverIp; }

    db.ref(tPath('chats/' + currentMatchId)).on('value', snap => {
      var msgs = snap.val() || {};
      document.getElementById('commonChatMessages').innerHTML = Object.keys(msgs).map(k => `<div class="chat-msg"><b>${escapeHtml(msgs[k].author)}:</b> ${escapeHtml(msgs[k].text)}</div>`).join('');
    });
  }

  window.closeModal = function() { document.getElementById('teamModalOverlay').classList.remove('active'); if (currentMatchId) db.ref(tPath('chats/' + currentMatchId)).off(); currentMatchId = null; };

  window.adminStartServer = function() {
    if (!isAdmin || !currentMatchId) return;
    var testIp = "46.174.52." + Math.floor(10 + Math.random()*200) + ":" + Math.floor(27015 + Math.random()*10);
    db.ref(tPath('matches/' + currentMatchId)).update({ serverStarted: true, serverIp: testIp });
  };

  window.sendMsg = function() {
    var txt = document.getElementById('commonChatInput').value.trim(); if (!txt || !currentMatchId) return;
    db.ref(tPath('chats/' + currentMatchId)).push({ author: myNick || 'Гость', text: txt, time: Date.now() });
    document.getElementById('commonChatInput').value = '';
  };

  function escapeHtml(t) { return String(t || '').replace(/</g, "&lt;").replace(/>/g, "&gt;"); }
  loadTournaments();
})();
</script>
</body>
</html>

