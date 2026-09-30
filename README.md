<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Плей-офф турнира</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

body { margin: 0; padding: 0; background: transparent; font-family: 'Inter', sans-serif; }

.tournament-wrapper {
  background-image: url('https://4ak4ak.moy.su/Tchak.jpg');
  background-size: cover; background-position: center center; background-repeat: no-repeat;
  padding: 30px 0; border-radius: 12px; position: relative; overflow: hidden;
}
.tournament-wrapper::before {
  content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.5); z-index: 0;
}
.tournament-wrapper > * { position: relative; z-index: 1; }

.tournament-title {
  text-align: center; color: #fff; font-size: 28px; font-weight: 800;
  text-transform: uppercase; letter-spacing: 2px; margin-bottom: 24px;
  text-shadow: 0 2px 8px rgba(0,0,0,0.8);
}

.bracket-row { display: flex; gap: 16px; justify-content: center; align-items: stretch; flex-wrap: wrap; }
.bracket-col { flex: 1; min-width: 180px; max-width: 22%; display: flex; flex-direction: column; }
.bracket-col.final-col { max-width: 260px; }
.bracket-col.semifinal-col, .bracket-col.final-col { justify-content: center; }

.conf-label {
  text-align: center; font-size: 16px; font-weight: 700; text-transform: uppercase;
  letter-spacing: 1px; margin-bottom: 12px; padding-bottom: 8px;
  border-bottom: 1px solid rgba(255,255,255,0.2);
}
.conf-label.west { color: #6fb3ff; }
.conf-label.east { color: #ff8a65; }

.round-label {
  text-align: center; color: rgba(255,255,255,0.55); font-size: 12px;
  font-weight: 600; text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 8px;
}

.match {
  display: flex; flex-direction: column; background: rgba(255,255,255,0.06);
  border-radius: 6px; overflow: hidden; margin: 0 auto 8px; width: 100%;
  border: 1px solid rgba(255,255,255,0.08); box-sizing: border-box;
  cursor: pointer; transition: border-color 0.2s, background 0.2s, transform 0.15s;
}
.match:hover { border-color: rgba(255,215,0,0.4); background: rgba(255,255,255,0.1); transform: translateY(-2px); }
.match.is-mine { border-color: rgba(74,158,255,0.6); box-shadow: 0 0 12px rgba(74,158,255,0.4); }

.match-row {
  display: flex; justify-content: space-between; align-items: center;
  padding: 8px 12px; color: #fff; font-size: 13px; font-weight: 500; transition: background 0.2s;
}
.match-row.winner { background: rgba(76,175,80,0.3); font-weight: 700; }
.match-row.winner::after { content: '\2713'; color: #66bb6a; margin-left: 6px; }
.match-row.is-mine-row { background: rgba(74,158,255,0.2); }
.match-team { display: flex; align-items: center; gap: 6px; }
.team-seed {
  background: rgba(255,255,255,0.12); border-radius: 3px; padding: 2px 5px;
  font-size: 11px; font-weight: 600; color: rgba(255,255,255,0.7); min-width: 18px; text-align: center;
}

/* Score: two fields + colon */
.score-container {
  display: inline-flex; align-items: center; justify-content: center; gap: 4px; min-width: 72px;
}
.score-input {
  width: 36px; text-align: center; font-size: 14px; font-weight: 700; color: #fff;
  background: rgba(0,0,0,0.25); border: 1px solid rgba(255,255,255,0.15);
  border-radius: 4px; padding: 4px 2px; box-sizing: border-box; font-family: 'Inter', sans-serif;
}
.score-colon { font-weight: 700; color: rgba(255,255,255,0.9); margin: 0 2px; font-size: 14px; }

.match-score { font-weight: 700; font-size: 14px; min-width: 22px; text-align: center; }
.match-divider { height: 1px; background: rgba(255,255,255,0.08); }

.final-box {
  background: rgba(0,0,0,0.55); border-radius: 12px; padding: 20px;
  border: 2px solid rgba(255,215,0,0.4); text-align: center; width: 100%; box-sizing: border-box;
}
.final-title {
  color: #ffd700; font-size: 18px; font-weight: 800; text-transform: uppercase;
  letter-spacing: 1.5px; margin-bottom: 14px; text-shadow: 0 2px 8px rgba(0,0,0,0.8);
}
.final-match { background: rgba(255,215,0,0.08); border: 1px solid rgba(255,215,0,0.2); }
.final-match:hover { border-color: rgba(255,215,0,0.6); }

.join-section { text-align: center; margin: 24px 0 20px; }
.btn-join {
  display: inline-block; padding: 14px 40px; font-size: 17px; font-weight: 700;
  color: #fff; background: linear-gradient(135deg, #0a2a6b, #1a4a8b);
  border: 2px solid rgba(255,255,255,0.3); border-radius: 50px; cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s, background 0.3s;
  box-shadow: 0 4px 14px rgba(10,42,107,0.4); font-family: 'Inter', sans-serif;
}
.btn-join:hover { transform: translateY(-2px); box-shadow: 0 8px 22px rgba(10,42,107,0.6); background: linear-gradient(135deg, #1a4a8b, #2a6abb); }
.joined-msg { text-align: center; color: #4a9eff; font-weight: 700; font-size: 17px; margin-bottom: 20px; text-shadow: 0 0 10px rgba(74,158,255,0.5); }
.guest-warning { text-align: center; margin-bottom: 20px; }
.guest-warning-text {
  font-weight: 800; background: linear-gradient(180deg, transparent 45%, #0a2a6bcc 45%);
  padding: 3px 10px; border-radius: 5px; color: #fff;
  text-shadow: 0 0 5px #0a2a6b, 0 0 10px #0a2a6b; display: inline-block; font-size: 15px;
}
.admin-login-row { text-align: center; margin: 20px 0; display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; }
.admin-login-row input {
  padding: 12px 18px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px;
  background: rgba(0,0,0,0.4); color: #fff; font-size: 14px; width: 200px; font-family: 'Inter', sans-serif;
}
.admin-login-row input::placeholder { color: rgba(255,255,255,0.4); }
.admin-panel {
  max-width: 850px; margin: 0 auto 20px; padding: 22px;
  background: rgba(10,42,107,0.3); backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(74,158,255,0.35); border-radius: 14px; box-shadow: 0 4px 20px rgba(10,42,107,0.3);
}
.admin-panel h3 { font-size: 18px; margin-bottom: 12px; color: #4a9eff; text-shadow: 0 0 10px rgba(74,158,255,0.5); font-weight: 700; }
.admin-hint { font-size: 13px; color: rgba(255,255,255,0.5); }
.admin-active-badge {
  display: inline-block; background: linear-gradient(135deg, #27ae60, #2ecc71); color: #fff;
  font-size: 12px; padding: 4px 12px; border-radius: 20px; font-weight: 700;
  margin-left: 10px; box-shadow: 0 2px 8px rgba(39,174,96,0.4);
}

.team-modal-overlay {
  display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.75); z-index: 9999; justify-content: center; align-items: center;
}
.team-modal-overlay.active { display: flex; }
.team-modal {
  background: #1a1a2e; border: 1px solid rgba(255,255,255,0.15); border-radius: 14px;
  padding: 28px 32px; width: 800px; max-width: 92vw; max-height: 88vh; overflow-y: auto;
  box-shadow: 0 12px 40px rgba(0,0,0,0.6); animation: modalIn 0.25s ease;
}
@keyframes modalIn { from { transform: scale(0.92); opacity: 0; } to { transform: scale(1); opacity: 1; } }
.team-modal-header {
  display: flex; justify-content: space-between; align-items: center;
  margin-bottom: 20px; padding-bottom: 14px; border-bottom: 1px solid rgba(255,255,255,0.15);
}
.team-modal-matchup { font-size: 18px; font-weight: 800; color: #fff; text-transform: uppercase; letter-spacing: 1px; }
.team-modal-close {
  background: rgba(255,255,255,0.1); border: none; color: #fff; font-size: 22px; cursor: pointer;
  width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
  transition: background 0.2s; line-height: 1;
}
.team-modal-close:hover { background: rgba(255,80,80,0.4); }

.participants-grid { display: flex; gap: 16px; justify-content: space-between; align-items: flex-start; }
.participants-col { flex: 1; min-width: 0; display: flex; flex-direction: column; }
.participants-col.is-mine-col {
  border: 2px solid rgba(74,158,255,0.5); border-radius: 10px; padding: 12px;
  box-shadow: 0 0 14px rgba(74,158,255,0.3); animation: pulse-mine 2.5s ease-in-out infinite;
}
@keyframes pulse-mine {
  0%,100% { box-shadow: 0 0 10px rgba(74,158,255,0.3); }
  50% { box-shadow: 0 0 22px rgba(74,158,255,0.6); }
}
.participants-col-title {
  font-size: 16px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px;
  text-align: center; margin-bottom: 14px; padding-bottom: 10px;
  border-bottom: 1px solid rgba(255,255,255,0.15);
}
.participants-col.left .participants-col-title { color: #6fb3ff; }
.participants-col.right .participants-col-title { color: #ff8a65; }
.participant-list { list-style: none; padding: 0; margin: 0 0 10px; display: flex; flex-direction: column; gap: 8px; }
.participant-item {
  display: flex; align-items: center; gap: 10px; background: rgba(255,255,255,0.06);
  border-radius: 8px; padding: 8px 12px; color: #fff; font-size: 13px; font-weight: 500;
  position: relative; flex-wrap: wrap;
}
.participant-item.is-me { background: rgba(74,158,255,0.2); border: 1px solid rgba(74,158,255,0.5); }
.participant-item.is-me .participant-avatar { box-shadow: 0 0 8px rgba(74,158,255,0.7); }
.participant-avatar {
  width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center;
  justify-content: center; font-size: 14px; font-weight: 700; color: #fff; flex-shrink: 0;
}
.participants-col.left .participant-avatar { background: linear-gradient(135deg, #4a90d9, #357abd); }
.participants-col.right .participant-avatar { background: linear-gradient(135deg, #e67e22, #d35400); }
.participant-name { flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

/* Score row per participant */
.participant-score-row {
  display: flex; align-items: center; gap: 6px; margin-top: 4px; width: 100%;
}
.participant-score-label { font-size: 11px; color: rgba(255,255,255,0.5); font-weight: 600; text-transform: uppercase; }
.participant-score-container {
  display: inline-flex; align-items: center; gap: 3px;
}
.participant-score-input {
  width: 34px; text-align: center; font-size: 13px; font-weight: 700; color: #fff;
  background: rgba(0,0,0,0.3); border: 1px solid rgba(255,255,255,0.15);
  border-radius: 4px; padding: 3px 2px; box-sizing: border-box; font-family: 'Inter', sans-serif;
}
.participant-score-colon { font-weight: 700; color: rgba(255,255,255,0.9); font-size: 13px; }
.btn-confirm-score {
  background: rgba(74,158,255,0.3); border: 1px solid rgba(74,158,255,0.5); color: #fff;
  font-size: 11px; font-weight: 700; padding: 3px 10px; border-radius: 4px; cursor: pointer;
  font-family: 'Inter', sans-serif; transition: background 0.2s;
}
.btn-confirm-score:hover { background: rgba(74,158,255,0.5); }

/* Winner section */
.winner-section {
  margin-top: 20px; padding-top: 16px; border-top: 1px solid rgba(255,255,255,0.15);
  text-align: center; display: none;
}
.winner-section.show { display: block; }
.winner-label {
  font-size: 14px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px;
  margin-bottom: 12px; color: #ffd700;
}
.winner-buttons { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
.btn-winner {
  padding: 10px 24px; font-size: 14px; font-weight: 700; color: #fff;
  background: rgba(76,175,80,0.3); border: 1px solid rgba(76,175,80,0.5);
  border-radius: 8px; cursor: pointer; font-family: 'Inter', sans-serif;
  transition: background 0.2s, transform 0.15s;
}
.btn-winner:hover { background: rgba(76,175,80,0.5); transform: translateY(-1px); }

/* Winner logo (logo1.jpg) - only here */
.winner-logo-section {
  margin-top: 14px; text-align: center; display: none;
}
.winner-logo-section.show { display: block; }
.winner-logo {
  width: 64px; height: 64px; border-radius: 50%; object-fit: cover;
  border: 2px solid rgba(255,215,0,0.5); display: inline-block;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.winner-logo.admin-clickable {
  cursor: pointer; border-color: rgba(255,100,100,0.6);
}
.winner-logo.admin-clickable:hover {
  border-color: rgba(255,80,80,0.9); box-shadow: 0 0 12px rgba(255,80,80,0.5);
}
.winner-logo-cancel-hint {
  font-size: 11px; color: rgba(255,100,100,0.8); margin-top: 6px; font-weight: 600;
}

.winner-name {
  font-size: 20px; font-weight: 800; color: #ffd700; text-transform: uppercase;
  letter-spacing: 1px; margin-top: 10px;
}

/* Bracket lines */
.bracket-line {
  width: 16px; display: flex; align-items: center; justify-content: center;
}
.bracket-line svg { width: 100%; height: 100%; }

/* Responsive */
@media (max-width: 768px) {
  .bracket-row { flex-direction: column; align-items: center; }
  .bracket-col { max-width: 90%; }
  .participants-grid { flex-direction: column; }
  .team-modal { width: 95vw; padding: 20px; }
}
</style>
</head>
<body>

<div class="tournament-wrapper" id="tournamentWrapper">
  <div class="tournament-title">Плей-офф</div>

  <!-- Join / Admin -->
  <div id="joinSection" class="join-section">
    <button class="btn-join" onclick="joinTournament()">Присоединиться</button>
  </div>
  <div id="joinedMsg" class="joined-msg" style="display:none;">Вы участвуете!</div>
  <div id="guestWarning" class="guest-warning" style="display:none;">
    <span class="guest-warning-text">Войдите как админ для управления</span>
  </div>
  <div class="admin-login-row">
    <input type="password" id="adminPass" placeholder="Админ-пароль" onkeydown="if(event.key==='Enter')loginAdmin()">
    <button class="btn-join" style="padding:12px 28px;font-size:14px;" onclick="loginAdmin()">Войти</button>
  </div>
  <div id="adminPanel" class="admin-panel" style="display:none;">
    <h3>Админ-панель <span class="admin-active-badge">Активна</span></h3>
    <p class="admin-hint">Нажмите на матч, чтобы открыть участников. Введите счёт и выберите победителя.</p>
  </div>

  <!-- Bracket -->
  <div class="bracket-row" id="bracketRow">
    <!-- Quarterfinals West -->
    <div class="bracket-col">
      <div class="conf-label west">Запад</div>
      <div class="round-label">1/4 финала</div>
      <div class="match" data-match="qf1" data-round="qf" data-conf="west" data-slot="1" onclick="openMatch('qf1')">
        <div class="match-row" id="qf1-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="qf1-bot"></div>
      </div>
      <div class="match" data-match="qf2" data-round="qf" data-conf="west" data-slot="2" onclick="openMatch('qf2')">
        <div class="match-row" id="qf2-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="qf2-bot"></div>
      </div>
    </div>

    <!-- Quarterfinals East -->
    <div class="bracket-col">
      <div class="conf-label east">Восток</div>
      <div class="round-label">1/4 финала</div>
      <div class="match" data-match="qf3" data-round="qf" data-conf="east" data-slot="1" onclick="openMatch('qf3')">
        <div class="match-row" id="qf3-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="qf3-bot"></div>
      </div>
      <div class="match" data-match="qf4" data-round="qf" data-conf="east" data-slot="2" onclick="openMatch('qf4')">
        <div class="match-row" id="qf4-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="qf4-bot"></div>
      </div>
    </div>

    <!-- Semifinals -->
    <div class="bracket-col semifinal-col">
      <div class="round-label">Полуфинал</div>
      <div class="match" data-match="sf1" data-round="sf" data-slot="1" onclick="openMatch('sf1')">
        <div class="match-row" id="sf1-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="sf1-bot"></div>
      </div>
      <div class="match" data-match="sf2" data-round="sf" data-slot="2" onclick="openMatch('sf2')">
        <div class="match-row" id="sf2-top"></div>
        <div class="match-divider"></div>
        <div class="match-row" id="sf2-bot"></div>
      </div>
    </div>

    <!-- Final -->
    <div class="bracket-col final-col">
      <div class="final-box">
        <div class="final-title">Финал</div>
        <div class="match final-match" data-match="final" data-round="final" onclick="openMatch('final')">
          <div class="match-row" id="final-top"></div>
          <div class="match-divider"></div>
          <div class="match-row" id="final-bot"></div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Modal -->
<div class="team-modal-overlay" id="modalOverlay" onclick="if(event.target===this)closeModal()">
  <div class="team-modal">
    <div class="team-modal-header">
      <div class="team-modal-matchup" id="modalMatchup">Матч</div>
      <button class="team-modal-close" onclick="closeModal()">&times;</button>
    </div>
    <div class="participants-grid" id="modalGrid"></div>
    <div class="winner-section" id="winnerSection">
      <div class="winner-label">Выберите победителя</div>
      <div class="winner-buttons" id="winnerButtons"></div>
    </div>
    <div class="winner-logo-section" id="winnerLogoSection">
      <img src="https://4ak4ak.moy.su/logo1.jpg" class="winner-logo" id="winnerLogoImg" alt="Победитель" onclick="cancelWinner(event)">
      <div class="winner-logo-cancel-hint" id="cancelHint" style="display:none;">Нажмите, чтобы отменить победителя</div>
      <div class="winner-name" id="winnerName"></div>
    </div>
  </div>
</div>

<script>
// --- Data ---
const TEAMS = {
  qf1: { left: { name: 'Команда A', seed: 1, players: ['Игрок 1','Игрок 2','Игрок 3','Игрок 4','Игрок 5'] },
          right: { name: 'Команда B', seed: 8, players: ['Игрок 6','Игрок 7','Игрок 8','Игрок 9','Игрок 10'] } },
  qf2: { left: { name: 'Команда C', seed: 4, players: ['Игрок 11','Игрок 12','Игрок 13','Игрок 14','Игрок 15'] },
          right: { name: 'Команда D', seed: 5, players: ['Игрок 16','Игрок 17','Игрок 18','Игрок 19','Игрок 20'] } },
  qf3: { left: { name: 'Команда E', seed: 2, players: ['Игрок 21','Игрок 22','Игрок 23','Игрок 24','Игрок 25'] },
          right: { name: 'Команда F', seed: 7, players: ['Игрок 26','Игрок 27','Игрок 28','Игрок 29','Игрок 30'] } },
  qf4: { left: { name: 'Команда G', seed: 3, players: ['Игрок 31','Игрок 32','Игрок 33','Игрок 34','Игрок 35'] },
          right: { name: 'Команда H', seed: 6, players: ['Игрок 36','Игрок 37','Игрок 38','Игрок 39','Игрок 40'] } },
  sf1: { left: { name: '', seed: null, players: [] }, right: { name: '', seed: null, players: [] } },
  sf2: { left: { name: '', seed: null, players: [] }, right: { name: '', seed: null, players: [] } },
  final: { left: { name: '', seed: null, players: [] }, right: { name: '', seed: null, players: [] } }
};

const RESULTS = {};
let currentMatch = null;
let isAdmin = false;
const ADMIN_PASS = 'admin123';
let myTeam = null;

function joinTournament() {
  myTeam = 'qf1-left';
  document.getElementById('joinSection').style.display = 'none';
  document.getElementById('joinedMsg').style.display = 'block';
  renderBracket();
}

function loginAdmin() {
  const val = document.getElementById('adminPass').value;
  if (val === ADMIN_PASS) {
    isAdmin = true;
    document.getElementById('adminPanel').style.display = 'block';
    document.getElementById('guestWarning').style.display = 'none';
    renderBracket();
    if (currentMatch) openMatch(currentMatch);
  } else {
    alert('Неверный пароль');
  }
}

function getWinner(matchId) { return RESULTS[matchId] ? RESULTS[matchId].winner : null; }

function getScoreDisplay(matchId) {
  if (!RESULTS[matchId] || !RESULTS[matchId].scores) return '';
  const s = RESULTS[matchId].scores;
  let leftSum = 0, rightSum = 0;
  for (const k in s) {
    if (k.startsWith('left')) leftSum += parseInt(s[k].left || 0);
    if (k.startsWith('right')) rightSum += parseInt(s[k].right || 0);
  }
  return leftSum + ':' + rightSum;
}

function renderBracket() {
  const matchIds = ['qf1','qf2','qf3','qf4','sf1','sf2','final'];
  matchIds.forEach(id => {
    const m = TEAMS[id];
    const topId = id + '-top', botId = id + '-bot';
    const topEl = document.getElementById(topId);
    const botEl = document.getElementById(botId);
    if (!topEl || !botEl) return;

    const winner = getWinner(id);
    const score = getScoreDisplay(id);

    const leftName = m.left.name || 'TBD';
    const rightName = m.right.name || 'TBD';
    const leftSeed = m.left.seed !== null ? m.left.seed : '';
    const rightSeed = m.right.seed !== null ? m.right.seed : '';

    topEl.innerHTML = '<span class="match-team"><span class="team-seed">'+leftSeed+'</span>'+leftName+'</span>' +
      (score ? '<span class="score-container"><input class="score-input" value="'+score.split(':')[0]+'" readonly><span class="score-colon">:</span><input class="score-input" value="'+score.split(':')[1]+'" readonly></span>' : '');
    botEl.innerHTML = '<span class="match-team"><span class="team-seed">'+rightSeed+'</span>'+rightName+'</span>' +
      (score ? '<span class="score-container"><input class="score-input" value="'+score.split(':')[0]+'" readonly><span class="score-colon">:</span><input class="score-input" value="'+score.split(':')[1]+'" readonly></span>' : '');

    topEl.classList.remove('winner','is-mine-row');
    botEl.classList.remove('winner','is-mine-row');

    if (winner === 'left') topEl.classList.add('winner');
    if (winner === 'right') botEl.classList.add('winner');

    if (myTeam && myTeam.startsWith(id)) {
      if (myTeam.endsWith('left')) topEl.classList.add('is-mine-row');
      if (myTeam.endsWith('right')) botEl.classList.add('is-mine-row');
    }

    const matchEl = document.querySelector('[data-match="'+id+'"]');
    if (matchEl) {
      matchEl.classList.remove('is-mine');
      if (myTeam && myTeam.startsWith(id)) matchEl.classList.add('is-mine');
    }
  });
}

function openMatch(matchId) {
  currentMatch = matchId;
  const m = TEAMS[matchId];
  document.getElementById('modalMatchup').textContent = m.left.name + ' vs ' + m.right.name;
  const grid = document.getElementById('modalGrid');
  grid.innerHTML = '';

  ['left','right'].forEach(side => {
    const col = document.createElement('div');
    col.className = 'participants-col ' + side;
    if (myTeam && myTeam === matchId+'-'+side) col.classList.add('is-mine-col');

    const title = document.createElement('div');
    title.className = 'participants-col-title';
    title.textContent = m[side].name || 'Ожидание';
    col.appendChild(title);

    const list = document.createElement('ul');
    list.className = 'participant-list';

    if (m[side].players.length === 0) {
      const li = document.createElement('li');
      li.className = 'participant-item';
      li.textContent = 'Участники не определены';
      list.appendChild(li);
    } else {
      m[side].players.forEach((p, idx) => {
        const li = document.createElement('li');
        li.className = 'participant-item';
        if (myTeam && myTeam === matchId+'-'+side) li.classList.add('is-me');

        const avatar = document.createElement('div');
        avatar.className = 'participant-avatar';
        avatar.textContent = p.charAt(0);
        li.appendChild(avatar);

        const name = document.createElement('span');
        name.className = 'participant-name';
        name.textContent = p;
        li.appendChild(name);

        // Score row: two inputs + colon + OK button
        if (isAdmin) {
          const scoreRow = document.createElement('div');
          scoreRow.className = 'participant-score-row';

          const scoreLabel = document.createElement('span');
          scoreLabel.className = 'participant-score-label';
          scoreLabel.textContent = 'Счёт:';
          scoreRow.appendChild(scoreLabel);

          const scoreContainer = document.createElement('div');
          scoreContainer.className = 'participant-score-container';

          const inputLeft = document.createElement('input');
          inputLeft.type = 'text';
          inputLeft.className = 'participant-score-input';
          inputLeft.maxLength = 4;
          inputLeft.placeholder = '0';
          inputLeft.dataset.side = side;
          inputLeft.dataset.player = idx;
          inputLeft.dataset.half = 'left';

          // Pre-fill
          const saved = RESULTS[matchId] && RESULTS[matchId].scores && RESULTS[matchId].scores[side+'-'+idx];
          if (saved) {
            inputLeft.value = saved.left || '';
          }

          const colon = document.createElement('span');
          colon.className = 'participant-score-colon';
          colon.textContent = ':';

          const inputRight = document.createElement('input');
          inputRight.type = 'text';
          inputRight.className = 'participant-score-input';
          inputRight.maxLength = 4;
          inputRight.placeholder = '0';
          inputRight.dataset.side = side;
          inputRight.dataset.player = idx;
          inputRight.dataset.half = 'right';

          if (saved) {
            inputRight.value = saved.right || '';
          }

          scoreContainer.appendChild(inputLeft);
          scoreContainer.appendChild(colon);
          scoreContainer.appendChild(inputRight);
          scoreRow.appendChild(scoreContainer);

          const btn = document.createElement('button');
          btn.className = 'btn-confirm-score';
          btn.textContent = 'OK';
          btn.onclick = function() { saveScore(matchId, side, idx, inputLeft.value, inputRight.value); };
          scoreRow.appendChild(btn);

          li.appendChild(scoreRow);
        }

        list.appendChild(li);
      });
    }

    col.appendChild(list);
    grid.appendChild(col);
  });

  // Winner section
  const winnerSection = document.getElementById('winnerSection');
  const winnerButtons = document.getElementById('winnerButtons');
  const winnerLogoSection = document.getElementById('winnerLogoSection');
  const winnerName = document.getElementById('winnerName');

  const winner = getWinner(matchId);

  if (winner) {
    winnerSection.classList.remove('show');
    winnerLogoSection.classList.add('show');
    winnerName.textContent = m[winner].name;
    const logoImg = document.getElementById('winnerLogoImg');
    if (isAdmin) {
      logoImg.classList.add('admin-clickable');
      document.getElementById('cancelHint').style.display = 'block';
    } else {
      logoImg.classList.remove('admin-clickable');
      document.getElementById('cancelHint').style.display = 'none';
    }
  } else {
    winnerSection.classList.add('show');
    winnerLogoSection.classList.remove('show');
    winnerButtons.innerHTML = '';

    if (isAdmin && m.left.name && m.right.name) {
      const btnL = document.createElement('button');
      btnL.className = 'btn-winner';
      btnL.textContent = m.left.name;
      btnL.onclick = function() { setWinner(matchId, 'left'); };
      winnerButtons.appendChild(btnL);

      const btnR = document.createElement('button');
      btnR.className = 'btn-winner';
      btnR.textContent = m.right.name;
      btnR.onclick = function() { setWinner(matchId, 'right'); };
      winnerButtons.appendChild(btnR);
    } else {
      winnerButtons.innerHTML = '<p style="color:rgba(255,255,255,0.5);font-size:13px;">' + (isAdmin ? 'Обе команды должны быть определены' : 'Только админ может выбрать победителя') + '</p>';
    }
  }

  document.getElementById('modalOverlay').classList.add('active');
}

function closeModal() {
  document.getElementById('modalOverlay').classList.remove('active');
  currentMatch = null;
}

function saveScore(matchId, side, playerIdx, leftVal, rightVal) {
  if (!RESULTS[matchId]) RESULTS[matchId] = { scores: {}, winner: null };
  if (!RESULTS[matchId].scores) RESULTS[matchId].scores = {};
  RESULTS[matchId].scores[side+'-'+playerIdx] = { left: leftVal, right: rightVal };
  renderBracket();
}

function setWinner(matchId, side) {
  if (!RESULTS[matchId]) RESULTS[matchId] = { scores: {}, winner: null };
  RESULTS[matchId].winner = side;

  const m = TEAMS[matchId];
  const winners = m[side].players.slice();

  // Distribute to next round
  distributeWinners(matchId, side, winners);

  renderBracket();
  openMatch(matchId);
}

function distributeWinners(matchId, side, winners) {
  // Shuffle winners
  const shuffled = winners.slice();
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    const tmp = shuffled[i]; shuffled[i] = shuffled[j]; shuffled[j] = tmp;
  }

  if (matchId.startsWith('qf')) {
    const num = parseInt(matchId.charAt(2));
    if (num === 1 || num === 2) {
      // -> sf1
      fillNextTeam('sf1', side === 'left' ? 'left' : 'right', TEAMS[matchId][side], shuffled);
    } else {
      // -> sf2
      fillNextTeam('sf2', side === 'left' ? 'left' : 'right', TEAMS[matchId][side], shuffled);
    }
  } else if (matchId === 'sf1') {
    fillNextTeam('final', 'left', TEAMS[matchId][side], shuffled);
  } else if (matchId === 'sf2') {
    fillNextTeam('final', 'right', TEAMS[matchId][side], shuffled);
  }
}

function fillNextTeam(nextMatch, slot, teamData, shuffledPlayers) {
  // Determine which slot based on original match
  // For qf1->sf1 left, qf2->sf1 right, qf3->sf2 left, qf4->sf2 right
  // sf1->final left, sf2->final right
  if (nextMatch === 'sf1') {
    if (currentMatch === 'qf1') TEAMS.sf1.left = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
    if (currentMatch === 'qf2') TEAMS.sf1.right = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
  } else if (nextMatch === 'sf2') {
    if (currentMatch === 'qf3') TEAMS.sf2.left = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
    if (currentMatch === 'qf4') TEAMS.sf2.right = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
  } else if (nextMatch === 'final') {
    if (currentMatch === 'sf1') TEAMS.final.left = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
    if (currentMatch === 'sf2') TEAMS.final.right = { name: teamData.name, seed: teamData.seed, players: shuffledPlayers };
  }
}

function cancelWinner(e) {
  if (e) e.stopPropagation();
  if (!isAdmin) return;
  if (!currentMatch) return;

  if (!confirm('Отменить победителя?')) return;

  const matchId = currentMatch;
  const winner = RESULTS[matchId] ? RESULTS[matchId].winner : null;
  if (!winner) return;

  RESULTS[matchId].winner = null;

  // Clear next round teams that came from this match
  if (matchId.startsWith('qf')) {
    const num = parseInt(matchId.charAt(2));
    if (num === 1) { TEAMS.sf1.left = { name:'', seed:null, players:[] }; }
    if (num === 2) { TEAMS.sf1.right = { name:'', seed:null, players:[] }; }
    if (num === 3) { TEAMS.sf2.left = { name:'', seed:null, players:[] }; }
    if (num === 4) { TEAMS.sf2.right = { name:'', seed:null, players:[] }; }
  } else if (matchId === 'sf1') {
    TEAMS.final.left = { name:'', seed:null, players:[] };
  } else if (matchId === 'sf2') {
    TEAMS.final.right = { name:'', seed:null, players:[] };
  }

  // Clear scores for this match
  if (RESULTS[matchId]) RESULTS[matchId].scores = {};

  renderBracket();
  openMatch(matchId);
}

// Init
renderBracket();
</script>
</body>
</html>
