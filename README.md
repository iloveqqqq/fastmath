<!doctype html>
<html lang="vi">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>App Tính Toán Nhanh — (Practice 10s / Challenge 5s)</title>
<style>
:root{
  --bg:#041526; --panel:#0b2330; --glass: rgba(255,255,255,0.03);
  --accent1:#6be7d6; --accent2:#7dd3fc; --gold:#ffd166; --ok:#7ee787; --bad:#ff7b7b;
  --muted: #9fb3b6; --text:#e6f0f2;
  font-family: Inter, "Segoe UI", Roboto, Arial, sans-serif;
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;background:linear-gradient(180deg,#041526 0%, #07283a 60%);color:var(--text);-webkit-font-smoothing:antialiased}
.app{max-width:1220px;margin:28px auto;padding:26px;display:grid;grid-template-columns:400px 1fr;gap:24px;align-items:start}

/* SIDEBAR */
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.04);border-radius:14px;padding:18px;box-shadow:0 12px 36px rgba(0,0,0,0.55)}
.brand{display:flex;gap:14px;align-items:center}
.logo{width:78px;height:78px;border-radius:14px;background:linear-gradient(135deg,var(--gold),#ffb4a2);display:flex;align-items:center;justify-content:center;color:#062024;font-weight:900;font-size:22px;box-shadow:0 10px 28px rgba(0,0,0,0.5)}
h1{margin:0;font-size:20px}
.lead{color:var(--muted);font-size:13px;margin-top:8px}

.stats{display:flex;gap:14px;margin-top:18px}
.stat{flex:1;padding:14px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));text-align:center}
.stat .num{font-size:22px;font-weight:900}
.stat .lbl{font-size:13px;color:var(--muted);margin-top:8px}

.record{margin-top:16px;padding:14px;border-radius:12px;background:linear-gradient(90deg, rgba(255,241,179,0.03), rgba(255,255,255,0.01));display:flex;align-items:center;gap:12px}
.record .big{font-size:22px;font-weight:900;color:#ffe8a6}
.record .small{font-size:13px;color:var(--muted)}

.ranks{margin-top:14px;display:grid;grid-template-columns:1fr 1fr;gap:10px;font-size:13px;color:var(--muted)}
.rank{padding:10px;border-radius:10px;background:linear-gradient(90deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));}

