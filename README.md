<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Плей-офф турнира с перемешиванием</title>
<style>
@import url('https://googleapis.com');

body { margin: 0; padding: 0; background: #0f111a; font-family: 'Inter', sans-serif; padding: 20px; color: #fff; }

.tournament-wrapper {
  background-image: url('https://moy.su');
  background-size: cover; background-position: center center; background-repeat: no-repeat;
  padding: 30px; border-radius: 12px; position: relative; overflow: hidden;
}
.tournament-wrapper::before {
  content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.7); z-index: 0;
}
.tournament-wrapper > * { position: relative; z-index: 1; }

.tournament-title {
  text-align: center; color: #fff; font-size: 28px; font-weight: 800;
  text-transform: uppercase; letter-spacing: 2px; margin-bottom: 24px;
  text-shadow: 0 2px 8px rgba(0,0,0,0.8);
}

.bracket-row { display: flex; gap: 16px; justify-content: center; align-items: stretch; flex-wrap: wrap; }
.bracket-col { flex: 1; min-width: 200px; max-width: 22%; display: flex; flex-direction: column; gap: 20px; }
.bracket-col.final-col { max-width: 260px; justify-content: center; }
.bracket-col.semifinal-col { justify-content: center; gap: 80px; }

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
  border-radius: 6px; overflow: hidden; width: 100%;
  border: 1px solid rgba(255,255,255,0.08); box-sizing: border-box;
  cursor: pointer; transition: border-color 0.2s, background 0.2s, transform 0.15s;
}
.match:hover { border-color: rgba(255,215,0,0.4); background: rgba(255,255,255,0.1); transform: translateY(-2px); }

.match-row {
  display: flex; justify-content: space-between; align-items: center;
  padding: 10px 12px; color: #fff; font-size: 13px; font-weight: 500; transition: background 0.2s;
}
.match-row.winner { background: rgba(76,175,80,0.25); font-weight: 700; }

.match-row.winner::after { 
  content: ''; display: inline-block; width: 18px; height: 18px; 
  background-image: url('https://moy.su'); 
  background-size: cover; background-position: center; border-radius: 4px; 
  margin-left: 6px; flex-shrink: 0;
}

