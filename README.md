<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Турнир</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

body { margin: 0; padding: 0; background: transparent; font-family: 'Inter', sans-serif; overflow-y: auto; scrollbar-width: none; -ms-overflow-style: none; }
body::-webkit-scrollbar { display: none; }

.tournament-selector-row {
  max-width: 850px; margin: 0 auto 16px; padding: 0 20px; display: flex; align-items: center; gap: 10px; flex-wrap: wrap;
}
.tournament-selector-row label {
  color: #fff; font-size: 14px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px;
}
.tournament-selector-row select {
  flex: 1; min-width: 200px; padding: 10px 14px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px;
  background: rgba(10,42,107,0.55); color: #fff; font-size: 14px; font-family: 'Inter', sans-serif; cursor: pointer;
}
.tournament-selector-row select option { background: #1a1a2e; color: #fff; }
.tournament-create-row {
  max-width: 850px; margin: 0 auto 16px; padding: 0 20px; display: none; align-items: center; gap: 10px; flex-wrap: wrap;
}
.tournament-create-row.visible { display: flex; }
.tournament-create-row input {
  flex: 1; min-width: 200px; padding: 10px 14px; border: 1px solid rgba(255,255,255,0.2); border-radius: 8px;
  background: rgba(10,42,107,0.55); color: #fff; font-size: 14px; font-family: 'Inter', sans-serif;
}
.tournament-create-row input::placeholder { color: rgba(255,255,255,0.4); }
.btn-create {
  padding: 10px 24px; font-size: 14px; font-weight: 700; color: #fff;
  background: linear-gradient(135deg, #27ae60, #2ecc71); border: 2px solid rgba(255,255,255,0.3);
  border-radius: 50px; cursor: pointer; font-family: 'Inter', sans-serif; transition: transform 0.2s, box-shadow 0.2s;
}
.btn-create:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(39,174,96,0.5); }
.btn-delete-tournament {
  padding: 10px 24px; font-size: 14px; font-weight: 700; color: #fff;
  background: linear-gradient(135deg, #c0392b, #e74c3c); border: 2px solid rgba(255,255,255,0.2);
  border-radius: 8px; cursor: pointer; font-family: 'Inter', sans-serif; transition: transform 0.2s, box-shadow 0.2s;
  display: none;
}
.btn-delete-tournament.visible { display: inline-block; }
.btn-delete-tournament:hover { transform: translateY(-2px); box-shadow: 0 6px 22px rgba(192,57,43,0.5); }

.tournament-wrapper {
  background-image: url('https://4ak4ak.moy.su/Tchak.jpg');
  background-size: cover; background-position: center center; background-repeat: no-repeat;
  padding: 30px 0; border-radius: 12px; position: relative; overflow: hidden;
  max-width: 1100px; margin: 0 auto;
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

.no-tournament-msg {
  text-align: center; color: rgba(255,255,255,0.5); font-size: 18px; font-weight: 600;
  padding: 60px 20px; max-width: 600px; margin: 0 auto;
}

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

.participants-grid { display: flex; gap: 16px; justify-content: space-between; align-items: flex-start; }
.participants-col { flex: 1; min-width: 0; display: flex; flex-direction: column; }
.participants-col.is-mine-col { border: 2px solid rgba(74,158,255,0.5); border-radius: 10px; padding: 12px; box-shadow: 0 0 14px rgba(74,158,255,0.3); animation: pulse-mine 2.5s ease-in-out infinite; }
@keyframes pulse-mine { 0%,100% { box-shadow: 0 0 10px rgba(74,158,255,0.3); } 50% { box-shadow: 0 0 22px rgba(74,158,255,0.6); } }
.participants-col-title { font-size: 16px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; text-align: center; margin-bottom: 14px; padding-bottom: 10px; border-bottom: 1px solid rgba(255,255,255,0.15); }
.participants-col.left .participants-col-title { color: #6fb3ff; }
.participants-col.right .participants-col-title { color: #ff8a65; }
.participant-list { list-style: none; padding: 0; margin: 0 0 10px; display: flex; flex-direction: column; gap: 8px; }
.participant-item { display: flex; align-items: center; gap: 10px; background: rgba(255,255,255,0.06); border-radius: 8px; padding: 8px 12px; color: #fff; font-size: 13px; font-weight: 500; position: relative; flex-wrap: wrap; }
.participant-item.is-me { background: rgba(74,158,255,0.2); border: 1px solid rgba(74,158,255,0.5); }
.participant-item.is-me .participant-avatar { box-shadow: 0 0 8px rgba(74,158,255,0.7); }
.participant-avatar { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 14px; font-weight: 700; color: #fff; flex-shrink: 0; }
.participants-col.left .participant-avatar { background: linear-gradient(135deg, #4a90d9, #6fb3ff); }
.participants-col.right .participant-avatar { background: linear-gradient(135deg, #e07845, #ff8a65); }
.participant-num { color: rgba(255,255,255,0.45); font-size: 11px; font-weight: 600; }
.participant-del { position: absolute; right: 6px; top: 50%; transform: translateY(-50%); background: rgba(255,80,80,0.2); border: none; color: #ff6b6b; font-size: 16px; cursor: pointer; width: 24px; height: 24px; border-radius: 50%; display: none; align-items: center; justify-content: center; line-height: 1; }
.participant-item:hover .participant-del { display: flex; }
.participant-del:hover { background: rgba(255,80,80,0.4); }

.winner-logo-section { display: flex; justify-content: center; padding: 8px 0 4px; }
.winner-logo-section img { width: 32px; height: 32px; border-radius: 50%; border: 2px solid #66bb6a; box-shadow: 0 0 12px rgba(102,187,106,0.7); animation: pulse-win 2s ease-in-out infinite; }
.winner-logo-section.admin-can-cancel img { cursor: pointer; border-color: #ff6b6b; box-shadow: 0 0 12px rgba(255,107,107,0.6); }
.winner-logo-section.admin-can-cancel img:hover { border-color: #ff3333; box-shadow: 0 0 18px rgba(255,51,51,0.9); transform: scale(1.15); }
.winner-logo-section.admin-can-cancel::after { content: '\2715'; color: #ff6b6b; font-size: 10px; font-weight: 700; margin-left: 4px; align-self: center; }
@keyframes pulse-win { 0%,100% { box-shadow: 0 0 8px rgba(102,187,106,0.5); } 50% { box-shadow: 0 0 18px rgba(102,187,106,0.9); } }

.admin-add-row { display: flex; gap: 8px; margin-top: 10px; }
.admin-add-row input { flex: 1; padding: 8px 10px; border: 1px solid rgba(255,255,255,0.15); border-radius: 6px; background: rgba(0,0,0,0.3); color: #fff; font-size: 13px; font-family: 'Inter', sans-serif; }
.admin-add-row input::placeholder { color: rgba(255,255,255,0.3); }
.admin-add-row button { padding: 8px 14px; border: none; border-radius: 6px; font-weight: 600; cursor: pointer; font-size: 12px; white-space: nowrap; font-family: 'Inter', sans-serif; }
.admin-add-row button.add-left { background: #4a90d9; color: #fff; }
.admin-add-row button.add-right { background: #e07845; color: #fff; }

.admin-winner-row { display: flex; gap: 10px; justify-content: center; margin-top: 16px; padding-top: 14px; border-top: 1px solid rgba(255,255,255,0.1); }
.admin-winner-btn { display: flex; align-items: center; gap: 6px; padding: 8px 16px; border: none; border-radius: 8px; font-weight: 700; cursor: pointer; font-size: 13px; font-family: 'Inter', sans-serif; transition: transform 0.15s, box-shadow 0.2s; }
.admin-winner-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.3); }
.admin-winner-btn.win-left { background: linear-gradient(135deg, #4a90d9, #6fb3ff); color: #fff; }
.admin-winner-btn.win-right { background: linear-gradient(135deg, #e07845, #ff8a65); color: #fff; }

.map-center { flex: 0 0 auto; width: 140px; display: flex; flex-direction: column; align-items: center; gap: 10px; padding-top: 40px; }
.map-label { font-size: 11px; font-weight: 700; color: rgba(255,215,0,0.7); text-transform: uppercase; letter-spacing: 1px; text-align: center; }
.map-image { width: 120px; height: 120px; border-radius: 10px; object-fit: cover; border: 2px solid rgba(255,215,0,0.3); box-shadow: 0 4px 16px rgba(0,0,0,0.4); }
.map-vs { font-size: 22px; font-weight: 800; color: rgba(255,215,0,0.6); text-align: center; }
.map-name { font-size: 12px; font-weight: 600; color: rgba(255,255,255,0.8); text-align: center; text-transform: uppercase; letter-spacing: 0.5px; }
.map-placeholder { width: 120px; height: 120px; border-radius: 10px; border: 2px dashed rgba(255,255,255,0.2); display: flex; align-items: center; justify-content: center; text-align: center; font-size: 11px; color: rgba(255,255,255,0.4); line-height: 1.4; padding: 10px; }
.map-name.empty { color: rgba(255,255,255,0.3); }

.prize-toggle-btn { padding: 6px 12px; font-size: 11px; font-weight: 700; color: #fff; background: linear-gradient(135deg, #b8860b, #daa520); border: 1px solid rgba(255,215,0,0.4); border-radius: 6px; cursor: pointer; font-family: 'Inter', sans-serif; transition: transform 0.15s, box-shadow 0.2s; white-space: nowrap; }
.prize-toggle-btn:hover { transform: translateY(-1px); box-shadow: 0 4px 12px rgba(218,165,32,0.5); }
.prize-toggle-btn.active { background: linear-gradient(135deg, #c0392b, #e74c3c); }

.prize-btn { background: rgba(255,215,0,0.15); border: 1px solid rgba(255,215,0,0.3); border-radius: 6px; padding: 2px 6px; cursor: pointer; font-size: 14px; line-height: 1; color: #ffd700; transition: background 0.2s, transform 0.15s; flex-shrink: 0; }
.prize-btn:hover { background: rgba(255,215,0,0.3); transform: scale(1.1); }
.prize-popup { position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%); background: #1a1a2e; border: 1px solid rgba(255,215,0,0.4); border-radius: 14px; padding: 20px; z-index: 10000; display: none; box-shadow: 0 12px 40px rgba(0,0,0,0.8); max-height: 90vh; overflow-y: auto; }
.prize-popup.active { display: block; }
.prize-popup-title { font-size: 16px; font-weight: 700; color: #ffd700; text-align: center; margin-bottom: 14px; text-transform: uppercase; letter-spacing: 1px; }
.prize-popup-skins { display: flex; gap: 14px; justify-content: center; }
.prize-skin-card { text-align: center; }
.prize-skin-card img { width: 100px; height: 60px; object-fit: contain; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); background: rgba(0,0,0,0.3); }
.prize-skin-name { font-size: 12px; color: rgba(255,255,255,0.8); margin-top: 8px; font-weight: 600; }
.prize-popup-close { position: absolute; top: 10px; right: 12px; background: rgba(255,255,255,0.1); border: none; color: #fff; font-size: 20px; cursor: pointer; width: 28px; height: 28px; border-radius: 50%; display: flex; align-items: center; justify-content: center; }
.prize-popup-close:hover { background: rgba(255,80,80,0.4); }
.prize-popup-divider { height: 1px; background: rgba(255,255,255,0.1); margin: 18px 0; }
.prize-winner-section { text-align: center; margin-top: 14px; }
.prize-winner-label { font-size: 13px; color: rgba(255,255,255,0.6); margin-bottom: 8px; }
.prize-winner-img { width: 55px; height: 33px; object-fit: contain; border-radius: 6px; border: 1px solid rgba(255,215,0,0.3); }
.prize-winner-name { font-size: 12px; font-weight: 600; color: #ffd700; }
.prize-assigned-list { display: flex; flex-direction: column; gap: 6px; margin-top: 10px; }
.prize-assigned-item { display: flex; align-items: center; gap: 8px; justify-content: center; flex-wrap: wrap; }
.prize-assigned-item img { width: 55px; height: 33px; object-fit: contain; border-radius: 6px; border: 1px solid rgba(255,215,0,0.3); }
.prize-assigned-nick { font-size: 13px; font-weight: 600; color: #fff; }
.prize-assigned-skin { font-size: 11px; color: rgba(255,255,255,0.6); }
.prize-assigned-role { font-size: 10px; font-weight: 700; padding: 1px 6px; border-radius: 3px; }
.prize-assigned-role.winner { background: rgba(76,175,80,0.3); color: #66bb6a; }
.prize-assigned-role.finalist { background: rgba(255,215,0,0.2); color: #ffd700; }

.common-chat-container { width: 100%; margin-top: 20px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 16px; }
.chat-title { font-size: 14px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 12px; color: #fff; text-align: center; }
.chat-messages { max-height: 200px; overflow-y: auto; display: flex; flex-direction: column; gap: 8px; margin-bottom: 16px; padding: 4px 0; }
.chat-messages::-webkit-scrollbar { width: 6px; }
.chat-messages::-webkit-scrollbar-track { background: rgba(255,255,255,0.05); }
.chat-messages::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.2); border-radius: 3px; }
.chat-msg { background: rgba(255,255,255,0.08); border-radius: 10px; padding: 10px 14px; font-size: 13px; line-height: 1.4; word-break: break-word; position: relative; max-width: 85%; align-self: flex-start; box-shadow: 0 1px 3px rgba(0,0,0,0.2); }
.chat-msg.team-west { background: rgba(74,144,217,0.15); border-left: 3px solid #6fb3ff; align-self: flex-start; }
.chat-msg.team-east { background: rgba(224,120,69,0.15); border-right: 3px solid #ff8a65; align-self: flex-end; }
.chat-msg.team-admin { background: rgba(255,215,0,0.15); border-left: 3px solid #ffd700; align-self: center; max-width: 90%; }
.chat-msg-author { font-size: 11px; font-weight: 700; margin-bottom: 4px; display: block; }
.chat-msg-author.west { color: #6fb3ff; }
.chat-msg-author.east { color: #ff8a65; }
.chat-msg-author.admin { color: #ffd700; }
.chat-msg-text { font-weight: 400; color: rgba(255,255,255,0.9); }
.chat-msg-time { font-size: 9px; color: rgba(255,255,255,0.3); margin-top: 4px; }
.chat-msg-delete { position: absolute; top: 6px; right: 6px; background: rgba(255,80,80,0.2); border: none; color: #ff6b6b; font-size: 14px; cursor: pointer; width: 24px; height: 24px; border-radius: 50%; display: none; align-items: center; justify-content: center; line-height: 1; }
.chat-msg:hover .chat-msg-delete { display: flex; }
.chat-msg-delete:hover { background: rgba(255,80,80,0.4); }
.chat-input-row { display: flex; gap: 10px; }
.chat-input { flex: 1; background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.1); border-radius: 8px; padding: 12px; color: #fff; font-size: 14px; font-family: 'Inter', sans-serif; outline: none; transition: border-color 0.2s; }
.chat-input:focus { border-color: rgba(255,215,0,0.4); }
.chat-input::placeholder { color: rgba(255,255,255,0.3); }
.chat-send { background: rgba(255,215,0,0.15); border: 1px solid rgba(255,215,0,0.3); border-radius: 8px; padding: 0 20px; color: #ffd700; font-size: 14px; font-weight: 600; cursor: pointer; transition: background 0.2s; white-space: nowrap; }
.chat-send:hover { background: rgba(255,215,0,0.25); }
.chat-guest-block { text-align: center; color: rgba(255,255,255,0.4); font-size: 13px; padding: 12px; font-style: italic; }
.admin-badge { display: inline-block; background: rgba(255,215,0,0.2); color: #ffd700; font-size: 9px; font-weight: 700; padding: 1px 5px; border-radius: 3px; margin-left: 4px; vertical-align: middle; }
.you-badge { display: inline-block; background: rgba(74,158,255,0.3); color: #4a9eff; font-size: 9px; font-weight: 700; padding: 1px 5px; border-radius: 3px; margin-left: 4px; vertical-align: middle; }

@media (max-width: 900px) {
  .bracket-row { flex-direction: column; align-items: center; }
  .bracket-col { max-width: 100%; width: 100%; }
  .tournament-title { font-size: 22px; }
  .match { max-width: 100%; }
  .participants-grid { flex-direction: column; align-items: center; gap: 20px; }
  .map-center { padding-top: 0; }
  .team-modal { width: 100%; padding: 20px; }
  .chat-msg { max-width: 95%; }
  .prize-popup-skins { flex-direction: column; align-items: center; }
  .prize-skin-card img { width: 80px; height: 50px; }
}

/* === Connect & Start buttons === */
.match-controls-bar {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  margin-left: 12px;
}
.btn-connect {
  padding: 5px 14px;
  font-size: 12px;
  font-weight: 700;
  color: #fff;
  background: linear-gradient(135deg, #1a73e8, #4285f4);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 6px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  transition: transform 0.2s, box-shadow 0.2s;
  display: none;
  text-decoration: none;
}
.btn-connect.visible { display: inline-block; }
.btn-connect:hover { transform: translateY(-1px); box-shadow: 0 4px 12px rgba(66,133,244,0.5); }
.btn-start {
  padding: 5px 14px;
  font-size: 12px;
  font-weight: 700;
  color: #fff;
  background: linear-gradient(135deg, #e67e22, #f39c12);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 6px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  transition: transform 0.2s, box-shadow 0.2s;
  display: none;
}
.btn-start.visible { display: inline-block; }
.btn-start:hover { transform: translateY(-1px); box-shadow: 0 4px 12px rgba(243,156,18,0.5); }
.btn-start.started {
  background: linear-gradient(135deg, #27ae60, #2ecc71);
}
.participant-link {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
  transition: color 0.2s;
}
.participant-link:hover {
  color: #66c0f4;
  text-decoration: underline;
}
.team-name-link {
  color: inherit;
  text-decoration: none;
  cursor: pointer;
  transition: color 0.2s;
}
.team-name-link:hover {
  color: #66c0f4;
  text-decoration: underline;
}
</style>
</head>
<body>

<div class="tournament-selector-row">
  <label>Турнир:</label>
  <select id="tournamentSelect" onchange="switchTournament()"></select>
</div>

<div class="tournament-create-row" id="tournamentCreateRow">
  <input type="text" id="newTournamentName" placeholder="Название нового турнира" maxlength="40">
  <button class="btn-create" onclick="createTournament()">Создать</button>
</div>

<div style="text-align:center; max-width:850px; margin:0 auto 10px; padding:0 20px;">
  <button class="btn-delete-tournament" id="btnDeleteTournament" onclick="deleteCurrentTournament()">Удалить турнир</button>
</div>

<div class="tournament-wrapper" id="tournamentWrapper" style="display:none;">
  <div class="tournament-title" id="tournamentTitle"></div>
  <div class="bracket-row" id="bracketRow"></div>
</div>

<div class="no-tournament-msg" id="noTournamentMsg" style="display:none;">Пока нет созданных турниров</div>

<div class="join-section" id="joinSection"></div>

<div class="admin-login-row" id="adminLoginRow">
  <input type="password" id="adminPassInput" placeholder="Админ-пароль" onkeydown="if(event.key==='Enter') toggleAdmin()">
  <button class="btn-join" style="padding:12px 26px; font-size:14px;" onclick="toggleAdmin()">Войти как админ</button>
</div>

<div class="admin-panel" id="adminPanel">
  <h3>Админ-панель <span class="admin-active-badge">АКТИВЕН</span></h3>
  <div class="admin-hint">Открой любой матч — появились поля добавления игроков и кнопки подтверждения победы. Чтобы отменить победителя — нажми на картинку под победившей командой.</div>
</div>

<div class="prize-popup" id="prizePopup">
  <button class="prize-popup-close" onclick="closePrizePopup()">&times;</button>
  <div id="prizePopupContent">
    <div class="prize-popup-title">Возможные призы победителю</div>
    <div class="prize-popup-skins">
      <div class="prize-skin-card">
        <img src="https://4ak4ak.moy.su/prizSkin/ak-47_poljot_zhuravlja.png" alt="AK-47">
        <div class="prize-skin-name">AK-47 Полёт журавля</div>
      </div>
      <div class="prize-skin-card">
        <img src="https://4ak4ak.moy.su/prizSkin/m4a1-s_dikij_tusovshhik.png" alt="M4A1-S">
        <div class="prize-skin-name">M4A1-S Дикий тусовщик</div>
      </div>
    </div>
    <div class="prize-popup-divider"></div>
    <div class="prize-popup-title">Возможные призы финалисту</div>
    <div class="prize-popup-skins">
      <div class="prize-skin-card">
        <img src="https://4ak4ak.moy.su/prizSkinFinalis/ak-47_kolymaga.png" alt="AK-47">
        <div class="prize-skin-name">AK-47 Колымага</div>
      </div>
      <div class="prize-skin-card">
        <img src="https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_vzgljad_v_proshloe.png" alt="M4A1-S">
        <div class="prize-skin-name">M4A1-S Взгляд в прошлое</div>
      </div>
    </div>
  </div>
</div>

<div class="team-modal-overlay" id="teamModalOverlay">
  <div class="team-modal">
    <div class="team-modal-header">
      <span class="team-modal-matchup" id="teamModalMatchup">Матч</span><div class="match-controls-bar"><button class="btn-connect" id="btnConnect" onclick="onConnectClick()">Connect</button><button class="btn-start" id="btnStart" onclick="onStartClick()">Start</button></div>
      <button class="team-modal-close" id="teamModalClose">&times;</button>
    </div>
    <div class="participants-grid" id="participantsGrid">
      <div class="participants-col left" id="leftCol">
        <div class="participants-col-title" id="leftTeamName">Команда 1</div>
        <ul class="participant-list" id="leftParticipantList"></ul>
      </div>
      <div class="map-center">
        <div class="map-label">Карта</div>
        <div id="prizeToggleContainer"></div>
        <div class="map-vs">VS</div>
        <div id="mapContainer">
          <img class="map-image" id="mapImage" src="" alt="Карта" style="display:none;">
          <div class="map-placeholder" id="mapPlaceholder">Карта будет выбрана, когда обе команды укомплектованы</div>
        </div>
        <div class="map-name" id="mapName"></div>
      </div>
      <div class="participants-col right" id="rightCol">
        <div class="participants-col-title" id="rightTeamName">Команда 2</div>
        <ul class="participant-list" id="rightParticipantList"></ul>
      </div>
    </div>
    <div id="prizeAssignedContainer"></div>
    <div class="common-chat-container">
      <div class="chat-title">Общий чат матча</div>
      <div class="chat-messages" id="commonChatMessages"></div>
      <div id="chatInputArea">
        <div class="chat-input-row">
          <input type="text" class="chat-input" id="commonChatInput" placeholder="Напишите сообщение..." maxlength="200">
          <button class="chat-send" id="commonChatSend">Отправить</button>
        </div>
      </div>
      <div id="chatGuestBlock" class="chat-guest-block" style="display:none;">Писать в чате могут только участники этого матча</div>
    </div>
  </div>
</div>

<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-database-compat.js"></script>
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
  var MAX_PLAYERS = 5;
  var WIN_LOGO = 'https://4ak4ak.moy.su/logo1.jpg';
  var myNick = null;
  var isAdmin = false;
  var currentTournamentId = null;
  var currentMatchId = null;
  var chatRef = null;
  var allMatches = {};
  var tournaments = {};
  var prizesEnabled = false;
  var qfMatchIds = ['qf_w1','qf_w2','qf_e1','qf_e2'];

    var PRIZE_SKINS_WINNER = [
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_poljot_zhuravlja.png', name: 'AK-47 Полёт журавля' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_dikij_tusovshhik.png', name: 'M4A1-S Дикий тусовщик' },
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_diletanty.png', name: 'AK-47 Дилетанты' },
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_fantomnyj_vreditel.png', name: 'AK-47 Фантомный вредитель' },
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_ledjanoj_ugol.png', name: 'AK-47 Ледяной угол' },
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_nuvo_ruzh.png', name: 'AK-47 Нуво-руж' },
    { url: 'https://4ak4ak.moy.su/prizSkin/ak-47_zhguchaja_jarost.png', name: 'AK-47 Жгучая ярость' },
    { url: 'https://4ak4ak.moy.su/prizSkin/awp_dvojstvennost.png', name: 'AWP Двойственность' },
    { url: 'https://4ak4ak.moy.su/prizSkin/awp_ehlitnoe_snarjazhenie.png', name: 'AWP Элитное снаряжение' },
    { url: 'https://4ak4ak.moy.su/prizSkin/awp_ledjanoj_ugol.png', name: 'AWP Ледяной угол' },
    { url: 'https://4ak4ak.moy.su/prizSkin/awp_mortis.png', name: 'AWP Мортис' },
    { url: 'https://4ak4ak.moy.su/prizSkin/awp_zeljonaja_ehnergija.png', name: 'AWP Зелёная энергия' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_chjornyj_lotos.png', name: 'M4A1-S Чёрный лотос' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_nochnoj_koshmar.png', name: 'M4A1-S Ночной кошмар' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_panel_upravlenija.png', name: 'M4A1-S Панель управления' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_stratosfera.png', name: 'M4A1-S Стратосфера' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a1-s_uedinenie.png', name: 'M4A1-S Уединение' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a4_adok.png', name: 'M4A4 Адок' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a4_grifon.png', name: 'M4A4 Грифон' },
    { url: 'https://4ak4ak.moy.su/prizSkin/m4a4_zubnaja_feja.png', name: 'M4A4 Зубная фея' }
  ];


    var PRIZE_SKINS_FINALIST = [
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/ak-47_kolymaga.png', name: 'AK-47 Колымага' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_vzgljad_v_proshloe.png', name: 'M4A1-S Взгляд в прошлое' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/ak-47_izumrudnye_zavitki.png', name: 'AK-47 Изумрудные завитки' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/ak-47_polunochnyj_gljanec.png', name: 'AK-47 Полуночный глянец' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/ak-47_proryv.png', name: 'AK-47 Прорыв' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/ak-47_slanec.png', name: 'AK-47 Сланец' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_bog_chervej.png', name: 'AWP Бог червей' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_chjornyj_jashhik.png', name: 'AWP Чёрный ящик' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_drevesnaja_gadjuka.png', name: 'AWP Древесная гадюка' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_ehkzoskelet.png', name: 'AWP Экзоскелет' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_ehkzotermija.png', name: 'AWP Экзотермия' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_fobos.png', name: 'AWP Фобос' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_gadjuka.png', name: 'AWP Гадюка' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/awp_lapki.png', name: 'AWP Лапки' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_ehlektrum.png', name: 'M4A1-S Электрум' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_ehmforozavr-s.png', name: 'M4A1-S Эмфорозавр-S' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_gljuk-kraska.png', name: 'M4A1-S Глюк-краска' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_likvidacija.png', name: 'M4A1-S Ликвидация' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_nitro.png', name: 'M4A1-S Нитро' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a1-s_nochnoj_uzhas.png', name: 'M4A1-S Ночной ужас' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a4_grifon.png', name: 'M4A4 Грифон' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a4_master_travli.png', name: 'M4A4 Мастер травли' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a4_turbina.png', name: 'M4A4 Турбина' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a4_zarnica.png', name: 'M4A4 Зарница' },
    { url: 'https://4ak4ak.moy.su/prizSkinFinalis/m4a4_zlobnyj_dajmjo.png', name: 'M4A4 Злобный даймё' }
  ];


  var maps = [
    { url: 'https://4ak4ak.moy.su/dust2.png', name: 'Dust 2' },
    { url: 'https://4ak4ak.moy.su/360fx360f.png', name: 'Nuke' },
    { url: 'https://4ak4ak.moy.su/ancient.jpg', name: 'Ancient' },
    { url: 'https://4ak4ak.moy.su/anubis.png', name: 'Anubis' },
    { url: 'https://4ak4ak.moy.su/cache.png', name: 'Cache' },
    { url: 'https://4ak4ak.moy.su/inferno.png', name: 'Inferno' },
    { url: 'https://4ak4ak.moy.su/mirage.png', name: 'Mirage' },
    { url: 'https://4ak4ak.moy.su/vertigo.png', name: 'Vertigo' }
  ];

  var matchOrder = ['qf_w1','qf_w2','sf_w','final','sf_e','qf_e1','qf_e2'];

  var qfTeams = {
    'qf_w1': { left: 'Команда A', right: 'Команда B' },
    'qf_w2': { left: 'Команда C', right: 'Команда D' },
    'qf_e1': { left: 'Команда E', right: 'Команда F' },
    'qf_e2': { left: 'Команда G', right: 'Команда H' }
  };

  var sfTeamNames = {
    'sf_w': { left: 'Команда I', right: 'Команда J' },
    'sf_e': { left: 'Команда K', right: 'Команда L' }
  };
  var finalTeamNames = { left: 'Команда M', right: 'Команда N' };

  var sfSlots = [
    { sfId: 'sf_w', sfSide: 'left' },
    { sfId: 'sf_w', sfSide: 'right' },
    { sfId: 'sf_e', sfSide: 'left' },
    { sfId: 'sf_e', sfSide: 'right' }
  ];
  var finalSlots = [
    { finalSide: 'left' },
    { finalSide: 'right' }
  ];

  function getUrlParam(n) { var u = new URL(window.location.href); return u.searchParams.get(n); }
  myNick = getUrlParam('user');

  function escapeHtml(t) {
    if (!t) return '';
    return String(t).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;").replace(/'/g,"&#039;");
  }

  function formatTime(ts) {
    var d = new Date(ts); var h = String(d.getHours()).padStart(2,'0'); var m = String(d.getMinutes()).padStart(2,'0');
    return h + ':' + m;
  }

  function isLoggedIn() {
    return myNick && myNick !== 'null' && myNick !== '' && myNick !== 'guest' && myNick !== '\u0413\u043e\u0441\u0442\u044c';
  }

  function tPath(sub) {
    return 'playoff/tournaments/' + currentTournamentId + (sub ? '/' + sub : '');
  }

  function checkAdminNicks() {
    if (!myNick) { return; }
    db.ref('playoff/adminNicks').once('value').then(function(snap) {
      var val = snap.val();
      if (!val) return;
      var found = false;
      if (Array.isArray(val)) {
        found = val.indexOf(myNick) !== -1;
      } else if (typeof val === 'object') {
        if (val[myNick] === true) { found = true; }
        else {
          Object.keys(val).forEach(function(k) {
            if (val[k] === myNick) found = true;
          });
        }
      }
      if (found) {
        document.getElementById('adminLoginRow').classList.add('visible');
      }
    });
  }

  function loadTournaments(cb) {
    db.ref('playoff/tournaments').once('value').then(function(snap) {
      tournaments = snap.val() || {};
      var sorted = Object.keys(tournaments).map(function(id) {
        return { id: id, data: tournaments[id] };
      }).sort(function(a, b) {
        var ta = (a.data && a.data.createdAt) || 0;
        var tb = (b.data && b.data.createdAt) || 0;
        return tb - ta;
      });
      cb(sorted);
    });
  }

  function populateTournamentSelect(selectedId) {
    loadTournaments(function(sorted) {
      var sel = document.getElementById('tournamentSelect');
      var html = '';
      if (sorted.length === 0) {
        html = '<option value="">Нет турниров</option>';
        sel.innerHTML = html;
        document.getElementById('tournamentWrapper').style.display = 'none';
        document.getElementById('noTournamentMsg').style.display = 'block';
        document.getElementById('joinSection').innerHTML = '';
        return;
      }
      sorted.forEach(function(t) {
        var name = (t.data && t.data.name) || t.id;
        html += '<option value="' + escapeHtml(t.id) + '"' + (t.id === selectedId ? ' selected' : '') + '>' + escapeHtml(name) + '</option>';
      });
      sel.innerHTML = html;
      document.getElementById('noTournamentMsg').style.display = 'none';
      document.getElementById('tournamentWrapper').style.display = '';
    });
  }

  window.switchTournament = function() {
    var sel = document.getElementById('tournamentSelect');
    var id = sel.value;
    if (!id) return;
    currentTournamentId = id;
    var name = (tournaments[id] && tournaments[id].name) || id;
    document.getElementById('tournamentTitle').textContent = name;
    buildBracketHtml();
    attachMatchListeners();
    loadAllMatches(function() {
      matchOrder.forEach(function(mid) { var m = allMatches[mid] || {}; syncMap(mid, m); });
      renderBracket(); updateJoinSection();
    });
    listenToMatches();
    loadPrizesEnabled();
  };

  window.createTournament = function() {
    if (!isAdmin) { alert('Только админ может создавать турниры!'); return; }
    var name = document.getElementById('newTournamentName').value.trim();
    if (!name) { alert('Введите название турнира!'); return; }
    var newId = db.ref('playoff/tournaments').push().key;
    var data = { name: name, createdAt: Date.now() };
    db.ref('playoff/tournaments/' + newId).set(data).then(function() {
      currentTournamentId = newId;
      document.getElementById('newTournamentName').value = '';
      populateTournamentSelect(newId);
      document.getElementById('tournamentTitle').textContent = name;
      document.getElementById('tournamentWrapper').style.display = '';
      document.getElementById('noTournamentMsg').style.display = 'none';
      buildBracketHtml();
      attachMatchListeners();
      allMatches = {};
      renderBracket(); updateJoinSection();
      listenToMatches();
      prizesEnabled = false;
    });
  };

  window.deleteCurrentTournament = function() {
    if (!isAdmin) return;
    if (!currentTournamentId) return;
    if (!confirm('Удалить этот турнир полностью? Все данные будут стёрты.')) return;
    db.ref('playoff/tournaments/' + currentTournamentId).remove().then(function() {
      currentTournamentId = null;
      allMatches = {};
      loadTournaments(function(sorted) {
        if (sorted.length > 0) {
          populateTournamentSelect(sorted[0].id);
          currentTournamentId = sorted[0].id;
          var name = (sorted[0].data && sorted[0].data.name) || sorted[0].id;
          document.getElementById('tournamentTitle').textContent = name;
          document.getElementById('tournamentWrapper').style.display = '';
          document.getElementById('noTournamentMsg').style.display = 'none';
          buildBracketHtml();
          attachMatchListeners();
          loadAllMatches(function() {
            matchOrder.forEach(function(mid) { var m = allMatches[mid] || {}; syncMap(mid, m); });
            renderBracket(); updateJoinSection();
          });
          listenToMatches();
          loadPrizesEnabled();
        } else {
          populateTournamentSelect(null);
        }
      });
    });
  };

  function buildBracketHtml() {
    var html = ''
      + '<div class="bracket-col">'
      +   '<div class="conf-label west">\u0417\u0430\u043f\u0430\u0434</div>'
      +   '<div class="round-label">\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u044b</div>'
      +   '<div class="match" data-match-id="qf_w1"><div class="match-row"><div class="match-team"><span class="team-seed">1</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 A</div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">4</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 B</div><div class="match-score"></div></div></div>'
      +   '<div class="match" data-match-id="qf_w2"><div class="match-row"><div class="match-team"><span class="team-seed">2</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 C</div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">3</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 D</div><div class="match-score"></div></div></div>'
      + '</div>'
      + '<div class="bracket-col semifinal-col">'
      +   '<div class="round-label">\u041f\u043e\u043b\u0443\u0444\u0438\u043d\u0430\u043b</div>'
      +   '<div class="match" data-match-id="sf_w"><div class="match-row"><div class="match-team"><span class="team-seed">\u04171</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">\u04172</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div></div>'
      + '</div>'
      + '<div class="bracket-col final-col">'
      +   '<div class="final-box">'
      +     '<div class="final-title">\u0424\u0438\u043d\u0430\u043b</div>'
      +     '<div class="match final-match" data-match-id="final"><div class="match-row"><div class="match-team"><span class="team-seed">\u0417</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">\u0412</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div></div>'
      +   '</div>'
      + '</div>'
      + '<div class="bracket-col semifinal-col">'
      +   '<div class="round-label">\u041f\u043e\u043b\u0443\u0444\u0438\u043d\u0430\u043b</div>'
      +   '<div class="match" data-match-id="sf_e"><div class="match-row"><div class="match-team"><span class="team-seed">\u04121</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">\u04122</span> <span class="sf-team-name">\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...</span></div><div class="match-score"></div></div></div>'
      + '</div>'
      + '<div class="bracket-col">'
      +   '<div class="conf-label east">\u0412\u043e\u0441\u0442\u043e\u043a</div>'
      +   '<div class="round-label">\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u044b</div>'
      +   '<div class="match" data-match-id="qf_e1"><div class="match-row"><div class="match-team"><span class="team-seed">1</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 E</div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">4</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 F</div><div class="match-score"></div></div></div>'
      +   '<div class="match" data-match-id="qf_e2"><div class="match-row"><div class="match-team"><span class="team-seed">2</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 G</div><div class="match-score"></div></div><div class="match-divider"></div><div class="match-row"><div class="match-team"><span class="team-seed">3</span> \u041a\u043e\u043c\u0430\u043d\u0434\u0430 H</div><div class="match-score"></div></div></div>'
      + '</div>';
    document.getElementById('bracketRow').innerHTML = html;
  }

  function attachMatchListeners() {
    document.querySelectorAll('.match').forEach(function(m) {
      m.addEventListener('click', function(e) { e.preventDefault(); openModal(this); });
    });
  }

  window.toggleAdmin = function() {
    var p = document.getElementById('adminPassInput').value;
    if (!isAdmin) {
      if (p === ADMIN_PASSWORD) {
        isAdmin = true;
        document.getElementById('adminPassInput').value = '';
        document.getElementById('adminPanel').style.display = 'block';
        document.getElementById('tournamentCreateRow').classList.add('visible');
        document.getElementById('btnDeleteTournament').classList.add('visible');
        renderBracket();
      } else { alert('\u041d\u0435\u0432\u0435\u0440\u043d\u044b\u0439 \u043f\u0430\u0440\u043e\u043b\u044c!'); }
    } else {
      isAdmin = false;
      document.getElementById('adminPanel').style.display = 'none';
      document.getElementById('tournamentCreateRow').classList.remove('visible');
      document.getElementById('btnDeleteTournament').classList.remove('visible');
      renderBracket();
    }
  };

  window.onStartClick = function() {
    if (!isAdmin) return;
    if (!currentMatchId) return;
    var m = allMatches[currentMatchId] || {};
    var newStarted = !m.started;
    db.ref(tPath('matches/' + currentMatchId + '/started')).set(newStarted).then(function() {
      allMatches[currentMatchId] = allMatches[currentMatchId] || {};
      allMatches[currentMatchId].started = newStarted;
      var btnStart = document.getElementById('btnStart');
      var btnConnect = document.getElementById('btnConnect');
      if (btnStart) {
        if (newStarted) { btnStart.classList.add('started'); btnStart.textContent = 'Started'; }
        else { btnStart.classList.remove('started'); btnStart.textContent = 'Start'; }
      }
      if (btnConnect) {
        if (newStarted) { btnConnect.classList.add('visible'); }
        else { btnConnect.classList.remove('visible'); }
      }
    });
  };

  window.onConnectClick = function() {
    var m = allMatches[currentMatchId] || {};
    if (!m.started) return;
    var connectUrl = m.connectUrl || '';
    if (connectUrl) {
      window.open(connectUrl, '_blank');
    } else {
      alert('Ссылка для подключения появится soon!');
    }
  };

  function loadAllMatches(cb) {
    if (!currentTournamentId) { allMatches = {}; if (cb) cb(); return; }
    db.ref(tPath('matches')).once('value').then(function(snap) {
      allMatches = snap.val() || {};
      if (cb) cb();
    });
  }

  function loadPrizesEnabled() {
    if (!currentTournamentId) { prizesEnabled = false; return; }
    db.ref(tPath('prizesEnabled')).once('value').then(function(snap) {
      prizesEnabled = snap.val() === true;
    });
  }

  window.togglePrizes = function() {
    if (!isAdmin || !currentTournamentId) return;
    prizesEnabled = !prizesEnabled;
    db.ref(tPath('prizesEnabled')).set(prizesEnabled);
    renderPrizeToggleBtn();
    var m = allMatches[currentMatchId] || {};
    var left = m.left || [], right = m.right || [];
    document.getElementById('leftParticipantList').innerHTML = buildParticipantList(currentMatchId,'left',left);
    document.getElementById('rightParticipantList').innerHTML = buildParticipantList(currentMatchId,'right',right);
    renderPrizeAssigned(currentMatchId);
  };

  function renderPrizeToggleBtn() {
    var container = document.getElementById('prizeToggleContainer');
    if (!container) return;
    if (!isAdmin || currentMatchId !== 'final') { container.innerHTML = ''; return; }
    var label = prizesEnabled ? '\u0423\u0431\u0440\u0430\u0442\u044c \u043f\u0440\u0438\u0437\u044b' : '\u0414\u043e\u0431\u0430\u0432\u0438\u0442\u044c \u043f\u0440\u0438\u0437\u044b';
    var cls = prizesEnabled ? 'prize-toggle-btn active' : 'prize-toggle-btn';
    container.innerHTML = '<button class="' + cls + '" onclick="event.stopPropagation();togglePrizes()">' + label + '</button>';
  }

  function isTeamFull(arr) { return arr && arr.length >= MAX_PLAYERS; }

  function isUserInAnyMatch(nick) {
    if (!nick) return false;
    for (var mid in allMatches) {
      var m = allMatches[mid];
      if (m && m.left && m.left.indexOf(nick) !== -1) return true;
      if (m && m.right && m.right.indexOf(nick) !== -1) return true;
    }
    return false;
  }

  function isUserInMatch(nick, mid) {
    if (!nick) return false;
    var m = allMatches[mid] || {};
    if (m.left && m.left.indexOf(nick) !== -1) return true;
    if (m.right && m.right.indexOf(nick) !== -1) return true;
    return false;
  }

  function canChat() {
    if (isAdmin) return true;
    if (!isLoggedIn()) return false;
    if (!currentMatchId) return false;
    return isUserInMatch(myNick, currentMatchId);
  }

  function updateChatInputVisibility() {
    var inputArea = document.getElementById('chatInputArea');
    var guestBlock = document.getElementById('chatGuestBlock');
    if (canChat()) { inputArea.style.display = ''; guestBlock.style.display = 'none'; }
    else { inputArea.style.display = 'none'; guestBlock.style.display = ''; }
  }

  function hasQfSpace() {
    for (var i = 0; i < qfMatchIds.length; i++) {
      var mid = qfMatchIds[i];
      var m = allMatches[mid] || {};
      var l = m.left || [], r = m.right || [];
      if (l.length < MAX_PLAYERS || r.length < MAX_PLAYERS) return true;
    }
    return false;
  }

  function updateJoinSection() {
    var s = document.getElementById('joinSection');
    if (!currentTournamentId) { s.innerHTML = ''; return; }
    if (!isLoggedIn()) { s.innerHTML = '<p class="guest-warning"><span class="guest-warning-text">\u0412\u043e\u0439\u0434\u0438\u0442\u0435 \u043d\u0430 \u0441\u0430\u0439\u0442, \u0447\u0442\u043e\u0431\u044b \u0443\u0447\u0430\u0441\u0442\u0432\u043e\u0432\u0430\u0442\u044c \u0432 \u0442\u0443\u0440\u043d\u0438\u0440\u0435</span></p>'; return; }
    if (isUserInAnyMatch(myNick)) { s.innerHTML = '<p class="joined-msg">\u0422\u044b \u0443\u0436\u0435 \u0432 \u0442\u0443\u0440\u043d\u0438\u0440\u0435! \u041d\u0430\u0439\u0434\u0438 \u0441\u0432\u043e\u044e \u043a\u043e\u043c\u0430\u043d\u0434\u0443 \u0432 \u0441\u0435\u0442\u043a\u0435.</p>'; return; }
    if (!hasQfSpace()) { s.innerHTML = '<p class="tournament-full-msg">\u0422\u0443\u0440\u043d\u0438\u0440\u043d\u0430\u044f \u0442\u0430\u0431\u043b\u0438\u0446\u0430 \u043f\u043e\u043b\u043d\u043e\u0441\u0442\u044c\u044e \u0437\u0430\u043f\u043e\u043b\u043d\u0435\u043d\u0430!</p>'; return; }
    s.innerHTML = '<button class="btn-join" onclick="joinTournament()">\u0423\u0447\u0430\u0441\u0442\u0432\u043e\u0432\u0430\u0442\u044c</button>';
  }

  window.joinTournament = function() {
    if (!isLoggedIn()) { alert('\u0412\u043e\u0439\u0434\u0438\u0442\u0435 \u043d\u0430 \u0441\u0430\u0439\u0442, \u0447\u0442\u043e\u0431\u044b \u0443\u0447\u0430\u0441\u0442\u0432\u043e\u0432\u0430\u0442\u044c \u0432 \u0442\u0443\u0440\u043d\u0438\u0440\u0435!'); return; }
    var candidates = []; var sides = ['left','right'];
    qfMatchIds.forEach(function(mid) {
      sides.forEach(function(side) {
        var m = allMatches[mid] || {}; var arr = m[side] || [];
        if (arr.length < MAX_PLAYERS) candidates.push({ matchId: mid, side: side });
      });
    });
    if (candidates.length === 0) { alert('\u0412\u0441\u0435 \u043a\u043e\u043c\u0430\u043d\u0434\u044b \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0437\u0430\u043f\u043e\u043b\u043d\u0435\u043d\u044b!'); return; }
    var pick = candidates[Math.floor(Math.random() * candidates.length)];
    var arr = (allMatches[pick.matchId] || {})[pick.side] || [];
    arr.push(myNick);
    db.ref(tPath('matches/' + pick.matchId + '/' + pick.side)).set(arr).then(function() {
      allMatches[pick.matchId] = allMatches[pick.matchId] || {};
      allMatches[pick.matchId][pick.side] = arr;
      updateJoinSection(); renderBracket();
    });
  };

  function renderBracket() {
    matchOrder.forEach(function(mid) {
      var el = document.querySelector('[data-match-id="' + mid + '"]'); if (!el) return;
      var m = allMatches[mid] || {};
      var left = m.left || [], right = m.right || [], winner = m.winner || null;
      var rows = el.querySelectorAll('.match-row');
      var lne = rows[0].querySelector('.match-team'), rne = rows[1].querySelector('.match-team');
      var lse = rows[0].querySelector('.match-score'), rse = rows[1].querySelector('.match-score');
      if (mid.startsWith('qf_')) {
        var teams = qfTeams[mid];
        lne.innerHTML = '<span class="team-seed">' + (rows[0].querySelector('.team-seed') ? rows[0].querySelector('.team-seed').textContent : '') + '</span> ' + teams.left;
        rne.innerHTML = '<span class="team-seed">' + (rows[1].querySelector('.team-seed') ? rows[1].querySelector('.team-seed').textContent : '') + '</span> ' + teams.right;
      } else {
        var sl = lne.querySelector('.sf-team-name'), sr = rne.querySelector('.sf-team-name');
        if (sl && sr) {
          if (mid === 'sf_w' || mid === 'sf_e') { var names = sfTeamNames[mid] || {left:'?',right:'?'}; sl.textContent = left.length>0?names.left:'\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...'; sr.textContent = right.length>0?names.right:'\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...'; }
          else if (mid === 'final') { sl.textContent = left.length>0?finalTeamNames.left:'\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...'; sr.textContent = right.length>0?finalTeamNames.right:'\u041e\u0436\u0438\u0434\u0430\u043d\u0438\u0435...'; }
        }
      }
      lse.textContent=''; rse.textContent='';
      rows[0].classList.remove('winner'); rows[1].classList.remove('winner');
      rows[0].classList.remove('is-mine-row'); rows[1].classList.remove('is-mine-row');
      if (myNick) { if (left.indexOf(myNick)!==-1) rows[0].classList.add('is-mine-row'); if (right.indexOf(myNick)!==-1) rows[1].classList.add('is-mine-row'); }
      if (winner==='left') rows[0].classList.add('winner');
      if (winner==='right') rows[1].classList.add('winner');
      el.classList.remove('is-mine');
      if (myNick && (left.indexOf(myNick)!==-1 || right.indexOf(myNick)!==-1)) el.classList.add('is-mine');
    });
  }

  function syncMap(mid, m) {
    var bothFull = isTeamFull(m.left) && isTeamFull(m.right);
    if (bothFull && !m.map) { db.ref(tPath('matches/' + mid + '/map')).set(maps[Math.floor(Math.random()*maps.length)]); }
    if (!bothFull && m.map) { db.ref(tPath('matches/' + mid + '/map')).remove(); }
  }

    window.showPrizePopup = function() {
    var c = document.getElementById('prizePopupContent');
    var html = '<div class="prize-popup-title">Возможные призы победителю</div>';
    html += '<div class="prize-popup-skins" style="flex-wrap:wrap;max-width:600px;">';
    PRIZE_SKINS_WINNER.forEach(function(s) {
      html += '<div class="prize-skin-card"><img src="'+s.url+'" alt="'+s.name+'"><div class="prize-skin-name">'+s.name+'</div></div>';
    });
    html += '</div>';
    html += '<div class="prize-popup-divider"></div>';
    html += '<div class="prize-popup-title">Возможные призы финалисту</div>';
    html += '<div class="prize-popup-skins" style="flex-wrap:wrap;max-width:600px;">';
    PRIZE_SKINS_FINALIST.forEach(function(s) {
      html += '<div class="prize-skin-card"><img src="'+s.url+'" alt="'+s.name+'"><div class="prize-skin-name">'+s.name+'</div></div>';
    });
    html += '</div>';
    c.innerHTML = html;
    document.getElementById('prizePopup').classList.add('active');
  };

  window.closePrizePopup = function() {
    document.getElementById('prizePopup').classList.remove('active');
  };

  function renderPrizeAssigned(mid) {
    var container = document.getElementById('prizeAssignedContainer');
    if (!container) return;
    if (mid !== 'final' || !prizesEnabled) { container.innerHTML = ''; return; }
    var m = allMatches[mid] || {};
    if (!m.winner || !m.prizes) { container.innerHTML = ''; return; }
    var winSide = m.winner;
    var loseSide = winSide === 'left' ? 'right' : 'left';
    var winners = m[winSide] || [];
    var finalists = m[loseSide] || [];
    var prizes = m.prizes || {};
    var html = '<div class="prize-winner-section">';
    var hasWin = false, hasFin = false;
    winners.forEach(function(nick) {
      var p = prizes['win_' + nick];
      if (p) { hasWin = true; }
    });
    finalists.forEach(function(nick) {
      var p = prizes['fin_' + nick];
      if (p) { hasFin = true; }
    });
    if (hasWin) {
      html += '<div class="prize-winner-label">\u041f\u0440\u0438\u0437\u044b \u043f\u043e\u0431\u0435\u0434\u0438\u0442\u0435\u043b\u044f\u043c:</div>';
      html += '<div class="prize-assigned-list">';
      winners.forEach(function(nick) {
        var p = prizes['win_' + nick];
        if (p) {
          html += '<div class="prize-assigned-item"><img src="'+p.url+'" class="prize-winner-img" alt=""><span class="prize-assigned-nick">'+escapeHtml(nick)+'</span><span class="prize-assigned-skin">'+escapeHtml(p.name)+'</span><span class="prize-assigned-role winner">\u041f\u043e\u0431\u0435\u0434\u0438\u0442\u0435\u043b\u044c</span></div>';
        }
      });
      html += '</div>';
    }
    if (hasFin) {
      html += '<div class="prize-winner-label" style="margin-top:12px;">\u041f\u0440\u0438\u0437\u044b \u0444\u0438\u043d\u0430\u043b\u0438\u0441\u0442\u0430\u043c:</div>';
      html += '<div class="prize-assigned-list">';
      finalists.forEach(function(nick) {
        var p = prizes['fin_' + nick];
        if (p) {
          html += '<div class="prize-assigned-item"><img src="'+p.url+'" class="prize-winner-img" alt=""><span class="prize-assigned-nick">'+escapeHtml(nick)+'</span><span class="prize-assigned-skin">'+escapeHtml(p.name)+'</span><span class="prize-assigned-role finalist">\u0424\u0438\u043d\u0430\u043b\u0438\u0441\u0442</span></div>';
        }
      });
      html += '</div>';
    }
    html += '</div>';
    container.innerHTML = html;
  }

  function openModal(matchEl) {
    var mid = matchEl.getAttribute('data-match-id'); currentMatchId = mid;
    loadAllMatches(function() {
      var m = allMatches[mid] || {};
      var left = m.left || [], right = m.right || [];
      var teams;
      if (mid.startsWith('qf_')) teams = qfTeams[mid];
      else if (mid==='sf_w'||mid==='sf_e') teams = sfTeamNames[mid]||{left:'?',right:'?'};
      else if (mid==='final') teams = finalTeamNames;
      else teams = {left:'\u041a\u043e\u043c\u0430\u043d\u0434\u0430 1',right:'\u041a\u043e\u043c\u0430\u043d\u0434\u0430 2'};
      document.getElementById('teamModalMatchup').textContent = teams.left+' vs '+teams.right;
      var btnStart = document.getElementById('btnStart');
      var btnConnect = document.getElementById('btnConnect');
      if (btnStart) { btnStart.classList.remove('visible','started'); }
      if (btnConnect) { btnConnect.classList.remove('visible'); }
      if (btnStart && isAdmin) { btnStart.classList.add('visible'); }
      if (m.started) {
        if (btnStart) { btnStart.classList.add('started'); btnStart.textContent = 'Started'; }
        if (btnConnect) { btnConnect.classList.add('visible'); }
      } else {
        if (btnStart) { btnStart.textContent = 'Start'; }
      }
      document.getElementById('leftTeamName').innerHTML = '<a class="team-name-link" href="/index/8-0-'+encodeURIComponent(teams.left)+'" target="_blank">'+escapeHtml(teams.left)+'</a>';
      document.getElementById('rightTeamName').innerHTML = '<a class="team-name-link" href="/index/8-0-'+encodeURIComponent(teams.right)+'" target="_blank">'+escapeHtml(teams.right)+'</a>';
      var lc = document.getElementById('leftCol'), rc = document.getElementById('rightCol');
      lc.classList.remove('is-mine-col'); rc.classList.remove('is-mine-col');
      if (myNick && left.indexOf(myNick)!==-1) lc.classList.add('is-mine-col');
      if (myNick && right.indexOf(myNick)!==-1) rc.classList.add('is-mine-col');
      document.getElementById('leftParticipantList').innerHTML = buildParticipantList(mid,'left',left);
      document.getElementById('rightParticipantList').innerHTML = buildParticipantList(mid,'right',right);
      var ow = document.querySelector('.winner-logo-section'); if (ow) ow.remove();
      if (m.winner) {
        var wc = m.winner==='left'?lc:rc;
        var ws = document.createElement('div'); ws.className='winner-logo-section';
        if (isAdmin) ws.classList.add('admin-can-cancel');
        ws.innerHTML = '<img src="'+WIN_LOGO+'" alt="\u041f\u043e\u0431\u0435\u0434\u0438\u0442\u0435\u043b\u044c"'+(isAdmin?' onclick="cancelWinner(\''+mid+'\')"':'')+'>';
        wc.appendChild(ws);
      }
      renderPrizeToggleBtn();
      renderPrizeAssigned(mid);
      if (isAdmin) addAdminControls(mid, teams);
      var bothFull = isTeamFull(left) && isTeamFull(right);
      var mapUrl = (bothFull && m.map) ? m.map.url : null;
      var mapName = (bothFull && m.map) ? m.map.name : null;
      var mi = document.getElementById('mapImage'), mp = document.getElementById('mapPlaceholder'), mn = document.getElementById('mapName');
      if (mapUrl) { mi.src=mapUrl; mi.alt=mapName; mi.style.display='block'; mp.style.display='none'; mn.textContent=mapName; mn.classList.remove('empty'); }
      else { mi.style.display='none'; mp.style.display='flex'; mn.textContent = bothFull?'\u0412\u044b\u0431\u0438\u0440\u0430\u0435\u0442\u0441\u044f...':'\u041d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u0430'; mn.classList.add('empty'); }
      updateChatInputVisibility();
      if (chatRef) chatRef.off();
      chatRef = db.ref(tPath('chats/' + mid));
      chatRef.on('value', renderChat);
      document.getElementById('teamModalOverlay').classList.add('active');
    });
  }

  function buildParticipantList(mid, side, arr) {
    var isFinal = (mid === 'final');
    var html = '';
    for (var i = 0; i < MAX_PLAYERS; i++) {
      var nick = arr[i]||null; var num = i+1;
      var isMe = myNick && nick && nick===myNick;
      var del = isAdmin && nick ? '<button class="participant-del" onclick="removeParticipant(\''+mid+'\',\''+side+'\','+i+')">&times;</button>' : '';
      var bg = side==='left'?'linear-gradient(135deg,#4a90d9,#6fb3ff)':'linear-gradient(135deg,#e07845,#ff8a65)';
      var av = nick ? escapeHtml(nick.charAt(0).toUpperCase()) : num;
      var mb = isMe ? '<span class="you-badge">\u0422\u042b</span>' : '';
      var prizeBtn = '';
      if (isFinal && nick && prizesEnabled) {
        prizeBtn = '<button class="prize-btn" onclick="event.stopPropagation();showPrizePopup()" title="\u041f\u0440\u0438\u0437\u044b">\u{1F381}</button>';
      }
      if (side === 'left') {
        html += '<li class="participant-item'+(isMe?' is-me':'')+'"><div class="participant-avatar" style="'+(nick?'':'opacity:0.3;')+'background:'+bg+'">'+av+'</div><span class="participant-num">#'+num+'</span> '+(nick?'<a class="participant-link" href="/index/8-0-'+encodeURIComponent(nick)+'" target="_blank">'+escapeHtml(nick)+'</a>'+mb:'<span style="color:rgba(255,255,255,0.25)">\u0421\u0432\u043e\u0431\u043e\u0434\u043d\u043e</span>')+prizeBtn+del+'</li>';
      } else {
        html += '<li class="participant-item'+(isMe?' is-me':'')+'"><div class="participant-avatar" style="'+(nick?'':'opacity:0.3;')+'background:'+bg+'">'+av+'</div><span class="participant-num">#'+num+'</span> '+(nick?'<a class="participant-link" href="/index/8-0-'+encodeURIComponent(nick)+'" target="_blank">'+escapeHtml(nick)+'</a>'+mb:'<span style="color:rgba(255,255,255,0.25)">\u0421\u0432\u043e\u0431\u043e\u0434\u043d\u043e</span>')+prizeBtn+del+'</li>';
      }
    }
    return html;
  }

  function addAdminControls(mid, teams) {
    var lc = document.getElementById('leftCol'), rc = document.getElementById('rightCol');
    var ol = lc.querySelector('.admin-add-row'); if (ol) ol.remove();
    var or = rc.querySelector('.admin-add-row'); if (or) or.remove();
    var lr = document.createElement('div'); lr.className='admin-add-row';
    lr.innerHTML = '<input type="text" placeholder="\u0414\u043e\u0431\u0430\u0432\u0438\u0442\u044c \u0438\u0433\u0440\u043e\u043a\u0430" id="addInput_left_'+mid+'" maxlength="20"><button class="add-left" onclick="addPlayer(\''+mid+'\',\'left\')">+</button>';
    var rr = document.createElement('div'); rr.className='admin-add-row';
    rr.innerHTML = '<input type="text" placeholder="\u0414\u043e\u0431\u0430\u0432\u0438\u0442\u044c \u0438\u0433\u0440\u043e\u043a\u0430" id="addInput_right_'+mid+'" maxlength="20"><button class="add-right" onclick="addPlayer(\''+mid+'\',\'right\')">+</button>';
    var lwl = lc.querySelector('.winner-logo-section'); if (lwl) lc.insertBefore(lr,lwl); else lc.appendChild(lr);
    var rwl = rc.querySelector('.winner-logo-section'); if (rwl) rc.insertBefore(rr,rwl); else rc.appendChild(rr);
    var ow = document.querySelector('.admin-winner-row'); if (ow) ow.remove();
    var wr = document.createElement('div'); wr.className='admin-winner-row';
    var m = allMatches[mid]||{}; var ld = m.winner?'disabled':'', rd = m.winner?'disabled':'';
    wr.innerHTML = '<button class="admin-winner-btn win-left" '+ld+' onclick="confirmWinner(\''+mid+'\',\'left\')">\u041f\u043e\u0431\u0435\u0434\u0430: '+escapeHtml(teams.left)+'</button><button class="admin-winner-btn win-right" '+rd+' onclick="confirmWinner(\''+mid+'\',\'right\')">\u041f\u043e\u0431\u0435\u0434\u0430: '+escapeHtml(teams.right)+'</button>';
    document.querySelector('.participants-grid').after(wr);
  }

  window.addPlayer = function(mid, side) {
    var inp = document.getElementById('addInput_'+side+'_'+mid); var nick = inp.value.trim(); if (!nick) return;
    var arr = (allMatches[mid]||{})[side]||[]; if (arr.length >= MAX_PLAYERS) { alert('\u041a\u043e\u043c\u0430\u043d\u0434\u0430 \u0437\u0430\u043f\u043e\u043b\u043d\u0435\u043d\u0430!'); return; }
    arr.push(nick);
    db.ref(tPath('matches/'+mid+'/'+side)).set(arr).then(function() {
      allMatches[mid] = allMatches[mid]||{}; allMatches[mid][side] = arr;
      inp.value=''; syncMap(mid, allMatches[mid]);
      openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection();
    });
  };

  window.removeParticipant = function(mid, side, idx) {
    var arr = (allMatches[mid]||{})[side]||[]; arr.splice(idx,1);
    db.ref(tPath('matches/'+mid+'/'+side)).set(arr).then(function() {
      allMatches[mid] = allMatches[mid]||{}; allMatches[mid][side] = arr;
      syncMap(mid, allMatches[mid]);
      openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection();
    });
  };

  window.confirmWinner = function(mid, side) {
    var m = allMatches[mid]||{}; if (m.winner) { alert('\u041f\u043e\u0431\u0435\u0434\u0438\u0442\u0435\u043b\u044c \u0443\u0436\u0435 \u0432\u044b\u0431\u0440\u0430\u043d!'); return; }
    var winArr = m[side]||[];
    var updates = {};
    updates[tPath('matches/'+mid+'/winner')] = side;
    if (mid.startsWith('qf_')) {
      var distribution = {};
      winArr.forEach(function(p) {
        var avail = sfSlots.filter(function(s) { var a = (allMatches[s.sfId]||{})[s.sfSide]||[]; return a.length < MAX_PLAYERS; });
        if (avail.length===0) return;
        var slot = avail[Math.floor(Math.random()*avail.length)];
        var sfArr = (allMatches[slot.sfId]||{})[slot.sfSide]||[]; sfArr.push(p);
        updates[tPath('matches/'+slot.sfId+'/'+slot.sfSide)] = sfArr;
        allMatches[slot.sfId] = allMatches[slot.sfId]||{}; allMatches[slot.sfId][slot.sfSide] = sfArr;
        distribution[p] = { sfId: slot.sfId, sfSide: slot.sfSide };
      });
      updates[tPath('distribution/'+mid)] = distribution;
    }
    if (mid==='sf_w'||mid==='sf_e') {
      var finDist = {};
      winArr.forEach(function(p) {
        var avail = finalSlots.filter(function(s) { var a = (allMatches['final']||{})[s.finalSide]||[]; return a.length < MAX_PLAYERS; });
        if (avail.length===0) return;
        var slot = avail[Math.floor(Math.random()*avail.length)];
        var finArr = (allMatches['final']||{})[slot.finalSide]||[]; finArr.push(p);
        updates[tPath('matches/final/'+slot.finalSide)] = finArr;
        allMatches['final'] = allMatches['final']||{}; allMatches['final'][slot.finalSide] = finArr;
        finDist[p] = { finalSide: slot.finalSide };
      });
      updates[tPath('distribution/'+mid)] = finDist;
    }
    if (mid === 'final' && prizesEnabled) {
      var loseSide = side === 'left' ? 'right' : 'left';
      var winners = m[side] || [];
      var finalists = m[loseSide] || [];
      var prizes = {};
      winners.forEach(function(nick) {
        var skin = PRIZE_SKINS_WINNER[Math.floor(Math.random() * PRIZE_SKINS_WINNER.length)];
        prizes['win_' + nick] = { url: skin.url, name: skin.name };
      });
      finalists.forEach(function(nick) {
        var skin = PRIZE_SKINS_FINALIST[Math.floor(Math.random() * PRIZE_SKINS_FINALIST.length)];
        prizes['fin_' + nick] = { url: skin.url, name: skin.name };
      });
      updates[tPath('matches/final/prizes')] = prizes;
    }
    db.ref().update(updates).then(function() {
      if (mid === 'final') {
        var loseSide2 = side === 'left' ? 'right' : 'left';
        var winners2 = (allMatches['final']||{})[side] || [];
        var runnersUp2 = (allMatches['final']||{})[loseSide2] || [];
        db.ref(tPath('finalResult')).set({ winners: winners2, runnersUp: runnersUp2, time: Date.now() });
      }
      loadAllMatches(function() {
        openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection();
      });
    });
  };

  window.cancelWinner = function(mid) {
    if (!isAdmin) return;
    var m = allMatches[mid]||{}; if (!m.winner) return;
    if (!confirm('\u041e\u0442\u043c\u0435\u043d\u0438\u0442\u044c \u043f\u043e\u0431\u0435\u0434\u0438\u0442\u0435\u043b\u044f \u044d\u0442\u043e\u0433\u043e \u043c\u0430\u0442\u0447\u0430? \u0418\u0433\u0440\u043e\u043a\u0438 \u0431\u0443\u0434\u0443\u0442 \u0443\u0431\u0440\u0430\u043d\u044b \u0438\u0437 \u0441\u043b\u0435\u0434\u0443\u044e\u0449\u0435\u0433\u043e \u0440\u0430\u0443\u043d\u0434\u0430.')) return;
    var updates = {};
    updates[tPath('matches/'+mid+'/winner')] = null;
    if (mid === 'final') {
      db.ref(tPath('finalResult')).remove();
      updates[tPath('matches/final/prizes')] = null;
    }
    if (mid.startsWith('qf_')) {
      db.ref(tPath('distribution/'+mid)).once('value').then(function(snap) {
        var dist = snap.val()||{}; var sfToRemove = {};
        Object.keys(dist).forEach(function(p) { var d = dist[p]; var key = d.sfId+'/'+d.sfSide; if (!sfToRemove[key]) sfToRemove[key]=[]; sfToRemove[key].push(p); });
        Object.keys(sfToRemove).forEach(function(key) {
          var parts = key.split('/'); var sfId=parts[0], sfSide=parts[1];
          var arr = (allMatches[sfId]||{})[sfSide]||[];
          sfToRemove[key].forEach(function(p) { var idx = arr.indexOf(p); if (idx!==-1) arr.splice(idx,1); });
          updates[tPath('matches/'+key)] = arr;
          allMatches[sfId] = allMatches[sfId]||{}; allMatches[sfId][sfSide] = arr;
        });
        updates[tPath('distribution/'+mid)] = null;
        db.ref().update(updates).then(function() {
          loadAllMatches(function() { openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection(); });
        });
      });
      return;
    }
    if (mid==='sf_w'||mid==='sf_e') {
      db.ref(tPath('distribution/'+mid)).once('value').then(function(snap) {
        var dist = snap.val()||{}; var finToRemove = {};
        Object.keys(dist).forEach(function(p) { var d = dist[p]; var key = d.finalSide; if (!finToRemove[key]) finToRemove[key]=[]; finToRemove[key].push(p); });
        Object.keys(finToRemove).forEach(function(finSide) {
          var arr = (allMatches['final']||{})[finSide]||[];
          finToRemove[finSide].forEach(function(p) { var idx = arr.indexOf(p); if (idx!==-1) arr.splice(idx,1); });
          updates[tPath('matches/final/'+finSide)] = arr;
          allMatches['final'] = allMatches['final']||{}; allMatches['final'][finSide] = arr;
        });
        updates[tPath('distribution/'+mid)] = null;
        db.ref().update(updates).then(function() {
          loadAllMatches(function() { openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection(); });
        });
      });
      return;
    }
    db.ref().update(updates).then(function() {
      loadAllMatches(function() { openModal(document.querySelector('[data-match-id="'+mid+'"]')); renderBracket(); updateJoinSection(); });
    });
  };

  function renderChat(snap) {
    var c = document.getElementById('commonChatMessages');
    if (!snap || !snap.exists()) { c.innerHTML = '<div style="font-size:12px;color:rgba(255,255,255,0.3);text-align:center;padding:16px;">\u0417\u0434\u0435\u0441\u044c \u043f\u043e\u043a\u0430 \u0442\u0438\u0445\u043e. \u041d\u0430\u043f\u0438\u0448\u0438\u0442\u0435 \u0447\u0442\u043e-\u043d\u0438\u0431\u0443\u0434\u044c!</div>'; return; }
    var data = snap.val();
    var keys = Object.keys(data).sort(function(a,b) { return (data[a].time||0) - (data[b].time||0); });
    var html = '';
    keys.forEach(function(k) {
      var msg = data[k]; var cls='', colCls='', badge='';
      if (msg.isAdmin) { cls='team-admin'; colCls='admin'; badge='<span class="admin-badge">ADMIN</span>'; }
      else if (msg.side==='left') { cls='team-west'; colCls='west'; }
      else { cls='team-east'; colCls='east'; }
      var del = isAdmin ? '<button class="chat-msg-delete" data-k="'+escapeHtml(k)+'">&times;</button>' : '';
      html += '<div class="chat-msg '+cls+'">'+del+'<div class="chat-msg-author '+colCls+'">'+escapeHtml(msg.author)+badge+'</div><div class="chat-msg-text">'+escapeHtml(msg.text)+'</div><div class="chat-msg-time">'+formatTime(msg.time)+'</div></div>';
    });
    c.innerHTML = html; c.scrollTop = c.scrollHeight;
    if (isAdmin) {
      c.querySelectorAll('.chat-msg-delete').forEach(function(b) {
        b.addEventListener('click', function(e) { e.stopPropagation(); db.ref(tPath('chats/'+currentMatchId+'/'+this.getAttribute('data-k'))).remove(); });
      });
    }
  }

  function sendMsg() {
    if (!canChat()) { alert('\u041f\u0438\u0441\u0430\u0442\u044c \u0432 \u0447\u0430\u0442\u0435 \u043c\u043e\u0433\u0443\u0442 \u0442\u043e\u043b\u044c\u043a\u043e \u0443\u0447\u0430\u0441\u0442\u043d\u0438\u043a\u0438 \u044d\u0442\u043e\u0433\u043e \u043c\u0430\u0442\u0447\u0430!'); return; }
    var inp = document.getElementById('commonChatInput'); var text = inp.value.trim(); if (!text) return;
    var side = 'left'; var m = allMatches[currentMatchId]||{};
    if (myNick && m.right && m.right.indexOf(myNick)!==-1) side = 'right';
    db.ref(tPath('chats/'+currentMatchId)).push({ author: myNick||'\u0413\u043e\u0441\u0442\u044c', text: text, time: Date.now(), isAdmin: isAdmin, side: side });
    inp.value = '';
  }

  function closeModal() {
    document.getElementById('teamModalOverlay').classList.remove('active'); currentMatchId = null;
    if (chatRef) { chatRef.off(); chatRef = null; }
    var ow = document.querySelector('.admin-winner-row'); if (ow) ow.remove();
    var ps = document.querySelector('.prize-winner-section'); if (ps) ps.remove();
  }

  document.getElementById('teamModalClose').addEventListener('click', closeModal);
  document.getElementById('teamModalOverlay').addEventListener('click', function(e) { if (e.target === this) closeModal(); });
  document.addEventListener('keydown', function(e) { if (e.key === 'Escape') { closeModal(); closePrizePopup(); } });
  document.getElementById('commonChatSend').addEventListener('click', function(e) { e.stopPropagation(); sendMsg(); });
  document.getElementById('commonChatInput').addEventListener('keydown', function(e) { if (e.key === 'Enter') { e.stopPropagation(); sendMsg(); } });
  document.getElementById('prizePopup').addEventListener('click', function(e) { if (e.target === this) closePrizePopup(); });

  var matchesListener = null;
  function listenToMatches() {
    if (matchesListener) { matchesListener.off(); }
    if (!currentTournamentId) return;
    matchesListener = db.ref(tPath('matches'));
    matchesListener.on('value', function(snap) {
      allMatches = snap.val() || {};
      matchOrder.forEach(function(mid) { var m = allMatches[mid] || {}; syncMap(mid, m); });
      renderBracket(); updateJoinSection();
      if (currentMatchId && allMatches[currentMatchId]) {
        var m = allMatches[currentMatchId];
        var btnStart = document.getElementById('btnStart');
        var btnConnect = document.getElementById('btnConnect');
        if (btnStart && btnConnect) {
          if (m.started) {
            btnStart.classList.add('started'); btnStart.textContent = 'Started';
            btnConnect.classList.add('visible');
          } else {
            btnStart.classList.remove('started'); btnStart.textContent = 'Start';
            btnConnect.classList.remove('visible');
          }
        }
      }
    });
    db.ref(tPath('prizesEnabled')).on('value', function(snap) {
      prizesEnabled = snap.val() === true;
      if (currentMatchId === 'final') {
        var m = allMatches[currentMatchId] || {};
        var left = m.left || [], right = m.right || [];
        document.getElementById('leftParticipantList').innerHTML = buildParticipantList(currentMatchId,'left',left);
        document.getElementById('rightParticipantList').innerHTML = buildParticipantList(currentMatchId,'right',right);
        renderPrizeToggleBtn();
        renderPrizeAssigned(currentMatchId);
      }
    });
  }

  checkAdminNicks();
  loadTournaments(function(sorted) {
    if (sorted.length > 0) {
      currentTournamentId = sorted[0].id;
      var name = (sorted[0].data && sorted[0].data.name) || sorted[0].id;
      populateTournamentSelect(currentTournamentId);
      document.getElementById('tournamentTitle').textContent = name;
      document.getElementById('tournamentWrapper').style.display = '';
      document.getElementById('noTournamentMsg').style.display = 'none';
      buildBracketHtml();
      attachMatchListeners();
      loadAllMatches(function() {
        matchOrder.forEach(function(mid) { var m = allMatches[mid] || {}; syncMap(mid, m); });
        renderBracket(); updateJoinSection();
      });
      listenToMatches();
      loadPrizesEnabled();
    } else {
      populateTournamentSelect(null);
    }
  });

  updateJoinSection();
})();
</script>
</body>
</html>