/* controls */
.controls{display:flex;gap:12px;margin-top:16px}
.btn{padding:12px 16px;border-radius:12px;border:none;background:linear-gradient(90deg,#00b4d8,#0077b6);color:white;font-weight:800;cursor:pointer;font-size:14px}
.btn.warn{background:linear-gradient(90deg,#ffb703,#fb5607)}
.btn.ghost{background:transparent;border:1px solid rgba(255,255,255,0.05);color:var(--text)}

/* main area */
.main{display:flex;flex-direction:column;gap:18px}
.board{display:grid;grid-template-columns:1fr 380px;gap:18px}

/* question card */
.question-card{padding:18px;border-radius:14px;display:flex;flex-direction:column;gap:12px;align-items:stretch}
.meta-row{display:flex;gap:12px;align-items:center;justify-content:space-between;flex-wrap:nowrap}
.meta-left{display:flex;gap:12px;align-items:center;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.difficultyTag{padding:8px 14px;border-radius:10px;background:linear-gradient(90deg,var(--accent1),var(--accent2));font-weight:900;color:#002533;min-width:94px;text-align:center;font-size:14px}
.descOps{color:var(--muted);font-size:14px}
.meta-right{color:var(--muted);font-size:14px;text-align:right}

/* single-line "Câu đúng liên tiếp: 0" */
.streakLine{font-size:14px;color:var(--muted);}

/* question area */
.question{font-size:72px;font-weight:900;color:#fff;text-align:center;margin:8px 0}
.hint-line{color:var(--muted);font-size:13px;margin-top:6px}

/* answer & timer row */
.row{display:flex;gap:12px;align-items:center}
.inputBox{flex:1;display:flex;gap:12px;align-items:center}
input[type="number"]{flex:1;padding:16px;border-radius:14px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text);font-size:20px;outline:none}
.submit{padding:14px 22px;border-radius:14px;border:none;background:linear-gradient(90deg,#7ef0a6,#00c2a8);font-weight:900;color:#043027;cursor:pointer;min-width:92px;display:inline-flex;align-items:center;justify-content:center;box-shadow:0 10px 22px rgba(2,30,51,0.35);font-size:16px}

/* circular timer */
.timerWrap{width:96px;height:96px;border-radius:50%;display:flex;align-items:center;justify-content:center;background:linear-gradient(180deg, rgba(255,255,255,0.012), rgba(255,255,255,0.006));border:1px solid rgba(255,255,255,0.03);position:relative}
.timerSvg{position:absolute;left:0;top:0;width:100%;height:100%}
.timerNumber{font-weight:900;font-size:22px;color:var(--gold);z-index:2}

/* progress bar */
.progress{height:12px;background:rgba(255,255,255,0.03);border-radius:10px;overflow:hidden;width:100%}
.progress > i{display:block;height:100%;background:linear-gradient(90deg,var(--gold),#ffb4a2);}

/* feedback */
.feedback{height:36px;font-weight:900;text-align:center;margin-top:8px;font-size:16px}

/* right card */
.right-card{padding:16px;border-radius:14px}
.history{max-height:520px;overflow:auto;margin-top:8px;padding-right:6px}
.history .row{display:flex;justify-content:space-between;padding:10px;border-bottom:1px dashed rgba(255,255,255,0.03);font-size:15px;color:var(--muted)}

/* leaderboard */
.leader{display:flex;flex-direction:column;gap:10px;margin-top:10px}
.leader .item{display:flex;justify-content:space-between;padding:8px;border-radius:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));font-size:13px}

/* bottom round score */
.roundScore{margin-top:12px;font-size:15px;color:var(--muted);font-weight:700}

/* responsive */
@media (max-width:1100px){
  .app{grid-template-columns:1fr;padding:14px}
  .board{grid-template-columns:1fr}
  .question{font-size:48px}
  .timerWrap{width:78px;height:78px}
}
</style>
</head>
<body>
<div class="app">
  <!-- LEFT -->
  <div class="card">
    <div class="brand">
      <div class="logo">TT</div>
      <div>
        <h1>App Tính Toán Nhanh</h1>
        <div class="lead">Mỗi câu đúng +10 điểm — chế độ Luyện tập / Thử thách.</div>
      </div>
    </div>

    <div class="stats">
      <div class="stat"><div class="num" id="score">0</div><div class="lbl">Điểm hiện tại</div></div>
      <div class="stat"><div class="num" id="qcount">0</div><div class="lbl">Đã trả lời</div></div>
    </div>

    <div class="record">
      <div>
        <div class="small">Kỉ lục</div>
        <div class="big" id="record">0</div>
      </div>
      <div style="margin-left:auto;text-align:right">
        <div class="small">Xếp loại</div>
        <div id="rankLabel" style="font-weight:900">GÀ</div>
      </div>
    </div>

    <div class="ranks">
      <div class="rank"><b>0 – 49</b> GÀ</div>
      <div class="rank"><b>50 – 99</b> CÒN NON</div>
      <div class="rank"><b>100 – 199</b> KHÁ LẮM</div>
      <div class="rank"><b>200 – 299</b> ĐẲNG CẤP</div>
      <div class="rank"><b>300 – 499</b> PHI THƯỜNG</div>
      <div class="rank"><b>500+</b> KHÔNG PHẢI NGƯỜI</div>
    </div>

    <div style="margin-top:14px" class="controls">
      <button class="btn" id="modePractice">Luyện tập</button>
      <button class="btn warn" id="modeChallenge">Thử thách</button>
    </div>

    <div style="margin-top:12px" class="controls">
      <button class="btn" id="newBtn">Câu mới</button>
      <button class="btn ghost" id="resetBtn">Đặt lại</button>
    </div>

    <div class="leader small">
      <div style="margin-top:12px;color:var(--muted)">Bảng kỉ lục (local)</div>
      <div id="leaderboard"></div>
    </div>

    <div class="roundScore small" id="leftRoundScore">Điểm vòng hiện tại: 0</div>

    <div class="tips small" style="margin-top:10px">
      <div>• Thử thách: 5s / câu — Luyện tập: 10s / câu. Sai hiển thị đáp án 1s rồi reset (trừ kỉ lục).</div>
    </div>
  </div>

  <!-- MAIN -->
  <div class="main">
    <div class="board">
      <div class="question-card card">
        <div class="meta-row">
          <div class="meta-left">
            <div class="difficultyTag" id="difficultyTag">Mức 1</div>
            <div class="descOps" id="descOps">Phạm vi & kiểu phép: Cộng, Trừ</div>
          </div>
          <div class="meta-right">
            <div class="streakLine">Câu đúng liên tiếp: <span id="streak" style="font-weight:900">0</span></div>
          </div>
        </div>

        <div class="question" id="question">—</div>

        <div class="row">
          <div class="inputBox">
            <input id="answer" type="number" placeholder="Nhập đáp án" />
            <button class="submit" id="submitBtn">OK</button>
          </div>

          <div class="timerWrap" title="Thời gian còn lại">
            <svg class="timerSvg" viewBox="0 0 100 100">
              <defs><linearGradient id="g1" x1="0" x2="1"><stop offset="0" stop-color="#ffd166"/><stop offset="1" stop-color="#ff8a66"/></linearGradient></defs>
              <circle cx="50" cy="50" r="44" stroke="rgba(255,255,255,0.05)" stroke-width="10" fill="none"></circle>
              <circle id="timerCircle" cx="50" cy="50" r="44" stroke="url(#g1)" stroke-width="10" fill="none" stroke-dasharray="276.46" stroke-dashoffset="0" transform="rotate(-90 50 50)"></circle>
            </svg>
            <div class="timerNumber" id="timerNumber">-</div>
          </div>
        </div>

        <div class="progress" aria-hidden><i id="progressBar" style="width:0%"></i></div>
        <div class="hint-line" id="hintLine">Độ khó tăng sau mỗi 10 câu. Mỗi câu đúng +10 điểm.</div>

        <div id="feedback" class="feedback"></div>

        <div class="roundScore" id="roundScoreCenter">Điểm vòng hiện tại: 0</div>
      </div>

      <div class="right-card card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div><div class="small">Lịch sử (gần nhất)</div><div style="font-weight:900">Kết quả</div></div>
          <div style="text-align:right"><div class="small">Độ khó hiện tại</div><div id="level" style="font-weight:900">1</div></div>
        </div>

        <div class="history" id="history"></div>

        <div style="margin-top:12px;color:var(--muted);font-size:13px">
          <label style="display:flex;gap:8px;align-items:center"><input id="autoNext" type="checkbox" /> <span style="color:var(--muted)">Tự chuyển nếu đúng</span></label>
          <label style="display:flex;gap:8px;align-items:center;margin-top:8px"><input id="showAnswer" type="checkbox" /> <span style="color:var(--muted)">Hiện đáp án khi sai</span></label>
        </div>

        <div style="margin-top:12px;color:var(--muted);font-size:13px">* Lưu local; lịch sử sẽ được xóa khi bắt đầu vòng mới.</div>
      </div>
    </div>

    <div style="display:flex;gap:12px;align-items:center">
      <div style="color:var(--muted);font-size:13px">Mẹo:</div>
      <div style="background:linear-gradient(90deg,#063b3b,#084b64);padding:8px 12px;border-radius:8px;color:#dff6f6;font-weight:900">Giữ tốc độ — Đừng hoảng!</div>
    </div>
  </div>
</div>

<!-- JS -->
<script>
/* COMPLETE app:
 - Practice: 10s / câu
 - Challenge: 5s / câu
 - Wrong or timeout: show answer 1s -> play wrong sound -> reset all (except highscore)
 - Suspense ticks while timer running
 - Round blocks and history cleared appropriately
*/

// CONFIG
const BASE_BLOCK = 10;
const SCORE_PER = 10;
const CHALLENGE_TIME = 5;
const PRACTICE_TIME = 10;
const SHOW_ANSWER_MS = 1000;

// STATE
let score = 0;
let record = Number(localStorage.getItem('tt_record') || 0);
let leaderboard = JSON.parse(localStorage.getItem('tt_leaderboard') || '[]');
let qcount = 0;       // number of answered questions in current session (resets on fail)
let streak = 0;
let roundScore = 0;
let history = [];
let mode = 'challenge'; // default
let currentQuestion = null;
let timerInterval = null;
let timeLeft = 0;
let locked = false;
let suspenseInterval = null;

// ELEMENTS
const $ = id => document.getElementById(id);
const els = {
  score: $('score'), record: $('record'), qcount: $('qcount'), question: $('question'),
  answer: $('answer'), submitBtn: $('submitBtn'), feedback: $('feedback'),
  progressBar: $('progressBar'), difficultyTag: $('difficultyTag'), descOps: $('descOps'),
  level: $('level'), streak: $('streak'), timerNumber: $('timerNumber'), timerCircle: $('timerCircle'),
  history: $('history'), leaderboard: $('leaderboard'), hintLine: $('hintLine'),
  autoNext: $('autoNext'), showAnswer: $('showAnswer'), roundScoreCenter: $('roundScoreCenter'),
  leftRoundScore: $('leftRoundScore')
};

// AUDIO (WebAudio)
const AudioCtx = window.AudioContext || window.webkitAudioContext;
const audioCtx = new AudioCtx();
function playTone(freq, type='sine', dur=0.08, vol=0.07){
  const o = audioCtx.createOscillator();
  const g = audioCtx.createGain();
  o.type = type; o.frequency.value = freq;
  g.gain.value = vol;
  o.connect(g); g.connect(audioCtx.destination);
  o.start();
  setTimeout(()=> o.stop(), dur*1000);
}
function soundCorrect(){ playTone(880,'sine',0.12,0.09); setTimeout(()=> playTone(1320,'sine',0.09,0.06), 90); }
function soundWrong(){ playTone(220,'sawtooth',0.22,0.12); setTimeout(()=> playTone(160,'sawtooth',0.16,0.1), 140); }
function startSuspense(totalSec){
  stopSuspense();
  let ticks = 0;
  suspenseInterval = setInterval(()=>{
    ticks++;
    const rem = totalSec - ticks;
    const base = 420;
    const freq = base + ( (totalSec - rem) * 140 );
    playTone(freq, 'square', 0.05, 0.05);
    if(rem <= 2) setTimeout(()=> playTone(freq+120,'square',0.04,0.04), 70);
    if(ticks >= totalSec) stopSuspense();
  }, 1000);
}
function stopSuspense(){ if(suspenseInterval){ clearInterval(suspenseInterval); suspenseInterval = null; } }

// HELPERS
function clamp(v,a,b){ return Math.max(a,Math.min(b,v)); }
function nowStr(){ return new Date().toLocaleString(); }
function saveRecord(){
  if(score > record){
    record = score;
    localStorage.setItem('tt_record', String(record));
    leaderboard.push({score: score, at: nowStr()});
    leaderboard.sort((a,b)=>b.score - a.score);
    if(leaderboard.length > 10) leaderboard.length = 10;
    localStorage.setItem('tt_leaderboard', JSON.stringify(leaderboard));
    renderLeaderboard();
  }
}

// DIFFICULTY & QUESTION
function difficultyFromCount(c){ return Math.floor(c / BASE_BLOCK) + 1; }
function rand(a,b){ return Math.floor(Math.random()*(b-a+1))+a; }

function generateQuestion(){
  // unlock, clear timers
  locked = false;
  clearTimer();
  stopSuspense();

  // If starting a new round (qcount==0) then history should already be cleared.
  const level = difficultyFromCount(qcount);

  // create a question according to level
  let text = '—', answer = 0, desc = '';
  if(level === 1){
    const a = rand(1,15), b = rand(1,12);
    if(Math.random() < 0.6){ text = `${a} + ${b}`; answer = a + b; desc = 'Cộng, Trừ'; }
    else { text = `${a} × ${b}`; answer = a * b; desc = 'Nhân, Cộng'; }
  } else if(level === 2){
    const a = rand(5,25), b = rand(2,18);
    const p = Math.random();
    if(p < 0.35){ text = `${a} + ${b}`; answer = a + b; }
    else if(p < 0.7){ text = `${a} - ${b}`; answer = a - b; }
    else { text = `${a} × ${b}`; answer = a * b; }
    desc = 'Cộng, Trừ, Nhân';
  } else if(level === 3){
    if(Math.random() < 0.4){
      const divisor = rand(2,15), quotient = rand(2,12);
      const dividend = divisor * quotient;
      text = `${dividend} ÷ ${divisor}`; answer = quotient;
    } else {
      const a = rand(10,60), b = rand(10,60);
      text = `${a} + ${b}`; answer = a + b;
    }
    desc = 'Cộng, Trừ, Nhân, Chia';
  } else if(level === 4){
    const a = rand(10,120), b = rand(2,40), c = rand(2,40);
    if(Math.random() < 0.6){ text = `${a} + ${b} × ${c}`; answer = a + (b * c); }
    else { text = `${a} - ${b} × ${c}`; answer = a - (b * c); }
    desc = 'Phức hợp (nhân trước)';
  } else {
    if(Math.random() < 0.5){
      const divisor = rand(2,30), quotient = rand(2,15);
      const dividend = divisor * quotient;
      const add = rand(0,9);
      text = `${dividend} ÷ ${divisor} + ${add}`; answer = Math.floor(dividend/divisor) + add;
    } else {
      const a = rand(30,200), b = rand(5,60), c = rand(1,30);
      text = `${a} × ${b} - ${c}`; answer = a * b - c;
    }
    desc = 'Nâng cao';
  }

  currentQuestion = {text, answer, level, opsDesc: desc};
  renderQuestion();

  // choose timer length by mode
  const t = (mode === 'practice') ? PRACTICE_TIME : CHALLENGE_TIME;
  startTimer(t);
  startSuspense(t);
}

function renderQuestion(){
  els.question.textContent = currentQuestion.text;
  els.descOps.textContent = `Phạm vi & kiểu phép: ${currentQuestion.opsDesc || 'Cơ bản'}`;
  els.difficultyTag.textContent = `Mức ${currentQuestion.level}`;
  els.answer.value = '';
  els.feedback.textContent = '';
  updateUI();
}

// TIMER
function startTimer(seconds){
  clearTimer();
  timeLeft = seconds;
  updateTimerVisual();
  timerInterval = setInterval(()=>{
    timeLeft--;
    updateTimerVisual();
    if(timeLeft <= 0){
      clearTimer();
      stopSuspense();
      handleTimeout();
    }
  }, 1000);
}
function clearTimer(){ if(timerInterval){ clearInterval(timerInterval); timerInterval = null; } }
function updateTimerVisual(){
  els.timerNumber.textContent = String(clamp(timeLeft,0,999));
  const full = 276.46;
  const pct = (timeLeft / ((mode==='practice')?PRACTICE_TIME:CHALLENGE_TIME));
  const offset = full * (1 - clamp(pct,0,1));
  els.timerCircle.style.strokeDashoffset = String(offset);
  const col = pct > 0.6 ? '#ffd166' : (pct > 0.3 ? '#ff8a66' : '#ff6b6b');
  els.timerNumber.style.color = col;
}

// SUBMIT & FLOW
function submitAnswer(){
  if(locked) return;
  if(audioCtx.state === 'suspended') audioCtx.resume();

  const raw = els.answer.value.trim();
  if(raw === '') return;
  const userVal = Number(raw);

  locked = true;
  clearTimer();
  stopSuspense();

  const correct = Number(currentQuestion.answer);

  if(Number.isFinite(userVal) && Math.abs(userVal - correct) < 1e-9){
    // correct
    score += SCORE_PER;
    roundScore += SCORE_PER;
    streak++;
    qcount++;
    history.unshift({txt: `${currentQuestion.text} = ${correct}`, ok:true, at: nowStr()});
    soundCorrect();
    els.feedback.style.color = 'var(--ok)';
    els.feedback.textContent = `Đúng! +${SCORE_PER} điểm`;
    saveRecord();
    renderHistory();
    updateUI();

    // new question quickly
    setTimeout(()=>{ locked = false; generateQuestion(); }, 350);
  } else {
    // wrong -> show answer 1s, then full reset (keep record)
    history.unshift({txt: `${currentQuestion.text} -> bạn: ${raw} (đáp: ${correct})`, ok:false, at: nowStr()});
    soundWrong();
    els.feedback.style.color = 'var(--bad)';
    els.feedback.textContent = `Sai — đáp án: ${correct}`;
    renderHistory();
    updateUI();

    setTimeout(()=>performFullResetAfterFail(), SHOW_ANSWER_MS);
  }
}

function handleTimeout(){
  if(locked) return;
  locked = true;
  const correct = Number(currentQuestion.answer);
  history.unshift({txt: `${currentQuestion.text} -> Hết giờ (đáp: ${correct})`, ok:false, at: nowStr()});
  soundWrong();
  els.feedback.style.color = 'var(--bad)';
  els.feedback.textContent = `Hết thời gian! Đáp án: ${correct}`;
  renderHistory();
  updateUI();

  setTimeout(()=>performFullResetAfterFail(), SHOW_ANSWER_MS);
}

// FULL RESET AFTER FAIL (KEEP record)
function performFullResetAfterFail(){
  // keep record & leaderboard
  const savedRecord = record;
  const savedLeaderboard = leaderboard ? Array.from(leaderboard) : [];

  // reset state
  score = 0;
  qcount = 0;
  streak = 0;
  roundScore = 0;
  history = [];

  clearTimer();
  stopSuspense();
  locked = false;

  // restore record/leaderboard display
  record = savedRecord;
  leaderboard = savedLeaderboard;
  $('record').textContent = record;
  renderLeaderboard();
  renderHistory();
  updateUI();

  // fresh question
  generateQuestion();
}

// UI updates
function updateUI(){
  els.score.textContent = score;
  els.record.textContent = record;
  els.qcount.textContent = qcount;
  els.streak.textContent = streak;
  els.level.textContent = difficultyFromCount(qcount);
  const pct = Math.round((qcount % BASE_BLOCK) / BASE_BLOCK * 100);
  els.progressBar.style.width = `${pct}%`;
  els.hintLine.textContent = `Độ khó tăng sau mỗi ${BASE_BLOCK} câu. Mỗi câu đúng +${SCORE_PER} điểm. (Chế độ: ${mode === 'challenge' ? 'Thử thách' : 'Luyện tập'})`;
  els.roundScoreCenter.textContent = `Điểm vòng hiện tại: ${roundScore}`;
  els.leftRoundScore.textContent = `Điểm vòng hiện tại: ${roundScore}`;
}

// History & leaderboard
function renderHistory(){
  if(!history.length){ els.history.innerHTML = `<div class="small" style="color:var(--muted)">Lịch sử trống cho vòng này.</div>`; return; }
  els.history.innerHTML = history.slice(0,200).map(h=>`<div class="row"><div style="max-width:72%">${h.txt}</div><div style="color:${h.ok? 'var(--ok)':'var(--bad)'}">${h.ok? '+10':'×'}</div></div>`).join('');
}
function renderLeaderboard(){
  if(!leaderboard || !leaderboard.length){ els.leaderboard.innerHTML = `<div class="small" style="color:var(--muted)">Chưa có kỉ lục</div>`; return; }
  els.leaderboard.innerHTML = leaderboard.map((it,idx)=>`<div class="item"><div>#${idx+1} — ${it.at}</div><div style="font-weight:900">${it.score}</div></div>`).join('');
}

// Mode & controls
function setMode(m){
  mode = m;
  $('modePractice').style.opacity = m === 'practice' ? '1' : '0.8';
  $('modeChallenge').style.opacity = m === 'challenge' ? '1' : '0.8';
  clearTimer();
  stopSuspense();
  // prepare timer display
  els.timerNumber.textContent = (mode==='practice') ? String(PRACTICE_TIME) : String(CHALLENGE_TIME);
  // reset session state (but KEEP record/leaderboard)
  score = 0; qcount = 0; streak = 0; roundScore = 0; history = [];
  renderHistory();
  updateUI();
  generateQuestion();
}

// Events
els.submitBtn.addEventListener('click', submitAnswer);
els.answer.addEventListener('keydown', (e)=>{ if(e.key === 'Enter') submitAnswer(); });
$('newBtn').addEventListener('click', ()=>{ if(!locked){ qcount++; generateQuestion(); }});
$('resetBtn').addEventListener('click', ()=>{
  if(confirm('Bạn có chắc muốn đặt lại điểm và kỉ lục (local)?')){
    score = 0; qcount = 0; streak = 0; roundScore = 0; history = []; record = 0; leaderboard = [];
    localStorage.removeItem('tt_record'); localStorage.removeItem('tt_leaderboard');
    renderHistory(); renderLeaderboard(); updateUI(); generateQuestion();
  }
});
$('modePractice').addEventListener('click', ()=> setMode('practice'));
$('modeChallenge').addEventListener('click', ()=> setMode('challenge'));

// INIT
$('record').textContent = record;
renderLeaderboard();
setMode('challenge'); // start default in challenge mode
renderHistory();
updateUI();

// resume audio on first user gesture
document.addEventListener('click', ()=>{ if(audioCtx.state === 'suspended') audioCtx.resume(); }, {once:true});
</script>
</body>
</html>