.match-team { display: flex; flex-direction: column; gap: 2px; }
.team-name { font-weight: 600; }
.team-players-preview { font-size: 10px; color: rgba(255,255,255,0.4); font-weight: 400; max-width: 140px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.team-seed {
  background: rgba(255,255,255,0.12); border-radius: 3px; padding: 2px 5px;
  font-size: 11px; font-weight: 600; color: rgba(255,255,255,0.7); margin-right: 6px;
}
.match-score { font-weight: 700; font-size: 14px; min-width: 22px; text-align: center; }
.match-divider { height: 1px; background: rgba(255,255,255,0.08); }

.final-box {
  background: rgba(0,0,0,0.6); border-radius: 12px; padding: 20px;
  border: 2px solid rgba(255,215,0,0.4); text-align: center; width: 100%; box-sizing: border-box;
}
.final-title {
  color: #ffd700; font-size: 18px; font-weight: 800; text-transform: uppercase;
  letter-spacing: 1.5px; margin-bottom: 14px; text-shadow: 0 2px 8px rgba(0,0,0,0.8);
  display: flex; align-items: center; justify-content: center; gap: 8px;
}
.final-title::before, .final-title::after {
  content: ''; display: inline-block; width: 22px; height: 22px;
  background-image: url('https://moy.su'); background-size: cover; border-radius: 4px;
}
.final-match { background: rgba(255,215,0,0.05); border: 1px solid rgba(255,215,0,0.2); }

.admin-panel {
  max-width: 850px; margin: 20px auto 0; padding: 22px;
  background: rgba(10,42,107,0.3); backdrop-filter: blur(8px);
  border: 1px solid rgba(74,158,255,0.35); border-radius: 14px;
}
.admin-panel h3 { font-size: 18px; margin: 0 0 8px 0; color: #4a9eff; font-weight: 700; }

.team-modal-overlay {
  display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.8); z-index: 9999; justify-content: center; align-items: center;
}
.team-modal-overlay.active { display: flex; }
.team-modal {
  background: #141622; border: 1px solid rgba(255,255,255,0.15); border-radius: 14px;
  padding: 24px; width: 550px; max-width: 92vw; box-shadow: 0 12px 40px rgba(0,0,0,0.6);
}
.team-modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.team-modal-matchup { font-size: 18px; font-weight: 800; color: #fff; }
.team-modal-close { background: rgba(255,255,255,0.1); border: none; color: #fff; font-size: 22px; cursor: pointer; width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center; }

.modal-teams-dev { display: flex; gap: 20px; margin-bottom: 20px; }
.modal-team-block { flex: 1; background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); padding: 12px; border-radius: 8px; }
.modal-team-title { font-weight: 700; margin-bottom: 8px; color: #ffd700; font-size: 14px; }
.players-list { font-size: 12px; color: rgba(255,255,255,0.7); margin: 0; padding-left: 20px; }

.btn {
  background: #1a4a8b; color: white; border: none; padding: 8px 16px; border-radius: 6px;
  font-weight: 600; cursor: pointer; font-size: 13px; transition: background 0.2s; width: 100%; margin-top: 6px;
}
.btn:hover { background: #2a6abb; }
.btn-win { background: #27ae60; margin-top: 10px; font-size: 14px; padding: 10px; }
.btn-win:hover { background: #2ecc71; }
</style>
</head>
<body>

<div class="tournament-wrapper">
  <div class="tournament-title">Плей-офф турнира</div>
  
  <div class="bracket-row">
    <!-- ЗАПАД ЧЕТВЕРТЬФИНАЛЫ -->
    <div class="bracket-col">
      <div class="conf-label west">Запад</div>
      <div class="round-label">Четвертьфиналы</div>
      
      <div class="match" onclick="openMatch('q1')">
        <div class="match-row" id="q1-t1-row">
          <div class="match-team">
            <span class="team-name"><span class="team-seed">1</span>Команда А</span>
            <span class="team-players-preview" id="q1-t1-preview">Нажмите для настройки</span>
          </div>
          <div class="match-score" id="q1-t1-score">-</div>
        </div>
        <div class="match-divider"></div>
        <div class="match-row" id="q1-t2-row">
          <div class="match-team">
            <span class="team-name"><span class="team-seed">4</span>Команда Б</span>
            <span class="team-players-preview" id="q1-t2-preview">Нажмите для настройки</span>
          </div>
          <div class="match-score" id="q1-t2-score">-</div>
        </div>
      </div>

      <div class="match" onclick="openMatch('q2')">
        <div class="match-row" id="q2-t1-row">
          <div class="match-team">
            <span class="team-name"><span class="team-seed">2</span>Команда В</span>
            <span class="team-players-preview" id="q2-t1-preview">Нажмите для настройки</span>
          </div>
          <div class="match-score" id="q2-t1-score">-</div>
        </div>
        <div class="match-divider"></div>
        <div class="match-row" id="q2-t2-row">
          <div class="match-team">
            <span class="team-name"><span class="team-seed">3</span>Команда Г</span>
            <span class="team-players-preview" id="q2-t2-preview">Нажмите для настройки</span>
          </div>
          <div class="match-score" id="q2-t2-score">-</div>
        </div>
      </div>
    </div>

    <!-- ЗАПАД ПОЛУФИНАЛ -->
    <div class="bracket-col semifinal-col">
      <div class="round-label">Полуфинал 1</div>
      <div class="match" onclick="openMatch('sf1')">
        <div class="match-row" id="sf1-t1-row">
          <div class="match-team">
            <span class="team-name" id="sf1-t1-name">Слот Полуфинала 1-А</span>
            <span class="team-players-preview" id="sf1-t1-preview">Ожидание игроков...</span>
          </div>
          <div class="match-score" id="sf1-t1-score">-</div>
        </div>
        <div class="match-divider"></div>
        <div class="match-row" id="sf1-t2-row">
          <div class="match-team">
            <span class="team-name" id="sf1-t2-name">Слот Полуфинала 1-Б</span>
            <span class="team-players-preview" id="sf1-t2-preview">Ожидание игроков...</span>
          </div>
          <div class="match-score" id="sf1-t2-score">-</div>
        </div>
      </div>
    </div>

    <!-- ГРАНД ФИНАЛ -->
    <div class="bracket-col final-col">
      <div class="final-box">
        <div class="final-title">Финал</div>
        <div class="match final-match" onclick="openMatch('f1')">
          <div class="match-row" id="f1-t1-row">
            <div class="match-team">
              <span class="team-name" id="f1-t1-name">Финалист 1</span>
              <span class="team-players-preview" id="f1-t1-preview">Ожидание игроков...</span>
            </div>
            <div class="match-score" id="f1-t1-score">-</div>
          </div>
          <div class="match-divider"></div>
          <div class="match-row" id="f1-t2-row">
            <div class="match-team">
              <span class="team-name" id="f1-t2-name">Финалист 2</span>
              <span class="team-players-preview" id="f1-t2-preview">Ожидание игроков...</span>
            </div>
            <div class="match-score" id="f1-t2-score">-</div>
          </div>
        </div>
      </div>
    </div>

