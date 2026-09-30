<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
 @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

 body { margin: 0; padding: 0; background: transparent; }

 .tournament-wrapper {
   background-image: url('https://4ak4ak.moy.su/Tchak.jpg');
   background-size: cover;
   background-position: center center;
   background-repeat: no-repeat;
   padding: 30px 0; /* ТОЛЬКО верх/низ — боковые убраны, чтобы не ломать прижатие к краям */
   border-radius: 12px;
   font-family: 'Inter', sans-serif;
   position: relative;
   overflow: hidden;
   box-sizing: border-box;
 }
 .tournament-wrapper::before {
   content: '';
   position: absolute; top: 0; left: 0; width: 100%; height: 100%;
   background: rgba(0, 0, 0, 0.5);
   z-index: 0;
 }
 .tournament-wrapper > * { position: relative; z-index: 1; }

 .tournament-title {
   text-align: center;
   color: #fff;
   font-size: 28px;
   font-weight: 800;
   text-transform: uppercase;
   letter-spacing: 2px;
   margin-bottom: 24px;
   text-shadow: 0 2px 8px rgba(0,0,0,0.8);
 }

 .bracket-row {
   display: flex;
   gap: 16px;
   justify-content: center;
   align-items: stretch;
   flex-wrap: wrap;
 }

 .bracket-col {
   flex: 1;
   min-width: 180px;
   max-width: 22%;
   display: flex;
   flex-direction: column;
 }
 .bracket-col.final-col { max-width: 260px; }

 .conf-label {
   text-align: center;
   font-size: 16px;
   font-weight: 700;
   text-transform: uppercase;
   letter-spacing: 1px;
   margin-bottom: 12px;
   padding-bottom: 8px;
   border-bottom: 1px solid rgba(255,255,255,0.2);
 }
 .conf-label.west { color: #6fb3ff; }
 .conf-label.east { color: #ff8a65; }

 .round-label {
   text-align: center;
   color: rgba(255,255,255,0.55);
   font-size: 12px;
   font-weight: 600;
   text-transform: uppercase;
   letter-spacing: 0.8px;
   margin-bottom: 8px;
 }

 .bracket-col.semifinal-col,
 .bracket-col.final-col { justify-content: center; }

 .match {
   display: flex; flex-direction: column;
   background: rgba(255,255,255,0.06);
   border-radius: 6px;
   overflow: hidden;
   margin: 0 auto 8px;
   width: 100%;
   border: 1px solid rgba(255,255,255,0.08);
   box-sizing: border-box;
   cursor: pointer;
   transition: border-color 0.2s, background 0.2s, transform 0.15s;
 }
 .match:hover {
   border-color: rgba(255,215,0,0.4);
   background: rgba(255,255,255,0.1);
   transform: translateY(-2px);
 }

 .match-row {
   display: flex; justify-content: space-between; align-items: center;
   padding: 8px 12px;
   color: #fff;
   font-size: 13px;
   font-weight: 500;
   transition: background 0.2s;
 }
 .match-row.winner {
   background: rgba(76,175,80,0.3);
   font-weight: 700;
 }
 .match-row.winner::after {
   content: '\2713';
   color: #66bb6a;
   margin-left: 6px;
 }
 .match-team { display: flex; align-items: center; gap: 6px; }
 .team-seed {
   background: rgba(255,255,255,0.12);
   border-radius: 3px;
   padding: 2px 5px;
   font-size: 11px;
   font-weight: 600;
   color: rgba(255,255,255,0.7);
   min-width: 18px;
   text-align: center;
 }
 .match-score {
   font-weight: 700;
   font-size: 14px;
   min-width: 22px;
   text-align: center;
 }
 .match-divider { height: 1px; background: rgba(255,255,255,0.08); }

 .final-box {
   background: rgba(0, 0, 0, 0.55);
   border-radius: 12px;
   padding: 20px 20px;
   border: 2px solid rgba(255,215,0,0.4);
   text-align: center;
   width: 100%;
   box-sizing: border-box;
 }
 .final-title {
   color: #ffd700;
   font-size: 18px;
   font-weight: 800;
   text-transform: uppercase;
   letter-spacing: 1.5px;
   margin-bottom: 14px;
   text-shadow: 0 2px 8px rgba(0,0,0,0.8);
 }
 .final-match {
   background: rgba(255,215,0,0.08);
   border: 1px solid rgba(255,215,0,0.2);
 }
 .final-match:hover {
   border-color: rgba(255,215,0,0.6);
 }

 .team-modal-overlay {
   display: none;
   position: fixed;
   top: 0; left: 0;
   width: 100%; height: 100%;
   background: rgba(0,0,0,0.75);
   z-index: 9999;
   justify-content: center;
   align-items: center;
 }
 .team-modal-overlay.active { display: flex; }
 .team-modal {
   background: #1a1a2e;
   border: 1px solid rgba(255,255,255,0.15);
   border-radius: 14px;
   padding: 28px 32px;
   width: 660px;
   max-width: 92vw;
   max-height: 85vh;
   overflow-y: auto;
   box-shadow: 0 12px 40px rgba(0,0,0,0.6);
   animation: modalIn 0.25s ease;
 }
 @keyframes modalIn {
   from { transform: scale(0.92); opacity: 0; }
   to { transform: scale(1); opacity: 1; }
 }
 .team-modal-header {
   display: flex;
   justify-content: space-between;
   align-items: center;
   margin-bottom: 20px;
   padding-bottom: 14px;
   border-bottom: 1px solid rgba(255,255,255,0.15);
 }
 .team-modal-matchup {
   font-size: 18px;
   font-weight: 800;
   color: #fff;
   text-transform: uppercase;
   letter-spacing: 1px;
 }
 .team-modal-close {
   background: rgba(255,255,255,0.1);
   border: none;
   color: #fff;
   font-size: 22px;
   cursor: pointer;
   width: 34px; height: 34px;
   border-radius: 50%;
   display: flex; align-items: center; justify-content: center;
   transition: background 0.2s;
   line-height: 1;
 }
 .team-modal-close:hover { background: rgba(255,80,80,0.4); }

 /* ИСПРАВЛЕННЫЙ БЛОК: команды строго от краёв, равные отступы */
 .participants-grid {
   display: flex;
   gap: 24px;
   justify-content: space-between;
   align-items: flex-start;
   flex-wrap: wrap;
   margin: 0; /* убираем лишние внешние отступы */
 }
 .participants-col {
   flex: 0 0 calc(50% - 12px); /* ровно половина минус половина gap */
   min-width: 0;
   display: flex;
   flex-direction: column;
   box-sizing: border-box;
 }
 /* ----------------------------- */

 .participants-col-title {
   font-size: 16px;
   font-weight: 700;
   text-transform: uppercase;
   letter-spacing: 1px;
   text-align: center;
   margin-bottom: 14px;
   padding-bottom: 10px;
   border-bottom: 1px solid rgba(255,255,255,0.15);
 }
 .participants-col.left .participants-col-title { color: #6fb3ff; }
 .participants-col.right .participants-col-title { color: #ff8a65; }

 .participant-list {
   list-style: none;
   padding: 0; margin: 0 0 16px 0;
   display: flex; flex-direction: column; gap: 8px;
 }
 .participant-item {
   display: flex; align-items: center; gap: 10px;
   background: rgba(255,255,255,0.06);
   border-radius: 8px;
   padding: 8px 12px;
   color: #fff;
   font-size: 13px;
   font-weight: 500;
 }
 .participant-avatar {
   width: 32px; height: 32px;
   border-radius: 50%;
   display: flex; align-items: center; justify-content: center;
   font-size: 14px; font-weight: 700; color: #fff;
   flex-shrink: 0;
 }
 .participants-col.left .participant-avatar {
   background: linear-gradient(135deg, #4a90d9, #6fb3ff);
 }
 .participants-col.right .participant-avatar {
   background: linear-gradient(135deg, #e07845, #ff8a65);
 }
 .participant-num {
   color: rgba(255,255,255,0.45);
   font-size: 11px;
   font-weight: 600;
 }

 .map-center {
   flex: 0 0 auto;
   width: 140px;
   display: flex;
   flex-direction: column;
   align-items: center;
   gap: 10px;
   padding-top: 40px;
 }
 .map-label {
   font-size: 11px;
   font-weight: 700;
   color: rgba(255,215,0,0.7);
   text-transform: uppercase;
   letter-spacing: 1px;
   text-align: center;
 }
 .map-image {
   width: 120px;
   height: 120px;
   border-radius: 10px;
   object-fit: cover;
   border: 2px solid rgba(255,215,0,0.3);
   box-shadow: 0 4px 16px rgba(0,0,0,0.4);
 }
 .map-vs {
   font-size: 22px;
   font-weight: 800;
   color: rgba(255,215,0,0.6);
   text-align: center;
 }
 .map-name {
   font-size: 12px;
   font-weight: 600;
   color: rgba(255,255,255,0.8);
   text-align: center;
   text-transform: uppercase;
   letter-spacing: 0.5px;
 }
 .map-placeholder {
   width: 120px;
   height: 120px;
   border-radius: 10px;
   border: 2px dashed rgba(255,255,255,0.2);
   display: flex;
   align-items: center;
   justify-content: center;
   text-align: center;
   font-size: 11px;
   color: rgba(255,255,255,0.4);
   line-height: 1.4;
   padding: 10px;
 }
 .map-name.empty {
   color: rgba(255,255,255,0.3);
 }

 .common-chat-container {
   width: 100%;
   margin-top: 20px;
   border-top: 1px solid rgba(255,255,255,0.1);
   padding-top: 16px;
 }
 .chat-title {
   font-size: 14px;
   font-weight: 700;
   text-transform: uppercase;
   letter-spacing: 0.8px;
   margin-bottom: 12px;
   color: #fff;
   text-align: center;
 }
 .chat-messages {
   max-height: 200px;
   overflow-y: auto;
   display: flex;
   flex-direction: column;
   gap: 8px;
   margin-bottom: 16px;
   padding: 4px 0;
 }
 .chat-messages::-webkit-scrollbar { width: 6px; }
 .chat-messages::-webkit-scrollbar-track { background: rgba(255,255,255,0.05); }
 .chat-messages::-webkit-scrollbar-thumb {
   background: rgba(255,255,255,0.2);
   border-radius: 3px;
 }
 .chat-msg {
   background: rgba(255,255,255,0.08);
   border-radius: 10px;
   padding: 10px 14px;
   font-size: 13px;
   line-height: 1.4;
   word-break: break-word;
   position: relative;
   max-width: 85%;
   align-self: flex-start;
   box-shadow: 0 1px 3px rgba(0,0,0,0.2);
 }
 .chat-msg.team-west {
   background: rgba(74, 144, 217, 0.15);
   border-left: 3px solid #6fb3ff;
   align-self: flex-start;
 }
 .chat-msg.team-east {
   background: rgba(224, 120, 69, 0.15);
   border-right: 3px solid #ff8a65;
   align-self: flex-end;
 }
 .chat-msg.team-admin {
   background: rgba(255, 215, 0, 0.15);
   border-left: 3px solid #ffd700;
   align-self: center;
   max-width: 90%;
 }
 .chat-msg-author {
   font-size: 11px;
   font-weight: 700;
   margin-bottom: 4px;
   display: block;
 }
 .chat-msg-author.west { color: #6fb3ff; }
 .chat-msg-author.east { color: #ff8a65; }
 .chat-msg-author.admin { color: #ffd700; }
 .chat-msg-text {
   font-weight: 400;
   color: rgba(255,255,255,0.9);
 }
 .chat-msg-time {
   font-size: 9px;
   color: rgba(255,255,255,0.3);
   margin-top: 4px;
 }
 .chat-msg-delete {
   position: absolute;
   top: 6px; right: 6px;
   background: rgba(255,80,80,0.2);
   border: none;
   color: #ff6b6b;
   font-size: 14px;
   cursor: pointer;
   width: 24px; height: 24px;
   border-radius: 50%;
   display: none;
   align-items: center;
   justify-content: center;
   line-height: 1;
 }
 .chat-msg:hover .chat-msg-delete { display: flex; }
 .chat-msg-delete:hover { background: rgba(255,80,80,0.4); }

 .chat-input-row {
   display: flex;
   gap: 10px;
 }
 .chat-input {
   flex: 1;
   background: rgba(255,255,255,0.08);
   border: 1px solid rgba(255,255,255,0.1);
   border-radius: 8px;
   padding: 12px;
   color: #fff;
   font-size: 14px;
   font-family: 'Inter', sans-serif;
   outline: none;
   transition: border-color 0.2s;
 }
 .chat-input:focus { border-color: rgba(255,215,0,0.4); }
 .chat-input::placeholder { color: rgba(255,255,255,0.3); }
 .chat-send {
   background: rgba(255,215,0,0.15);
   border: 1px solid rgba(255,215,0,0.3);
   border-radius: 8px;
   padding: 0 20px;
   color: #ffd700;
   font-size: 14px;
   font-weight: 600;
   cursor: pointer;
   transition: background 0.2s;
   white-space: nowrap;
 }
 .chat-send:hover { background: rgba(255,215,0,0.25); }

 .admin-badge {
   display: inline-block;
   background: rgba(255,215,0,0.2);
   color: #ffd700;
   font-size: 9px;
   font-weight: 700;
   padding: 1px 5px;
   border-radius: 3px;
   margin-left: 4px;
   vertical-align: middle;
 }

 .chat-loading {
   text-align: center;
   font-size: 12px;
   color: rgba(255,255,255,0.3);
   padding: 16px;
 }

 @media (max-width: 900px) {
   .bracket-row { flex-direction: column; align-items: center; }
   .bracket-col { max-width: 100%; width: 100%; }
   .tournament-title { font-size: 22px; }
   .match { max-width: 100%; }
   .participants-grid { flex-direction: column; align-items: center; gap: 20px; }
   .map-center { padding-top: 0; }
   .team-modal { width: 100%; padding: 20px; }
   .chat-msg { max-width: 95%; }
 }
</style>
</head>
<body>

<div class="tournament-wrapper">
  <div class="tournament-title">Плей-офф турнира</div>
  <div class="bracket-row">

    <div class="bracket-col">
      <div class="conf-label west">Запад</div>
      <div class="round-label">Четвертьфиналы</div>
      <div class="match" data-team-left="Команда A" data-team-right="Команда B" data-participants-left="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5" data-participants-right="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5">
        <div class="match-row winner"><div class="match-team"><span class="team-seed">1</span> Команда A</div><div class="match-score">3</div></div>
        <div class="match-divider"></div>
        <div class="match-row"><div class="match-team"><span class="team-seed">4</span> Команда B</div><div class="match-score">1</div></div>
      </div>
      <div class="match" data-team-left="Команда C" data-team-right="Команда D" data-participants-left="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5" data-participants-right="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5">
        <div class="match-row winner"><div class="match-team"><span class="team-seed">2</span> Команда C</div><div class="match-score">2</div></div>
        <div class="match-divider"></div>
        <div class="match-row"><div class="match-team"><span class="team-seed">3</span> Команда D</div><div class="match-score">0</div></div>
      </div>
    </div>

    <div class="bracket-col semifinal-col">
      <div class="round-label">Полуфинал</div>
      <div class="match" data-team-left="Команда A" data-team-right="Команда C" data-participants-left="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5" data-participants-right="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5">
        <div class="match-row winner"><div class="match-team"><span class="team-seed">1</span> Команда A</div><div class="match-score">2</div></div>
        <div class="match-divider"></div>
        <div class="match-row"><div class="match-team"><span class="team-seed">2</span> Команда C</div><div class="match-score">1</div></div>
      </div>
    </div>

    <div class="bracket-col final-col">
      <div class="final-box">
        <div class="final-title">Финал</div>
        <div class="match final-match" data-team-left="Команда A" data-team-right="Команда E" data-participants-left="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5" data-participants-right="Участник 1,Участник 2,Участник 3,Участник 4,Участник 5">
          <div class="match-row winner"><div class="match-team"><span class="team-seed">З1</span> Команда A</div><div class="match-score">3</div></div>
          <div class="match-divider"></div>
          <div class="match-row"><div class="match-team"><span class="team-seed">В1</span> Команда E</div><div class="match-score">2</div></div>
        </div>
      </div>
    </div>
  </div>
</div>

</body>
</html>
