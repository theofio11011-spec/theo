<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>QuizMaster</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg:#07071a;
  --bg2:#0d0d24;
  --bg3:#12122e;
  --surface:#161630;
  --surface2:#1c1c3a;
  --border:rgba(255,255,255,0.09);
  --border2:rgba(255,255,255,0.18);
  --text:#f0f0ff;
  --muted:#8888bb;
  --accent:#7c6df0;
  --accent2:#a89bf5;
  --accent-glow:rgba(124,109,240,0.3);
  --green:#00e5a0;
  --green-bg:rgba(0,229,160,0.12);
  --red:#ff5c7a;
  --red-bg:rgba(255,92,122,0.12);
  --gold:#ffd166;
  --gold-bg:rgba(255,209,102,0.12);
  --cat-sci:#00c97a;
  --cat-math:#ff9f43;
  --cat-lang:#7c6df0;
  --cat-anim:#ff5c7a;
  --font:'Nunito',sans-serif;
  --mono:'DM Mono',monospace;
  --r:18px; --rsm:10px;
}

html,body { min-height:100vh; background:var(--bg); color:var(--text); font-family:var(--font); overflow-x:hidden; }

/* ── STARS BACKGROUND ── */
body::before {
  content:''; position:fixed; inset:0; pointer-events:none; z-index:0;
  background-image:
    radial-gradient(1px 1px at 10% 15%, rgba(255,255,255,.5) 0%, transparent 100%),
    radial-gradient(1px 1px at 20% 80%, rgba(255,255,255,.4) 0%, transparent 100%),
    radial-gradient(1px 1px at 35% 30%, rgba(255,255,255,.3) 0%, transparent 100%),
    radial-gradient(1px 1px at 50% 60%, rgba(255,255,255,.5) 0%, transparent 100%),
    radial-gradient(1px 1px at 70% 20%, rgba(255,255,255,.4) 0%, transparent 100%),
    radial-gradient(1px 1px at 80% 70%, rgba(255,255,255,.3) 0%, transparent 100%),
    radial-gradient(1px 1px at 90% 40%, rgba(255,255,255,.5) 0%, transparent 100%),
    radial-gradient(1px 1px at 5% 50%, rgba(255,255,255,.4) 0%, transparent 100%),
    radial-gradient(1px 1px at 60% 85%, rgba(255,255,255,.3) 0%, transparent 100%),
    radial-gradient(1px 1px at 45% 5%, rgba(255,255,255,.5) 0%, transparent 100%),
    radial-gradient(2px 2px at 25% 55%, rgba(255,255,255,.2) 0%, transparent 100%),
    radial-gradient(2px 2px at 75% 10%, rgba(255,255,255,.2) 0%, transparent 100%);
}
body::after {
  content:''; position:fixed; inset:0; pointer-events:none; z-index:0;
  background:radial-gradient(ellipse 80% 50% at 50% 0%, rgba(100,80,240,0.15) 0%, transparent 70%),
             radial-gradient(ellipse 60% 40% at 100% 80%, rgba(124,109,240,0.1) 0%, transparent 70%);
}

/* SCREENS */
.screen { display:none; min-height:100vh; position:relative; z-index:1; }
.screen.active { display:flex; flex-direction:column; }

/* ═══════════════════════════════
   LANDING SCREEN
═══════════════════════════════ */
#screen-landing {
  min-height:100vh;
  display:none;
  flex-direction:column;
  overflow:hidden;
}
#screen-landing.active { display:flex; }

/* NEON LIGHT BEAMS */
.beam {
  position:absolute; pointer-events:none; border-radius:50%;
  filter:blur(80px); animation:beamFloat 10s ease-in-out infinite;
}
.beam-1 { width:500px;height:500px;background:rgba(76,29,240,0.2);top:-150px;left:-100px;animation-delay:0s; }
.beam-2 { width:400px;height:400px;background:rgba(240,50,130,0.15);bottom:-100px;right:-80px;animation-delay:-3.5s; }
.beam-3 { width:300px;height:300px;background:rgba(0,220,160,0.12);top:40%;right:20%;animation-delay:-6s; }
.beam-4 { width:250px;height:250px;background:rgba(255,160,40,0.1);bottom:20%;left:15%;animation-delay:-8s; }
@keyframes beamFloat {
  0%,100%{transform:translate(0,0) scale(1)}
  33%{transform:translate(20px,-20px) scale(1.05)}
  66%{transform:translate(-15px,15px) scale(0.95)}
}

/* DIAGONAL NEON LINES */
.neon-lines {
  position:absolute; inset:0; pointer-events:none; overflow:hidden;
}
.neon-lines::before {
  content:''; position:absolute;
  width:3px; height:200px;
  background:linear-gradient(to bottom, transparent, rgba(124,109,240,.6), transparent);
  top:10%; right:8%;
  transform:rotate(15deg);
  animation:lineFlash 4s ease-in-out infinite;
}
.neon-lines::after {
  content:''; position:absolute;
  width:2px; height:150px;
  background:linear-gradient(to bottom, transparent, rgba(0,229,160,.5), transparent);
  top:20%; left:5%;
  transform:rotate(-20deg);
  animation:lineFlash 4s ease-in-out infinite 2s;
}
@keyframes lineFlash { 0%,100%{opacity:.4} 50%{opacity:1} }

/* LANDING LAYOUT */
.landing-layout {
  flex:1;
  display:grid;
  grid-template-columns:1fr 380px;
  gap:0;
  max-width:1100px;
  margin:0 auto;
  padding:40px 32px;
  align-items:center;
  width:100%;
}

/* LANDING LEFT */
.landing-left {
  position:relative;
  padding-right:40px;
}

.logo-badge-top {
  display:inline-flex; align-items:center; gap:8px;
  background:rgba(124,109,240,0.12); border:1px solid rgba(124,109,240,0.35);
  border-radius:99px; padding:6px 16px;
  font-size:12px; font-family:var(--mono); color:var(--accent2);
  margin-bottom:20px; animation:fadeDown .5s ease both;
}
.star-icon { color:var(--gold); font-size:14px; }

.landing-title {
  font-size:clamp(3.2rem,7vw,5.5rem);
  font-weight:900;
  line-height:1;
  letter-spacing:-.03em;
  margin-bottom:16px;
  animation:fadeDown .5s .08s ease both;
}
.landing-title .quiz { color:white; }
.landing-title .master {
  background:linear-gradient(135deg,#a89bf5,#00e5a0);
  -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text;
}

.landing-desc-big {
  font-size:1.5rem; font-weight:800; color:white;
  margin-bottom:10px; animation:fadeDown .5s .14s ease both;
}
.landing-desc-sub {
  font-size:.95rem; color:var(--muted); line-height:1.7; max-width:380px;
  margin-bottom:28px; animation:fadeDown .5s .2s ease both;
}

.btn-start-big {
  display:inline-flex; align-items:center; gap:10px;
  background:linear-gradient(135deg,#6c5ce7,#00e5a0);
  color:white; border:none; border-radius:14px;
  padding:16px 36px; font-size:1.05rem; font-weight:800;
  font-family:var(--font); cursor:pointer;
  box-shadow:0 8px 40px rgba(108,92,231,0.45);
  transition:transform .15s,box-shadow .15s;
  animation:fadeDown .5s .26s ease both;
}
.btn-start-big:hover { transform:translateY(-3px) scale(1.02); box-shadow:0 14px 50px rgba(108,92,231,0.6); }
.btn-start-big .rocket { font-size:1.2rem; }
.btn-start-big .arr { display:inline-block; transition:transform .2s; margin-left:4px; }
.btn-start-big:hover .arr { transform:translateX(4px); }

.landing-hint { font-size:11px; color:var(--muted); font-family:var(--mono); margin-top:10px; animation:fadeDown .5s .3s ease both; }

/* BRAIN MASCOT AREA */
.brain-area {
  position:absolute; bottom:-20px; right:260px;
  width:220px; animation:brainBob 3s ease-in-out infinite;
  pointer-events:none;
}
@keyframes brainBob { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }

.brain-emoji {
  font-size:140px; line-height:1; display:block;
  filter:drop-shadow(0 0 30px rgba(124,109,240,0.5));
  animation:fadeDown .5s .35s ease both;
}

/* TROPHY AND PAPER PLANE FLOATIES */
.floaty {
  position:absolute; pointer-events:none;
  animation:floatyAnim 5s ease-in-out infinite;
}
.floaty-trophy { font-size:44px; top:10%; right:35%; animation-delay:-2s; }
.floaty-plane  { font-size:36px; top:5%; right:15%; animation-delay:-1s; }
.floaty-star1  { font-size:20px; top:25%; right:10%; animation-delay:-3s; color:var(--gold); }
.floaty-star2  { font-size:14px; bottom:30%; right:30%; animation-delay:-4s; color:var(--gold); }
.floaty-sparkle { font-size:24px; top:60%; left:10%; animation-delay:-.5s; }
@keyframes floatyAnim { 0%,100%{transform:translateY(0) rotate(0deg)} 50%{transform:translateY(-10px) rotate(8deg)} }

/* LANDING STATS BAR */
.landing-stats {
  display:flex; gap:0;
  background:rgba(255,255,255,0.04); border:1px solid var(--border);
  border-radius:var(--r); overflow:hidden;
  margin-top:32px; animation:fadeDown .5s .38s ease both;
}
.ls-item { flex:1; padding:14px 10px; text-align:center; border-right:1px solid var(--border); }
.ls-item:last-child { border-right:none; }
.ls-icon { font-size:20px; margin-bottom:4px; display:block; }
.ls-val { font-size:1.3rem; font-weight:900; }
.ls-lbl { font-size:10px; color:var(--muted); font-family:var(--mono); margin-top:2px; letter-spacing:.04em; }

/* LANDING RIGHT — CATEGORY CARDS */
.landing-right {
  display:grid; grid-template-columns:1fr 1fr; gap:12px;
  animation:fadeRight .6s .2s ease both;
}
@keyframes fadeRight { from{opacity:0;transform:translateX(20px)} to{opacity:1;transform:translateX(0)} }

.lcat-card {
  border-radius:18px; padding:20px 16px;
  cursor:pointer; position:relative; overflow:hidden;
  transition:transform .2s,box-shadow .2s;
  min-height:150px;
  display:flex; flex-direction:column; justify-content:space-between;
}
.lcat-card:hover { transform:translateY(-6px) scale(1.02); }
.lcat-card.sci  { background:linear-gradient(135deg,#0d4a2a,#0a3d22); border:1px solid rgba(0,201,122,.3); }
.lcat-card.math { background:linear-gradient(135deg,#4a2a0d,#3d220a); border:1px solid rgba(255,159,67,.3); }
.lcat-card.lang { background:linear-gradient(135deg,#1e1660,#17104a); border:1px solid rgba(124,109,240,.3); }
.lcat-card.anim { background:linear-gradient(135deg,#4a0d1a,#3d0a14); border:1px solid rgba(255,92,122,.3); }
.lcat-card.sci:hover  { box-shadow:0 12px 40px rgba(0,201,122,.25); }
.lcat-card.math:hover { box-shadow:0 12px 40px rgba(255,159,67,.25); }
.lcat-card.lang:hover { box-shadow:0 12px 40px rgba(124,109,240,.25); }
.lcat-card.anim:hover { box-shadow:0 12px 40px rgba(255,92,122,.25); }

.lcat-icon { font-size:44px; display:block; margin-bottom:10px; }
.lcat-name { font-size:15px; font-weight:900; }
.lcat-name.sci  { color:#00e5a0; }
.lcat-name.math { color:#ff9f43; }
.lcat-name.lang { color:#a89bf5; }
.lcat-name.anim { color:#ff7a95; }
.lcat-desc { font-size:10px; color:rgba(255,255,255,.55); margin-top:3px; font-family:var(--mono); line-height:1.4; }
.lcat-progress { margin-top:12px; }
.lcat-prog-row { display:flex; align-items:center; gap:8px; margin-top:6px; }
.lcat-prog-num { font-size:10px; font-family:var(--mono); color:rgba(255,255,255,.4); }
.lcat-prog-bar { flex:1; height:3px; background:rgba(255,255,255,.1); border-radius:99px; overflow:hidden; }
.lcat-prog-fill { height:100%; border-radius:99px; transition:width .8s cubic-bezier(.4,0,.2,1); }
.lcat-prog-fill.sci  { background:var(--cat-sci); }
.lcat-prog-fill.math { background:var(--cat-math); }
.lcat-prog-fill.lang { background:var(--cat-lang); }
.lcat-prog-fill.anim { background:var(--cat-anim); }

/* CONQUISTAS BUTTON */
.conquistas-btn {
  position:absolute; top:24px; right:32px;
  display:flex; align-items:center; gap:8px;
  background:linear-gradient(135deg,#ff6b9d,#ff4757);
  border:none; border-radius:99px; padding:10px 20px;
  font-size:13px; font-weight:800; font-family:var(--font); color:white;
  cursor:pointer; z-index:10;
  transition:transform .15s,box-shadow .15s;
  box-shadow:0 4px 20px rgba(255,71,87,.35);
  animation:fadeDown .5s .1s ease both;
}
.conquistas-btn:hover { transform:translateY(-2px); box-shadow:0 8px 30px rgba(255,71,87,.5); }

@keyframes fadeDown { from{opacity:0;transform:translateY(-14px)} to{opacity:1;transform:translateY(0)} }

/* ═══════════════════════════════
   APP SCREEN
═══════════════════════════════ */
#screen-app {
  flex-direction:row !important;
  min-height:100vh;
}

/* SIDEBAR */
.sidebar {
  width:220px; flex-shrink:0;
  background:rgba(10,10,30,.9); backdrop-filter:blur(20px);
  border-right:1px solid var(--border);
  display:flex; flex-direction:column;
  padding:20px 0;
  position:sticky; top:0; height:100vh; overflow-y:auto;
}

.sb-brand {
  display:flex; align-items:center; gap:10px;
  padding:0 18px 20px;
  border-bottom:1px solid var(--border);
  margin-bottom:14px;
}
.sb-brand-icon { font-size:24px; }
.sb-brand-text { font-size:16px; font-weight:900; }
.sb-brand-text .quiz { color:white; }
.sb-brand-text .master { background:linear-gradient(135deg,var(--accent2),var(--green)); -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text; }

.sb-section { padding:0 10px; margin-bottom:8px; }
.sb-section-label { font-size:10px; font-family:var(--mono); color:var(--muted); letter-spacing:.1em; padding:0 8px; margin-bottom:6px; }

.sb-item {
  display:flex; align-items:center; gap:10px;
  padding:10px 12px; border-radius:12px;
  cursor:pointer; font-size:14px; font-weight:700;
  color:var(--muted); transition:background .15s,color .15s;
  border:1px solid transparent;
  margin-bottom:2px;
}
.sb-item:hover { background:var(--surface); color:var(--text); }
.sb-item.active-cat.sci  { background:rgba(0,201,122,.12); border-color:rgba(0,201,122,.25); color:var(--cat-sci); }
.sb-item.active-cat.math { background:rgba(255,159,67,.12); border-color:rgba(255,159,67,.25); color:var(--cat-math); }
.sb-item.active-cat.lang { background:rgba(124,109,240,.12); border-color:rgba(124,109,240,.25); color:var(--cat-lang); }
.sb-item.active-cat.anim { background:rgba(255,92,122,.12); border-color:rgba(255,92,122,.25); color:var(--cat-anim); }
.sb-item.active-nav { background:var(--surface2); color:var(--text); }

.sb-item-icon { font-size:18px; width:24px; text-align:center; }
.sb-item-text { flex:1; }

.sb-divider { height:1px; background:var(--border); margin:10px 18px; }

/* SIDEBAR BOTTOM MASCOT */
.sb-bottom {
  margin-top:auto; padding:16px;
  border-top:1px solid var(--border);
}
.sb-mascot { text-align:center; }
.sb-mascot-emoji { font-size:48px; display:block; animation:brainBob 3s ease-in-out infinite; }
.sb-mascot-msg { font-size:11px; font-weight:800; color:var(--green); margin-top:6px; }

/* MAIN CONTENT */
.main-wrap {
  flex:1; display:flex; flex-direction:column; min-width:0;
}

/* TOP BAR */
.topbar {
  display:flex; align-items:center; gap:16px;
  padding:14px 28px;
  background:rgba(7,7,26,.85); backdrop-filter:blur(14px);
  border-bottom:1px solid var(--border);
  position:sticky; top:0; z-index:50;
}
.tb-brand { font-size:15px; font-weight:900; }
.tb-brand .quiz{color:white;} .tb-brand .master{background:linear-gradient(135deg,var(--accent2),var(--green));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.tb-topic-pill {
  display:flex; align-items:center; gap:7px;
  background:var(--surface); border:1px solid var(--border2); border-radius:99px;
  padding:6px 14px; font-size:13px; font-weight:700;
}
.tb-topic-pill .pill-icon { font-size:16px; }
.tb-sep { flex:1; }
.tb-stat {
  display:flex; align-items:center; gap:7px;
  background:var(--surface); border:1px solid var(--border); border-radius:99px;
  padding:7px 14px; font-size:13px; font-weight:700;
}
.tb-stat-icon { font-size:16px; }
.tb-stat-val { font-weight:900; }
.tb-stat-lbl { font-size:10px; color:var(--muted); font-family:var(--mono); }

/* CONTENT AREA */
.content-area { flex:1; padding:28px; overflow-y:auto; }

/* SUB-SCREENS */
.sub { display:none; animation:fadein .3s ease both; }
.sub.active { display:block; }
@keyframes fadein { from{opacity:0;transform:scale(1.01)} to{opacity:1;transform:scale(1)} }

/* ── HOME: CATEGORY GRID ── */
.home-header { margin-bottom:24px; }
.home-header h1 { font-size:1.5rem; font-weight:900; letter-spacing:-.02em; }
.home-header p { font-size:12px; font-family:var(--mono); color:var(--muted); margin-top:4px; }

.cat-grid {
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(300px,1fr));
  gap:16px;
}

.cat-card {
  border-radius:20px; border:1px solid; padding:24px;
  cursor:pointer; position:relative; overflow:hidden;
  transition:transform .2s,box-shadow .2s;
}
.cat-card:hover { transform:translateY(-5px); }
.cat-card.sci  { background:linear-gradient(145deg,rgba(0,201,122,.1),rgba(0,201,122,.04)); border-color:rgba(0,201,122,.3); }
.cat-card.math { background:linear-gradient(145deg,rgba(255,159,67,.1),rgba(255,159,67,.04)); border-color:rgba(255,159,67,.3); }
.cat-card.lang { background:linear-gradient(145deg,rgba(124,109,240,.1),rgba(124,109,240,.04)); border-color:rgba(124,109,240,.3); }
.cat-card.anim { background:linear-gradient(145deg,rgba(255,92,122,.1),rgba(255,92,122,.04)); border-color:rgba(255,92,122,.3); }
.cat-card.sci:hover  { box-shadow:0 16px 48px rgba(0,201,122,.2); }
.cat-card.math:hover { box-shadow:0 16px 48px rgba(255,159,67,.2); }
.cat-card.lang:hover { box-shadow:0 16px 48px rgba(124,109,240,.2); }
.cat-card.anim:hover { box-shadow:0 16px 48px rgba(255,92,122,.2); }

.cat-card-top { display:flex; align-items:flex-start; gap:16px; margin-bottom:18px; }
.cat-card-icon { font-size:44px; line-height:1; flex-shrink:0; }
.cat-card-title { font-size:18px; font-weight:900; }
.cat-card-title.sci  { color:var(--cat-sci); }
.cat-card-title.math { color:var(--cat-math); }
.cat-card-title.lang { color:#a89bf5; }
.cat-card-title.anim { color:#ff7a95; }
.cat-card-desc { font-size:12px; color:var(--muted); margin-top:4px; line-height:1.5; font-family:var(--mono); }
.cat-card-topics { display:flex; flex-wrap:wrap; gap:7px; }
.topic-tag {
  font-size:11px; font-family:var(--mono); font-weight:600;
  padding:4px 12px; border-radius:99px; border:1px solid;
}
.cat-card-footer { display:flex; justify-content:space-between; align-items:center; margin-top:16px; }
.cat-card-meta { font-size:11px; font-family:var(--mono); }
.cat-card-arrow { font-size:18px; transition:transform .2s; }
.cat-card:hover .cat-card-arrow { transform:translateX(6px); }

/* ── TOPICS ── */
.topics-header { display:flex; align-items:center; gap:14px; margin-bottom:22px; }
.topics-icon { font-size:36px; }
.topics-title-wrap h2 { font-size:1.4rem; font-weight:900; }
.topics-title-wrap p { font-size:11px; color:var(--muted); font-family:var(--mono); margin-top:3px; }

.topics-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(160px,1fr)); gap:12px; }
.topic-card {
  background:var(--surface); border:1px solid var(--border); border-radius:16px;
  padding:18px; cursor:pointer; position:relative; overflow:hidden;
  transition:transform .15s,border-color .15s,background .15s;
}
.topic-card:not(.locked):hover { transform:translateY(-3px); background:var(--surface2); }
.topic-card.locked { cursor:not-allowed; opacity:.45; }
.topic-icon { font-size:28px; margin-bottom:12px; display:block; }
.topic-name { font-size:13px; font-weight:800; line-height:1.3; }
.topic-meta { font-size:10px; color:var(--muted); margin-top:4px; font-family:var(--mono); }
.topic-status { position:absolute; top:10px; right:10px; font-size:9px; font-weight:600; font-family:var(--mono); padding:3px 8px; border-radius:99px; letter-spacing:.04em; }
.s-locked { background:rgba(255,255,255,.05); color:var(--muted); }
.s-done { background:var(--green-bg); color:var(--green); border:1px solid rgba(0,229,160,.2); }
.s-new { background:rgba(124,109,240,.15); color:var(--accent2); border:1px solid rgba(124,109,240,.25); }
.lock-icon { position:absolute; bottom:10px; right:10px; font-size:15px; opacity:.4; }

/* ── QUIZ ── */
.quiz-nav { display:flex; align-items:center; gap:14px; margin-bottom:20px; }
.back-btn {
  background:var(--surface); border:1px solid var(--border); border-radius:var(--rsm);
  padding:8px 16px; font-size:12px; font-family:var(--mono); color:var(--muted);
  cursor:pointer; transition:background .1s,color .1s; display:flex; align-items:center; gap:6px;
}
.back-btn:hover { background:var(--surface2); color:var(--text); }
.quiz-info { flex:1; }
.quiz-topic-label { font-size:15px; font-weight:800; }
.quiz-q-counter { font-size:11px; font-family:var(--mono); color:var(--muted); }

/* QUIZ TWO-COL LAYOUT */
.quiz-layout { display:grid; grid-template-columns:1fr 280px; gap:20px; align-items:start; }

.quiz-main {}
.progress-outer { height:4px; background:var(--surface2); border-radius:99px; margin-bottom:22px; overflow:hidden; }
.progress-fill { height:100%; background:linear-gradient(90deg,var(--accent),var(--green)); border-radius:99px; transition:width .4s ease; }

.q-header-row { display:flex; align-items:center; justify-content:space-between; margin-bottom:16px; }
.q-num-badge {
  background:var(--surface2); border:1px solid var(--border); border-radius:99px;
  padding:5px 14px; font-size:11px; font-family:var(--mono); color:var(--muted);
}
.q-xp-badge { font-size:11px; font-family:var(--mono); color:var(--gold); display:flex; align-items:center; gap:5px; }

.question-box { background:var(--surface); border:1px solid var(--border); border-radius:20px; padding:24px; margin-bottom:14px; }
.q-text { font-size:17px; font-weight:800; line-height:1.55; }

.options { display:flex; flex-direction:column; gap:10px; }
.opt-btn {
  background:var(--surface); border:1px solid var(--border); border-radius:14px;
  padding:14px 18px; text-align:left; cursor:pointer;
  font-size:14px; font-family:var(--font); color:var(--text); font-weight:700;
  display:flex; align-items:center; gap:14px;
  transition:background .12s,border-color .12s,transform .1s;
}
.opt-btn:hover:not(:disabled) { background:var(--surface2); border-color:var(--border2); transform:translateX(6px); }
.opt-btn:disabled { cursor:default; }
.opt-btn.correct { background:rgba(0,229,160,.12); border-color:var(--green); color:var(--green); }
.opt-btn.wrong   { background:rgba(255,92,122,.12); border-color:var(--red); color:var(--red); }
.opt-letter {
  width:30px;height:30px;border-radius:8px;flex-shrink:0;
  background:var(--surface2); border:1px solid var(--border);
  display:flex;align-items:center;justify-content:center;
  font-size:12px;font-family:var(--mono);font-weight:600;
  transition:background .12s,border-color .12s,color .12s;
}
.opt-btn.correct .opt-letter { background:var(--green);border-color:var(--green);color:var(--bg); }
.opt-btn.wrong   .opt-letter { background:var(--red);  border-color:var(--red);  color:var(--bg); }

.feedback-strip { display:none;align-items:center;gap:10px;padding:13px 18px;border-radius:12px;font-size:13px;margin:14px 0;font-weight:700; }
.feedback-strip.show { display:flex; }
.feedback-strip.correct { background:var(--green-bg);color:var(--green);border:1px solid rgba(0,229,160,.2); }
.feedback-strip.wrong   { background:var(--red-bg);  color:var(--red);  border:1px solid rgba(255,92,122,.2); }

.next-btn { width:100%;padding:15px;background:linear-gradient(135deg,var(--accent),var(--green));color:white;border:none;border-radius:14px;font-size:15px;font-weight:800;font-family:var(--font);cursor:pointer;display:none;transition:opacity .15s,transform .15s; }
.next-btn.show { display:block; }
.next-btn:hover { opacity:.9;transform:translateY(-2px); }

/* QUIZ SIDEBAR */
.quiz-sidebar {}
.quiz-prog-card {
  background:var(--surface); border:1px solid var(--border); border-radius:20px;
  padding:22px; margin-bottom:14px;
}
.quiz-prog-title { font-size:12px; font-weight:800; color:var(--muted); text-transform:uppercase; letter-spacing:.08em; margin-bottom:16px; }
.quiz-prog-circle {
  width:110px; height:110px;
  margin:0 auto 12px;
  position:relative; display:flex; align-items:center; justify-content:center;
}
.qpc-svg { width:110px;height:110px;transform:rotate(-90deg); }
.qpc-bg { fill:none;stroke:var(--surface2);stroke-width:8; }
.qpc-fill { fill:none;stroke-width:8;stroke-linecap:round;transition:stroke-dashoffset .6s cubic-bezier(.4,0,.2,1); }
.qpc-fill.sci  { stroke:var(--cat-sci); }
.qpc-fill.math { stroke:var(--cat-math); }
.qpc-fill.lang { stroke:var(--cat-lang); }
.qpc-fill.anim { stroke:var(--cat-anim); }
.qpc-val { position:absolute;font-size:22px;font-weight:900;font-family:var(--mono); }
.quiz-prog-sub { text-align:center; font-size:11px; font-family:var(--mono); color:var(--muted); }

.hint-card {
  background:var(--surface); border:1px solid rgba(255,209,102,.2); border-radius:20px;
  padding:18px;
}
.hint-header { display:flex; align-items:center; justify-content:space-between; margin-bottom:10px; }
.hint-title { font-size:12px; font-weight:800; color:var(--gold); display:flex; align-items:center; gap:6px; }
.hint-body { font-size:13px; color:rgba(255,255,255,.75); line-height:1.6; }
.hint-img { margin-top:12px; font-size:48px; text-align:center; display:block; animation:brainBob 3s ease-in-out infinite; }

/* ── RESULT ── */
.result-unlock { background:var(--gold-bg);border:1px solid rgba(255,209,102,.25);border-radius:14px;padding:14px 18px;margin-bottom:18px;font-size:13px;color:var(--gold);align-items:center;gap:10px;display:none;font-weight:700; }
.result-unlock.show { display:flex; }
.result-hero { background:var(--surface);border:1px solid var(--border);border-radius:22px;padding:32px 24px;text-align:center;margin-bottom:14px; }
.result-emoji { font-size:60px;margin-bottom:16px;display:block;animation:pop .4s cubic-bezier(.34,1.56,.64,1) both; }
@keyframes pop { from{transform:scale(0)}to{transform:scale(1)} }
.result-title { font-size:24px;font-weight:900;letter-spacing:-.02em; }
.result-sub { font-size:13px;color:var(--muted);margin-top:5px;font-family:var(--mono); }
.result-stats { display:flex;gap:12px;margin:22px 0;justify-content:center; }
.r-stat { background:var(--bg2);border-radius:14px;padding:14px 20px;text-align:center;min-width:85px; }
.r-val { font-size:24px;font-weight:900;font-family:var(--mono); }
.r-lbl { font-size:10px;color:var(--muted);font-family:var(--mono);margin-top:2px;letter-spacing:.04em; }
.result-xp { font-size:22px;font-weight:800;font-family:var(--mono);color:var(--green);margin:4px 0; }
.btn-row { display:flex;flex-direction:column;gap:10px; }
.btn-back-cat { padding:14px;background:var(--surface);border:1px solid var(--border);border-radius:14px;font-size:13px;font-family:var(--font);font-weight:700;color:var(--text);cursor:pointer;transition:background .12s; }
.btn-back-cat:hover { background:var(--surface2); }
.btn-retry { padding:14px;background:linear-gradient(135deg,var(--accent),var(--green));border:none;border-radius:14px;font-size:15px;font-weight:800;font-family:var(--font);color:white;cursor:pointer;transition:opacity .12s; }
.btn-retry:hover { opacity:.88; }

/* XP TOAST */
.xp-toast { position:fixed;top:68px;right:20px;z-index:200;background:var(--surface2);border:1px solid var(--green);border-radius:12px;padding:10px 18px;font-size:14px;font-weight:800;font-family:var(--mono);color:var(--green);opacity:0;transform:translateX(20px);transition:opacity .3s,transform .3s;pointer-events:none; }
.xp-toast.show { opacity:1;transform:translateX(0); }

.fade-out { animation:fout .28s ease forwards; }
@keyframes fout { to{opacity:0;transform:scale(.98)} }

/* NAV BACK BTN */
.nav-back-btn {
  background:var(--surface); border:1px solid var(--border);
  border-radius:var(--rsm); padding:6px 14px;
  font-size:11px; font-family:var(--mono); color:var(--muted);
  cursor:pointer; transition:background .1s,color .1s; display:none;
}
.nav-back-btn.visible { display:flex; align-items:center; gap:5px; }
.nav-back-btn:hover { background:var(--surface2); color:var(--text); }

/* XP BAR (topbar) */
.xp-bar-wrap { display:flex; align-items:center; gap:9px; }
.xp-level-badge { background:linear-gradient(135deg,var(--accent),#9c5ce7); border-radius:8px; padding:5px 12px; font-size:11px; font-family:var(--mono); color:white; font-weight:700; white-space:nowrap; }
.xp-track-outer { width:100px; }
.xp-track { height:5px; background:var(--surface2); border-radius:99px; overflow:hidden; }
.xp-fill { height:100%; background:linear-gradient(90deg,var(--accent),var(--green)); border-radius:99px; transition:width .7s cubic-bezier(.4,0,.2,1); }
.xp-track-lbl { font-size:10px; color:var(--muted); font-family:var(--mono); margin-top:3px; text-align:right; }
.xp-total { font-size:12px; font-family:var(--mono); font-weight:700; color:var(--green); white-space:nowrap; }

/* Responsive */
@media(max-width:900px) {
  .landing-layout { grid-template-columns:1fr; }
  .landing-right { display:none; }
  .quiz-layout { grid-template-columns:1fr; }
  .quiz-sidebar { display:none; }
  .sidebar { width:64px; }
  .sb-item-text,.sb-section-label,.sb-brand-text,.sb-mascot-msg { display:none; }
  .sb-brand-icon { margin:0 auto; }
  .sb-item { justify-content:center; }
  .content-area { padding:18px; }
}

/* ═══════════════════════════════
   LOGIN SCREEN
═══════════════════════════════ */
#screen-login {
  min-height:100vh; display:none;
  align-items:center; justify-content:center;
  padding:24px; overflow:hidden;
}
#screen-login.active { display:flex; }

.login-card {
  background:rgba(22,22,48,.92); backdrop-filter:blur(24px);
  border:1px solid rgba(124,109,240,.25);
  border-radius:28px; padding:48px 44px;
  width:100%; max-width:420px;
  position:relative; z-index:2;
  box-shadow:0 32px 80px rgba(0,0,0,.5), 0 0 0 1px rgba(124,109,240,.1);
  animation:loginSlide .5s cubic-bezier(.34,1.2,.64,1) both;
}
@keyframes loginSlide { from{opacity:0;transform:translateY(30px) scale(.97)} to{opacity:1;transform:translateY(0) scale(1)} }

.login-logo { text-align:center; margin-bottom:32px; }
.login-logo-emoji { font-size:56px; display:block; animation:brainBob 3s ease-in-out infinite; filter:drop-shadow(0 0 20px rgba(124,109,240,.5)); }
.login-logo-title { font-size:26px; font-weight:900; margin-top:10px; }
.login-logo-title .quiz{color:white;} .login-logo-title .master{background:linear-gradient(135deg,var(--accent2),var(--green));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.login-logo-sub { font-size:12px; color:var(--muted); font-family:var(--mono); margin-top:5px; }

.login-tabs {
  display:flex; background:var(--surface2); border-radius:12px; padding:4px;
  margin-bottom:28px; gap:4px;
}
.login-tab {
  flex:1; padding:9px; border-radius:9px; border:none;
  font-size:13px; font-weight:800; font-family:var(--font);
  cursor:pointer; transition:background .2s,color .2s;
  background:transparent; color:var(--muted);
}
.login-tab.active { background:var(--accent); color:white; box-shadow:0 4px 14px rgba(124,109,240,.4); }

.login-field { margin-bottom:18px; }
.login-label { font-size:12px; font-weight:700; color:var(--muted); font-family:var(--mono); letter-spacing:.06em; display:block; margin-bottom:7px; }
.login-input {
  width:100%; padding:13px 16px;
  background:var(--surface2); border:1px solid var(--border2);
  border-radius:12px; font-size:14px; font-weight:600; font-family:var(--font); color:var(--text);
  outline:none; transition:border-color .2s,box-shadow .2s;
}
.login-input:focus { border-color:var(--accent); box-shadow:0 0 0 3px rgba(124,109,240,.18); }
.login-input::placeholder { color:var(--muted); font-weight:500; }

.login-input-wrap { position:relative; }
.login-eye { position:absolute; right:14px; top:50%; transform:translateY(-50%); background:none; border:none; cursor:pointer; font-size:16px; color:var(--muted); padding:4px; }

.login-btn {
  width:100%; padding:15px;
  background:linear-gradient(135deg,var(--accent),#9c5ce7);
  color:white; border:none; border-radius:12px;
  font-size:15px; font-weight:800; font-family:var(--font);
  cursor:pointer; margin-top:6px;
  box-shadow:0 6px 28px rgba(124,109,240,.4);
  transition:transform .15s,box-shadow .15s,opacity .15s;
}
.login-btn:hover { transform:translateY(-2px); box-shadow:0 10px 36px rgba(124,109,240,.55); }
.login-btn:active { transform:translateY(0); opacity:.9; }

.login-error {
  background:rgba(255,92,122,.1); border:1px solid rgba(255,92,122,.3);
  border-radius:10px; padding:11px 15px;
  font-size:13px; color:var(--red); font-weight:700;
  margin-bottom:16px; display:none; align-items:center; gap:9px;
}
.login-error.show { display:flex; }

.login-divider { display:flex; align-items:center; gap:12px; margin:22px 0; }
.login-divider-line { flex:1; height:1px; background:var(--border); }
.login-divider-text { font-size:11px; color:var(--muted); font-family:var(--mono); }

.login-guest-btn {
  width:100%; padding:13px;
  background:transparent; border:1px solid var(--border2); border-radius:12px;
  font-size:14px; font-weight:700; font-family:var(--font); color:var(--text);
  cursor:pointer; transition:background .15s,border-color .15s;
}
.login-guest-btn:hover { background:var(--surface); border-color:var(--accent); }

.login-admin-link {
  text-align:center; margin-top:18px;
  font-size:11px; color:var(--muted); font-family:var(--mono);
}
.login-admin-link a { color:var(--accent2); cursor:pointer; text-decoration:none; }
.login-admin-link a:hover { color:var(--text); }

/* ═══════════════════════════════
   ADMIN SCREEN
═══════════════════════════════ */
#screen-admin {
  min-height:100vh; display:none; flex-direction:column; overflow:hidden;
}
#screen-admin.active { display:flex; }

.admin-topbar {
  display:flex; align-items:center; gap:16px;
  padding:14px 32px;
  background:rgba(7,7,26,.9); backdrop-filter:blur(16px);
  border-bottom:1px solid var(--border);
  position:sticky; top:0; z-index:50;
}
.admin-brand { display:flex; align-items:center; gap:10px; }
.admin-brand-icon { font-size:22px; }
.admin-brand-text { font-size:15px; font-weight:900; }
.admin-brand-badge {
  background:linear-gradient(135deg,#ff6b9d,#ff4757); border-radius:99px;
  padding:3px 11px; font-size:10px; font-weight:800; font-family:var(--mono); color:white;
  letter-spacing:.05em;
}
.admin-sep { flex:1; }
.admin-user-pill {
  display:flex; align-items:center; gap:8px;
  background:var(--surface); border:1px solid var(--border2); border-radius:99px;
  padding:7px 16px; font-size:12px; font-weight:700;
}
.admin-user-dot { width:8px; height:8px; border-radius:50%; background:var(--green); animation:blink 2s ease infinite; }
@keyframes blink { 0%,100%{opacity:1}50%{opacity:.2} }
.admin-logout-btn {
  background:rgba(255,92,122,.12); border:1px solid rgba(255,92,122,.3); border-radius:99px;
  padding:7px 16px; font-size:12px; font-weight:700; font-family:var(--font); color:var(--red);
  cursor:pointer; transition:background .15s;
}
.admin-logout-btn:hover { background:rgba(255,92,122,.22); }

.admin-content { flex:1; padding:32px; max-width:1100px; margin:0 auto; width:100%; }

.admin-hero { margin-bottom:32px; }
.admin-hero h1 { font-size:1.8rem; font-weight:900; letter-spacing:-.02em; }
.admin-hero p { font-size:13px; color:var(--muted); font-family:var(--mono); margin-top:5px; }
.admin-hero-last { font-size:11px; color:var(--muted); font-family:var(--mono); margin-top:4px; }

/* KPI CARDS */
.admin-kpis { display:grid; grid-template-columns:repeat(auto-fill,minmax(200px,1fr)); gap:14px; margin-bottom:32px; }
.kpi-card {
  border-radius:20px; padding:22px;
  border:1px solid; position:relative; overflow:hidden;
  transition:transform .18s;
}
.kpi-card:hover { transform:translateY(-3px); }
.kpi-card.blue  { background:rgba(124,109,240,.08); border-color:rgba(124,109,240,.25); }
.kpi-card.green { background:rgba(0,229,160,.08);  border-color:rgba(0,229,160,.25); }
.kpi-card.gold  { background:rgba(255,209,102,.08); border-color:rgba(255,209,102,.25); }
.kpi-card.red   { background:rgba(255,92,122,.08);  border-color:rgba(255,92,122,.25); }
.kpi-card.purple{ background:rgba(190,100,250,.08); border-color:rgba(190,100,250,.25); }
.kpi-icon { font-size:32px; margin-bottom:12px; display:block; }
.kpi-val { font-size:2rem; font-weight:900; font-family:var(--mono); line-height:1; }
.kpi-lbl { font-size:12px; color:var(--muted); font-family:var(--mono); margin-top:5px; letter-spacing:.04em; }
.kpi-trend { font-size:11px; font-weight:700; margin-top:8px; }
.kpi-trend.up { color:var(--green); } .kpi-trend.neu { color:var(--muted); }

/* SECTION HEADERS */
.admin-section-header { display:flex; align-items:center; justify-content:space-between; margin-bottom:16px; }
.admin-section-title { font-size:15px; font-weight:900; }
.admin-section-sub { font-size:11px; color:var(--muted); font-family:var(--mono); margin-top:2px; }
.admin-refresh-btn {
  background:var(--surface); border:1px solid var(--border); border-radius:99px;
  padding:6px 14px; font-size:11px; font-family:var(--mono); font-weight:600; color:var(--muted);
  cursor:pointer; transition:background .12s,color .12s; display:flex; align-items:center; gap:6px;
}
.admin-refresh-btn:hover { background:var(--surface2); color:var(--text); }

/* ACCESS LOG TABLE */
.admin-table-wrap { background:var(--surface); border:1px solid var(--border); border-radius:20px; overflow:hidden; margin-bottom:28px; }
.admin-table { width:100%; border-collapse:collapse; }
.admin-table th { padding:13px 18px; text-align:left; font-size:10px; font-weight:700; font-family:var(--mono); color:var(--muted); letter-spacing:.08em; text-transform:uppercase; border-bottom:1px solid var(--border); background:rgba(255,255,255,.02); }
.admin-table td { padding:13px 18px; font-size:13px; border-bottom:1px solid rgba(255,255,255,.04); }
.admin-table tr:last-child td { border-bottom:none; }
.admin-table tr:hover td { background:rgba(255,255,255,.02); }
.admin-badge { display:inline-block; padding:3px 10px; border-radius:99px; font-size:10px; font-weight:700; font-family:var(--mono); }
.admin-badge.guest  { background:rgba(255,255,255,.07); color:var(--muted); }
.admin-badge.user   { background:rgba(124,109,240,.15); color:var(--accent2); }
.admin-badge.admin  { background:rgba(255,209,102,.15); color:var(--gold); }

/* CHART BAR */
.admin-chart { background:var(--surface); border:1px solid var(--border); border-radius:20px; padding:24px; margin-bottom:28px; }
.admin-chart-bars { display:flex; align-items:flex-end; gap:10px; height:120px; margin-top:18px; }
.admin-bar-col { flex:1; display:flex; flex-direction:column; align-items:center; gap:6px; }
.admin-bar { width:100%; border-radius:8px 8px 0 0; background:linear-gradient(to top,var(--accent),var(--accent2)); transition:height .6s cubic-bezier(.4,0,.2,1); min-height:4px; }
.admin-bar-lbl { font-size:9px; color:var(--muted); font-family:var(--mono); }
.admin-bar-val { font-size:10px; font-weight:700; font-family:var(--mono); color:var(--accent2); }

/* USERS REGISTERED TABLE */
.admin-users-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(280px,1fr)); gap:12px; margin-bottom:28px; }
.admin-user-card {
  background:var(--surface); border:1px solid var(--border); border-radius:16px; padding:18px;
  display:flex; align-items:center; gap:14px;
}
.admin-user-avatar { width:42px; height:42px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:18px; flex-shrink:0; font-weight:900; }
.admin-user-name { font-size:14px; font-weight:800; }
.admin-user-meta { font-size:11px; color:var(--muted); font-family:var(--mono); margin-top:2px; }

.admin-empty { text-align:center; padding:40px; color:var(--muted); font-family:var(--mono); font-size:13px; }
.admin-empty-icon { font-size:40px; display:block; margin-bottom:12px; opacity:.4; }
</style>
</head>
<body>

<!-- ═══ LOGIN SCREEN ═══ -->
<div id="screen-login" class="screen active">
  <div class="beam beam-1"></div>
  <div class="beam beam-2"></div>
  <div class="beam beam-3"></div>

  <div class="login-card">
    <div class="login-logo">
      <span class="login-logo-emoji">🧠</span>
      <div class="login-logo-title"><span class="quiz">Quiz</span><span class="master">Master</span></div>
      <div class="login-logo-sub">teste seus conhecimentos!</div>
    </div>

    <!-- TABS -->
    <div class="login-tabs">
      <button class="login-tab active" id="tab-login" onclick="switchTab('login')">Entrar</button>
      <button class="login-tab" id="tab-register" onclick="switchTab('register')">Cadastrar</button>
    </div>

    <!-- ERROR MESSAGE -->
    <div class="login-error" id="login-error">
      <span>⚠️</span><span id="login-error-msg">Usuário ou senha incorretos.</span>
    </div>

    <!-- LOGIN FORM -->
    <div id="form-login">
      <div class="login-field">
        <label class="login-label">USUÁRIO</label>
        <input class="login-input" id="li-user" type="text" placeholder="seu usuário" autocomplete="username">
      </div>
      <div class="login-field">
        <label class="login-label">SENHA</label>
        <div class="login-input-wrap">
          <input class="login-input" id="li-pass" type="password" placeholder="••••••••" autocomplete="current-password" onkeydown="if(event.key==='Enter')doLogin()">
          <button class="login-eye" onclick="togglePass('li-pass',this)">👁</button>
        </div>
      </div>
      <button class="login-btn" onclick="doLogin()">Entrar →</button>

      <div class="login-divider">
        <div class="login-divider-line"></div>
        <span class="login-divider-text">ou</span>
        <div class="login-divider-line"></div>
      </div>
      <button class="login-guest-btn" onclick="doGuest()">👤 Continuar como visitante</button>

      <div class="login-admin-link">
        <a onclick="switchTab('admin-login')">🔐 Acesso administrativo</a>
      </div>
    </div>

    <!-- REGISTER FORM -->
    <div id="form-register" style="display:none">
      <div class="login-field">
        <label class="login-label">USUÁRIO</label>
        <input class="login-input" id="reg-user" type="text" placeholder="escolha um nome">
      </div>
      <div class="login-field">
        <label class="login-label">SENHA</label>
        <div class="login-input-wrap">
          <input class="login-input" id="reg-pass" type="password" placeholder="mínimo 4 caracteres" onkeydown="if(event.key==='Enter')doRegister()">
          <button class="login-eye" onclick="togglePass('reg-pass',this)">👁</button>
        </div>
      </div>
      <button class="login-btn" style="background:linear-gradient(135deg,var(--cat-sci),#00b894);" onclick="doRegister()">Criar conta →</button>
      <div class="login-divider">
        <div class="login-divider-line"></div>
        <span class="login-divider-text">ou</span>
        <div class="login-divider-line"></div>
      </div>
      <button class="login-guest-btn" onclick="doGuest()">👤 Continuar como visitante</button>
    </div>

    <!-- ADMIN LOGIN FORM -->
    <div id="form-admin-login" style="display:none">
      <div style="text-align:center;margin-bottom:20px;">
        <span style="font-size:32px">🔐</span>
        <div style="font-size:14px;font-weight:800;color:var(--gold);margin-top:6px;">Painel Administrativo</div>
        <div style="font-size:11px;color:var(--muted);font-family:var(--mono);margin-top:3px;">Acesso restrito</div>
      </div>
      <div class="login-field">
        <label class="login-label">USUÁRIO ADMIN</label>
        <input class="login-input" id="adm-user" type="text" placeholder="admin">
      </div>
      <div class="login-field">
        <label class="login-label">SENHA</label>
        <div class="login-input-wrap">
          <input class="login-input" id="adm-pass" type="password" placeholder="••••••••" onkeydown="if(event.key==='Enter')doAdminLogin()">
          <button class="login-eye" onclick="togglePass('adm-pass',this)">👁</button>
        </div>
      </div>
      <button class="login-btn" style="background:linear-gradient(135deg,#ff6b9d,#ff4757);" onclick="doAdminLogin()">Acessar painel →</button>
      <div style="text-align:center;margin-top:14px;">
        <a style="font-size:11px;color:var(--muted);cursor:pointer;font-family:var(--mono);" onclick="switchTab('login')">← voltar ao login</a>
      </div>
    </div>

  </div>
</div>

<!-- ═══ ADMIN SCREEN ═══ -->
<div id="screen-admin" class="screen">
  <div class="beam beam-1"></div>
  <div class="beam beam-2"></div>
  <div class="beam beam-3"></div>

  <nav class="admin-topbar">
    <div class="admin-brand">
      <span class="admin-brand-icon">🛡️</span>
      <span class="admin-brand-text">QuizMaster Admin</span>
      <span class="admin-brand-badge">ADMIN</span>
    </div>
    <div class="admin-sep"></div>
    <div class="admin-user-pill">
      <div class="admin-user-dot"></div>
      <span id="admin-username-display">admin</span>
    </div>
    <button class="admin-logout-btn" onclick="adminLogout()">← Sair</button>
  </nav>

  <div class="admin-content">
    <div class="admin-hero">
      <h1>📊 Painel de Controle</h1>
      <p id="admin-hero-sub">Monitoramento de acessos e usuários do QuizMaster</p>
      <p class="admin-hero-last" id="admin-last-update"></p>
    </div>

    <!-- KPIs -->
    <div class="admin-kpis" id="admin-kpis"></div>

    <!-- CHART -->
    <div class="admin-chart">
      <div class="admin-section-header">
        <div>
          <div class="admin-section-title">Acessos nos últimos 7 dias</div>
          <div class="admin-section-sub">visitantes únicos por dia</div>
        </div>
        <button class="admin-refresh-btn" onclick="refreshAdmin()">↻ Atualizar</button>
      </div>
      <div class="admin-chart-bars" id="admin-chart-bars"></div>
    </div>

    <!-- ACCESS LOG -->
    <div style="margin-bottom:28px;">
      <div class="admin-section-header">
        <div>
          <div class="admin-section-title">Log de Acessos Recentes</div>
          <div class="admin-section-sub">últimas 50 entradas</div>
        </div>
      </div>
      <div class="admin-table-wrap">
        <table class="admin-table">
          <thead>
            <tr>
              <th>#</th>
              <th>Usuário</th>
              <th>Tipo</th>
              <th>Data / Hora</th>
              <th>Ação</th>
            </tr>
          </thead>
          <tbody id="admin-log-body"></tbody>
        </table>
      </div>
    </div>

    <!-- REGISTERED USERS -->
    <div>
      <div class="admin-section-header">
        <div>
          <div class="admin-section-title">Usuários Cadastrados</div>
          <div class="admin-section-sub" id="admin-users-count">0 usuários</div>
        </div>
      </div>
      <div class="admin-users-grid" id="admin-users-grid"></div>
    </div>
  </div>
</div>

<!-- ═══ LANDING ═══ -->
<div id="screen-landing" class="screen">
  <div class="beam beam-1"></div>
  <div class="beam beam-2"></div>
  <div class="beam beam-3"></div>
  <div class="beam beam-4"></div>
  <div class="neon-lines"></div>

  <button class="conquistas-btn" onclick="showConquistas()">🎁 Conquistas</button>

  <!-- FLOATING DECORATIONS -->
  <div class="floaty floaty-trophy">🏆</div>
  <div class="floaty floaty-plane">✈️</div>
  <div class="floaty floaty-star1">✦</div>
  <div class="floaty floaty-star2">✦</div>
  <div class="floaty floaty-sparkle">✨</div>

  <div class="landing-layout">
    <!-- LEFT -->
    <div class="landing-left">
      <div class="logo-badge-top"><span class="star-icon">★</span> QUIZMASTER</div>
      <h1 class="landing-title"><span class="quiz">Quiz</span><span class="master">Master</span></h1>
      <p class="landing-desc-big">Teste seus conhecimentos!</p>
      <p class="landing-desc-sub">Desafie-se em diferentes categorias e evolua seu conhecimento de forma divertida e interativa!</p>

      <div style="position:relative;display:inline-block;">
        <button class="btn-start-big" onclick="goToApp()">
          <span class="rocket">🚀</span>
          COMEÇAR AGORA
          <span class="arr">→</span>
        </button>
      </div>
      <p class="landing-hint">Escolha um tema e responda as perguntas ↗</p>

      <!-- STATS -->
      <div class="landing-stats" id="landing-stats">
        <div class="ls-item">
          <span class="ls-icon">🔥</span>
          <div class="ls-val" id="ls-seq">0</div>
          <div class="ls-lbl">Sequência</div>
        </div>
        <div class="ls-item">
          <span class="ls-icon">⭐</span>
          <div class="ls-val" id="ls-acertos">0</div>
          <div class="ls-lbl">Acertos</div>
        </div>
        <div class="ls-item">
          <span class="ls-icon">🏆</span>
          <div class="ls-val" id="ls-quizzes">0</div>
          <div class="ls-lbl">Quiz feitos</div>
        </div>
        <div class="ls-item">
          <span class="ls-icon">💎</span>
          <div class="ls-val" id="ls-xp">0 XP</div>
          <div class="ls-lbl">Experiência</div>
        </div>
      </div>
    </div>

    <!-- RIGHT: CATEGORY CARDS -->
    <div class="landing-right">
      <div class="lcat-card sci" onclick="goToApp('science')">
        <div>
          <span class="lcat-icon">🔬</span>
          <div class="lcat-name sci">Ciências</div>
          <div class="lcat-desc">Explore o mundo científico</div>
        </div>
        <div class="lcat-progress">
          <div class="lcat-prog-row">
            <span class="lcat-prog-num" id="lp-sci">0/20</span>
            <div class="lcat-prog-bar"><div class="lcat-prog-fill sci" id="lpf-sci" style="width:0%"></div></div>
          </div>
        </div>
      </div>
      <div class="lcat-card math" onclick="goToApp('math')">
        <div>
          <span class="lcat-icon">🧮</span>
          <div class="lcat-name math">Matemática</div>
          <div class="lcat-desc">Números, lógica e raciocínio</div>
        </div>
        <div class="lcat-progress">
          <div class="lcat-prog-row">
            <span class="lcat-prog-num" id="lp-math">0/20</span>
            <div class="lcat-prog-bar"><div class="lcat-prog-fill math" id="lpf-math" style="width:0%"></div></div>
          </div>
        </div>
      </div>
      <div class="lcat-card lang" onclick="goToApp('lang')">
        <div>
          <span class="lcat-icon">📚</span>
          <div class="lcat-name lang">Linguagens</div>
          <div class="lcat-desc">Português, idiomas e muito mais</div>
        </div>
        <div class="lcat-progress">
          <div class="lcat-prog-row">
            <span class="lcat-prog-num" id="lp-lang">0/20</span>
            <div class="lcat-prog-bar"><div class="lcat-prog-fill lang" id="lpf-lang" style="width:0%"></div></div>
          </div>
        </div>
      </div>
      <div class="lcat-card anim" onclick="goToApp('anim')">
        <div>
          <span class="lcat-icon">🎬</span>
          <div class="lcat-name anim">Personagens Animados</div>
          <div class="lcat-desc">Seus personagens favoritos</div>
        </div>
        <div class="lcat-progress">
          <div class="lcat-prog-row">
            <span class="lcat-prog-num" id="lp-anim">0/20</span>
            <div class="lcat-prog-bar"><div class="lcat-prog-fill anim" id="lpf-anim" style="width:0%"></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══ APP SCREEN ═══ -->
<div id="screen-app" class="screen">

  <!-- SIDEBAR -->
  <aside class="sidebar">
    <div class="sb-brand">
      <span class="sb-brand-icon">🧠</span>
      <div class="sb-brand-text"><span class="quiz">Quiz</span><span class="master">Master</span></div>
    </div>

    <!-- CATEGORIES -->
    <div class="sb-section">
      <div class="sb-section-label">CATEGORIAS</div>
      <div class="sb-item" id="sb-sci" onclick="goToCat('science')">
        <span class="sb-item-icon">🔬</span>
        <span class="sb-item-text">Ciências</span>
      </div>
      <div class="sb-item" id="sb-math" onclick="goToCat('math')">
        <span class="sb-item-icon">🧮</span>
        <span class="sb-item-text">Matemática</span>
      </div>
      <div class="sb-item" id="sb-lang" onclick="goToCat('lang')">
        <span class="sb-item-icon">📚</span>
        <span class="sb-item-text">Linguagens</span>
      </div>
      <div class="sb-item" id="sb-anim" onclick="goToCat('anim')">
        <span class="sb-item-icon">🎬</span>
        <span class="sb-item-text">Personagens</span>
      </div>
    </div>

    <div class="sb-divider"></div>

    <!-- NAVIGATION -->
    <div class="sb-section">
      <div class="sb-section-label">NAVEGAÇÃO</div>
      <div class="sb-item" id="sb-conquistas" onclick="showConquistas()">
        <span class="sb-item-icon">🏆</span>
        <span class="sb-item-text">Conquistas</span>
      </div>
      <div class="sb-item" id="sb-ranking" onclick="showRanking()">
        <span class="sb-item-icon">📊</span>
        <span class="sb-item-text">Ranking</span>
      </div>
      <div class="sb-item" onclick="backToLanding()">
        <span class="sb-item-icon">🏠</span>
        <span class="sb-item-text">Início</span>
      </div>
    </div>

    <div class="sb-bottom">
      <div class="sb-mascot">
        <span class="sb-mascot-emoji">🧠</span>
        <div class="sb-mascot-msg">Você é incrível!</div>
      </div>
    </div>
  </aside>

  <!-- MAIN -->
  <div class="main-wrap">
    <!-- TOP BAR -->
    <nav class="topbar">
      <div class="tb-brand"><span class="quiz">Quiz</span><span class="master">Master</span></div>
      <div class="tb-topic-pill" id="tb-topic-pill" style="display:none">
        <span class="pill-icon" id="tb-topic-icon">🔬</span>
        <span>Tema: <strong id="tb-topic-name">Ciências</strong></span>
        <span style="color:var(--muted)">⌄</span>
      </div>
      <button class="nav-back-btn" id="nav-back-btn" onclick="navBack()">← voltar</button>
      <div class="tb-sep"></div>
      <div class="tb-stat" id="tb-streak" style="display:none">
        <span class="tb-stat-icon">🔥</span>
        <div><div class="tb-stat-val" id="tb-streak-val">0</div><div class="tb-stat-lbl">Sequência</div></div>
      </div>
      <div class="xp-bar-wrap">
        <div class="xp-level-badge" id="nav-level">nível 1</div>
        <div class="xp-track-outer">
          <div class="xp-track"><div class="xp-fill" id="xp-fill" style="width:0%"></div></div>
          <div class="xp-track-lbl" id="xp-lbl">0 / 100</div>
        </div>
        <div class="xp-total" id="xp-total">💎 0 XP</div>
      </div>
    </nav>

    <!-- CONTENT -->
    <div class="content-area">

      <!-- HOME: categories -->
      <div id="sub-home" class="sub active">
        <div class="home-header">
          <h1>Escolha uma categoria</h1>
          <p id="home-sub">responda questões · ganhe XP · desbloqueie temas</p>
        </div>
        <div class="cat-grid" id="cat-grid"></div>
      </div>

      <!-- TOPICS -->
      <div id="sub-topics" class="sub">
        <div class="topics-header">
          <div class="topics-icon" id="topics-icon"></div>
          <div class="topics-title-wrap">
            <h2 id="topics-title"></h2>
            <p id="topics-desc"></p>
          </div>
        </div>
        <div class="topics-grid" id="topics-grid"></div>
      </div>

      <!-- QUIZ -->
      <div id="sub-quiz" class="sub">
        <div class="quiz-nav">
          <button class="back-btn" onclick="goTopics()">← temas</button>
          <div class="quiz-info">
            <div class="quiz-topic-label" id="quiz-topic-name"></div>
            <div class="quiz-q-counter" id="quiz-counter"></div>
          </div>
        </div>

        <div class="quiz-layout">
          <div class="quiz-main">
            <div class="progress-outer"><div class="progress-fill" id="q-progress"></div></div>
            <div class="q-header-row">
              <div class="q-num-badge" id="q-num"></div>
              <div class="q-xp-badge">⭐ +10 XP</div>
            </div>
            <div class="question-box">
              <div class="q-text" id="q-text"></div>
            </div>
            <div class="options" id="options"></div>
            <div class="feedback-strip" id="feedback"></div>
            <button class="next-btn" id="next-btn" onclick="nextQ()"></button>
          </div>
          <div class="quiz-sidebar">
            <div class="quiz-prog-card">
              <div class="quiz-prog-title">Progresso no tema</div>
              <div class="quiz-prog-circle">
                <svg class="qpc-svg" viewBox="0 0 110 110">
                  <circle class="qpc-bg" cx="55" cy="55" r="46"/>
                  <circle class="qpc-fill" id="qpc-fill" cx="55" cy="55" r="46" stroke-dasharray="289" stroke-dashoffset="289"/>
                </svg>
                <div class="qpc-val" id="qpc-val">0%</div>
              </div>
              <div class="quiz-prog-sub" id="quiz-prog-sub">0 / 0 perguntas</div>
            </div>
            <div class="hint-card" id="hint-card" style="display:none">
              <div class="hint-header">
                <div class="hint-title">💡 Dica</div>
              </div>
              <div class="hint-body" id="hint-body"></div>
              <span class="hint-img" id="hint-img"></span>
            </div>
          </div>
        </div>
      </div>

      <!-- RESULT -->
      <div id="sub-result" class="sub">
        <div class="result-unlock" id="result-unlock">
          <span style="font-size:20px">🔓</span>
          <div id="unlock-msg"></div>
        </div>
        <div class="result-hero">
          <span class="result-emoji" id="r-emoji"></span>
          <div class="result-title" id="r-title"></div>
          <div class="result-sub" id="r-sub"></div>
          <div class="result-stats">
            <div class="r-stat"><div class="r-val" id="r-correct"></div><div class="r-lbl">acertos</div></div>
            <div class="r-stat"><div class="r-val" id="r-total"></div><div class="r-lbl">questões</div></div>
            <div class="r-stat"><div class="r-val" id="r-pct"></div><div class="r-lbl">taxa</div></div>
          </div>
          <div class="result-xp" id="r-xp"></div>
        </div>
        <div class="btn-row">
          <button class="btn-retry" onclick="retryQuiz()">↺ tentar novamente</button>
          <button class="btn-back-cat" onclick="goTopics()">← outros temas desta categoria</button>
          <button class="btn-back-cat" onclick="goHome()">⊞ escolher outra categoria</button>
        </div>
      </div>

      <!-- CONQUISTAS -->
      <div id="sub-conquistas" class="sub">
        <div class="home-header">
          <h1>🏆 Conquistas</h1>
          <p>Desbloqueie medalhas completando desafios</p>
        </div>
        <div id="conquistas-grid" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:14px;"></div>
      </div>

      <!-- RANKING -->
      <div id="sub-ranking" class="sub">
        <div class="home-header">
          <h1>📊 Ranking</h1>
          <p>Acompanhe seu progresso em cada categoria</p>
        </div>
        <div id="ranking-content"></div>
      </div>

    </div><!-- /content-area -->
  </div><!-- /main-wrap -->
</div>

<div class="xp-toast" id="toast"></div>

<script>
/* ══════════════════════════════════
   DATA
══════════════════════════════════ */
const CATEGORIES = [
  {
    id:'anim', name:'Personagens Animados', icon:'🎭',
    desc:'Universo dos desenhos, animes e animações clássicas e modernas.',
    cls:'anim',
    color:{bg:'rgba(255,92,122,0.08)',border:'rgba(255,92,122,0.28)',tag:'rgba(255,92,122,0.15)',tagBorder:'rgba(255,92,122,0.3)',text:'#ff7a95',meta:'rgba(255,92,122,0.7)'},
    topics:[
      {id:'disney',name:'Disney & Pixar',icon:'🏰',xpReq:0,hint:'A Pixar é famosa por suas histórias emocionantes e personagens inesquecíveis!',hintImg:'🏰',
        questions:[
          {q:'Qual é o nome do peixe-palhaço protagonista de "Procurando Nemo"?',opts:['Marlin','Nemo','Dory','Gill'],a:0},
          {q:'Em "O Rei Leão", qual é o nome do tio vilão de Simba?',opts:['Scar','Mufasa','Rafiki','Timon'],a:0},
          {q:'Qual princesa da Disney beija um sapo e vira sapo também?',opts:['Ariel','Cinderela','Tiana','Belle'],a:2},
          {q:'Em "Toy Story", qual é o nome do cowboy?',opts:['Buzz','Woody','Rex','Hamm'],a:1},
          {q:'Qual estúdio criou os filmes "Up" e "Valente"?',opts:['DreamWorks','Sony Pictures','Pixar','Blue Sky'],a:2},
        ]},
      {id:'anime',name:'Animes Clássicos',icon:'⚔️',xpReq:0,hint:'Dragon Ball Z popularizou os animes no mundo inteiro nos anos 90!',hintImg:'⚔️',
        questions:[
          {q:'Em "Dragon Ball Z", qual é o nome da transformação mais icônica do Goku?',opts:['Ultra Instinct','Super Saiyajin','Kaioken','Fusion'],a:1},
          {q:'Em "Naruto", qual é o nome da aldeia natal do protagonista?',opts:['Vila da Folha','Vila da Areia','Vila da Névoa','Vila da Pedra'],a:0},
          {q:'Qual é o nome do protagonista de "Cavaleiros do Zodíaco"?',opts:['Shiryu','Hyoga','Seiya','Ikki'],a:2},
          {q:'Em "Pokémon", qual é o primeiro Pokémon da Pokédex Nacional?',opts:['Pikachu','Bulbasaur','Charmander','Mewtwo'],a:1},
          {q:'Qual personagem usa o Rasengan em "Naruto"?',opts:['Sasuke','Kakashi','Naruto','Jiraiya'],a:2},
        ]},
      {id:'cartoon',name:'Cartoons Ocidentais',icon:'📺',xpReq:100,hint:'Os Simpsons é o desenho animado de maior tempo no ar da história!',hintImg:'📺',
        questions:[
          {q:'Quem é o criador de "Os Simpsons"?',opts:['Seth MacFarlane','Matt Groening','Trey Parker','Mike Judge'],a:1},
          {q:'Em "Batman: A Série Animada", quem é a parceira do Coringa?',opts:['Arlequina','Venenosa','Mulher-Gato','Mulher-Aranha'],a:0},
          {q:'Qual é o nome do cachorro da família Jetson?',opts:['Rex','Astro','Dino','Pluto'],a:1},
          {q:'Em "Scooby-Doo", qual é o nome do dono do Scooby?',opts:['Fred','Shaggy','Salsicha','Barney'],a:1},
          {q:'Qual personagem vive em "Fenda do Bikini"?',opts:['Patrick','Sandy','Bob Esponja','Lula Molusco'],a:2},
        ]},
      {id:'studio_ghibli',name:'Studio Ghibli',icon:'🌿',xpReq:100,hint:'Hayao Miyazaki é considerado o "Walt Disney japonês"!',hintImg:'🌿',
        questions:[
          {q:'Quem dirigiu "Meu Vizinho Totoro"?',opts:['Isao Takahata','Makoto Shinkai','Hayao Miyazaki','Mamoru Hosoda'],a:2},
          {q:'Qual é o nome da protagonista de "A Viagem de Chihiro"?',opts:['Nausicaä','Chihiro','Kiki','Sophie'],a:1},
          {q:'Em "O Castelo Animado", qual feiticeira amaldiçoa Sophie?',opts:['Bruxa das Trevas','Bruxa das Pântanos','Bruxa do Leste','Bruxa do Norte'],a:1},
          {q:'Em "Princesa Mononoke", qual deus-animal central protege a floresta?',opts:['Moro','Okkoto','Kodama','O Deus Cervo'],a:3},
          {q:'Qual filme Ghibli retrata uma garota que se transforma em gato?',opts:['Kiki','Nausicaä','A Gatobela Yuki','O Caminho para Amalthea'],a:2},
        ]},
    ]
  },
  {
    id:'science', name:'Ciências', icon:'🔬',
    desc:'Biologia, física, química, astronomia e muito mais.',
    cls:'sci',
    color:{bg:'rgba(0,201,122,0.08)',border:'rgba(0,201,122,0.28)',tag:'rgba(0,201,122,0.12)',tagBorder:'rgba(0,201,122,0.3)',text:'#00e5a0',meta:'rgba(0,201,122,0.7)'},
    topics:[
      {id:'bio',name:'Biologia',icon:'🧬',xpReq:0,hint:'A mitocôndria é a organela responsável pela produção de energia celular.',hintImg:'🧬',
        questions:[
          {q:'Qual organela é responsável pela respiração celular?',opts:['Núcleo','Mitocôndria','Ribossomo','Retículo endoplasmático'],a:1},
          {q:'Quantos pares de cromossomos tem o ser humano?',opts:['21','23','24','46'],a:1},
          {q:'Qual processo converte luz solar em energia química nas plantas?',opts:['Respiração','Fermentação','Fotossíntese','Osmose'],a:2},
          {q:'Qual é o nome do maior osso do corpo humano?',opts:['Tíbia','Úmero','Fêmur','Rádio'],a:2},
          {q:'Qual molécula carrega informação genética?',opts:['ATP','RNA','DNA','Proteína'],a:2},
        ]},
      {id:'physics',name:'Física',icon:'⚡',xpReq:0,hint:'A velocidade da luz é de aproximadamente 300.000 km/s no vácuo!',hintImg:'⚡',
        questions:[
          {q:'Qual é a unidade de medida de força no SI?',opts:['Joule','Pascal','Newton','Watt'],a:2},
          {q:'Qual é a velocidade aproximada da luz no vácuo?',opts:['200 mil km/s','300 mil km/s','400 mil km/s','150 mil km/s'],a:1},
          {q:'Em qual estado da matéria as moléculas têm maior energia cinética?',opts:['Sólido','Líquido','Gasoso','Plasma'],a:2},
          {q:'O que mede um amperímetro?',opts:['Tensão','Resistência','Corrente elétrica','Potência'],a:2},
          {q:'Qual é a lei de Newton que diz "toda ação tem uma reação igual e oposta"?',opts:['1ª Lei','2ª Lei','3ª Lei','Lei da Gravitação'],a:2},
        ]},
      {id:'astro',name:'Astronomia',icon:'🌌',xpReq:200,hint:'Júpiter é tão grande que caberia mais de 1300 Terras dentro dele!',hintImg:'🪐',
        questions:[
          {q:'Qual é o maior planeta do Sistema Solar?',opts:['Saturno','Urano','Netuno','Júpiter'],a:3},
          {q:'Qual é a estrela mais próxima do Sol?',opts:['Sirius','Proxima Centauri','Vega','Betelgeuse'],a:1},
          {q:'Quantos planetas há no Sistema Solar atualmente?',opts:['7','8','9','10'],a:1},
          {q:'O que é um buraco negro?',opts:['Uma estrela gigante','Uma região com gravidade tão intensa que nada escapa','Um planeta morto','Uma nebulosa'],a:1},
          {q:'Qual missão levou o primeiro humano à Lua?',opts:['Apollo 10','Gemini 7','Apollo 11','Mercury 6'],a:2},
        ]},
      {id:'chem',name:'Química',icon:'⚗️',xpReq:200,hint:'O símbolo Au vem do latim "Aurum", que significa ouro!',hintImg:'⚗️',
        questions:[
          {q:'Qual é o símbolo químico do ouro?',opts:['Or','Go','Au','Ag'],a:2},
          {q:'Quantos elementos tem a tabela periódica atual?',opts:['108','112','118','124'],a:2},
          {q:'Qual é o pH de uma solução neutra?',opts:['0','7','14','5'],a:1},
          {q:'Qual gás é liberado na eletrólise da água?',opts:['CO₂ e N₂','H₂ e O₂','O₂ e CO','N₂ e H₂'],a:1},
          {q:'O que é um isótopo?',opts:['Elemento com prótons diferentes','Átomo com mesmo número de prótons mas nêutrons diferentes','Molécula com cargas iguais','Composto com mesmo ponto de fusão'],a:1},
        ]},
    ]
  },
  {
    id:'lang', name:'Linguagens', icon:'🗣️',
    desc:'Português, literaturas, idiomas e comunicação.',
    cls:'lang',
    color:{bg:'rgba(124,109,240,0.08)',border:'rgba(124,109,240,0.28)',tag:'rgba(124,109,240,0.12)',tagBorder:'rgba(124,109,240,0.3)',text:'#a89bf5',meta:'rgba(168,155,245,0.7)'},
    topics:[
      {id:'ptbr',name:'Português',icon:'📝',xpReq:0,hint:'O português é a 5ª língua mais falada do mundo, com mais de 260 milhões de falantes!',hintImg:'📝',
        questions:[
          {q:'Qual é a classe gramatical da palavra "rapidamente"?',opts:['Adjetivo','Advérbio','Substantivo','Pronome'],a:1},
          {q:'Qual figura de linguagem é usada em "O tempo é dinheiro"?',opts:['Metonímia','Hipérbole','Metáfora','Ironia'],a:2},
          {q:'Qual é o plural correto de "cidadão"?',opts:['Cidadões','Cidadãos','Cidadões','Cidadans'],a:1},
          {q:'O que é um oxímoro?',opts:['Repetição de sons','Combinação de ideias contraditórias','Diminuição exagerada','Inversão da ordem das palavras'],a:1},
          {q:'Qual é a forma verbal correta?',opts:['"Fazem dez anos"','"Faz dez anos"','As duas estão corretas','Nenhuma está correta'],a:1},
        ]},
      {id:'litbr',name:'Literatura Brasileira',icon:'📚',xpReq:0,hint:'Machado de Assis é considerado o maior nome da literatura brasileira.',hintImg:'📚',
        questions:[
          {q:'Quem escreveu "Dom Casmurro"?',opts:['José de Alencar','Machado de Assis','Clarice Lispector','Carlos Drummond'],a:1},
          {q:'Qual movimento literário foi iniciado pela Semana de Arte Moderna de 1922?',opts:['Romantismo','Realismo','Modernismo','Parnasianismo'],a:2},
          {q:'Quem é o autor de "Grande Sertão: Veredas"?',opts:['Guimarães Rosa','Graciliano Ramos','Jorge Amado','Érico Veríssimo'],a:0},
          {q:'Qual é o primeiro romance brasileiro, de José de Alencar?',opts:['O Guarani','Iracema','A Moreninha','Memórias de um Sargento de Milícias'],a:0},
          {q:'Clarice Lispector nasceu em qual país?',opts:['Brasil','Portugal','Ucrânia','Argentina'],a:2},
        ]},
      {id:'english',name:'Inglês',icon:'🇬🇧',xpReq:100,hint:'O inglês é a língua de negócios mais usada no mundo!',hintImg:'🇬🇧',
        questions:[
          {q:'What is the past tense of "go"?',opts:['Goed','Gone','Went','Going'],a:2},
          {q:'Which sentence is correct?',opts:['"She don\'t know"','"She doesn\'t know"','"She not know"','"She knows not"'],a:1},
          {q:'What does "ubiquitous" mean?',opts:['Rare','Found everywhere','Ancient','Unknown'],a:1},
          {q:'Which word is a synonym of "happy"?',opts:['Sad','Tired','Joyful','Angry'],a:2},
          {q:'What is the plural of "child"?',opts:['Childs','Childrens','Children','Childes'],a:2},
        ]},
      {id:'lingworld',name:'Línguas do Mundo',icon:'🌐',xpReq:100,hint:'O mandarim é a língua nativa mais falada, com mais de 900 milhões de falantes!',hintImg:'🌐',
        questions:[
          {q:'Qual é a língua mais falada do mundo como língua nativa?',opts:['Inglês','Espanhol','Mandarim','Hindi'],a:2},
          {q:'"Obrigado" em japonês é:',opts:['Arigatou','Konnichiwa','Sayonara','Gomen'],a:0},
          {q:'Quantas línguas oficiais tem a Suíça?',opts:['2','3','4','5'],a:2},
          {q:'Qual é a língua oficial do Brasil?',opts:['Espanhol','Português','Inglês','Francês'],a:1},
          {q:'O alfabeto árabe é escrito de qual direção?',opts:['Esquerda para direita','Direita para esquerda','De cima para baixo','De baixo para cima'],a:1},
        ]},
    ]
  },
  {
    id:'math', name:'Matemática', icon:'📐',
    desc:'Aritmética, álgebra, geometria, lógica e raciocínio matemático.',
    cls:'math',
    color:{bg:'rgba(255,209,102,0.08)',border:'rgba(255,209,102,0.28)',tag:'rgba(255,209,102,0.12)',tagBorder:'rgba(255,209,102,0.3)',text:'#ffd166',meta:'rgba(255,209,102,0.7)'},
    topics:[
      {id:'arith',name:'Aritmética',icon:'🔢',xpReq:0,hint:'A multiplicação nada mais é do que uma soma repetida!',hintImg:'🔢',
        questions:[
          {q:'Qual é o resultado de 17 × 13?',opts:['201','221','211','231'],a:1},
          {q:'Qual é a raiz quadrada de 196?',opts:['13','14','15','16'],a:1},
          {q:'Quanto é 15% de 340?',opts:['45','51','54','60'],a:1},
          {q:'Qual é o MMC de 12 e 18?',opts:['6','24','36','72'],a:2},
          {q:'Quanto é 2⁸?',opts:['128','256','512','64'],a:1},
        ]},
      {id:'geo',name:'Geometria',icon:'📏',xpReq:0,hint:'A soma dos ângulos internos de qualquer triângulo é sempre 180°!',hintImg:'📐',
        questions:[
          {q:'Quantos graus tem a soma dos ângulos internos de um triângulo?',opts:['90°','180°','270°','360°'],a:1},
          {q:'Qual é a fórmula da área do círculo?',opts:['2πr','πr²','πd','2πr²'],a:1},
          {q:'Quantos lados tem um hexágono?',opts:['5','6','7','8'],a:1},
          {q:'Em um triângulo retângulo, o lado oposto ao ângulo reto chama-se:',opts:['Cateto','Hipotenusa','Base','Apótema'],a:1},
          {q:'Qual é o volume de um cubo com aresta 3 cm?',opts:['9 cm³','18 cm³','27 cm³','81 cm³'],a:2},
        ]},
      {id:'algebra',name:'Álgebra',icon:'🔣',xpReq:200,hint:'A palavra "álgebra" vem do árabe "al-jabr", introduzida pelo matemático Al-Khwarizmi!',hintImg:'🔣',
        questions:[
          {q:'Qual é o valor de x em: 3x + 6 = 21?',opts:['3','4','5','6'],a:2},
          {q:'Qual é o resultado de (x + 3)² expandido?',opts:['x² + 9','x² + 3x + 9','x² + 6x + 9','x² + 6x + 3'],a:2},
          {q:'O que é uma função afim (do 1º grau)?',opts:['f(x) = ax²+ b','f(x) = ax + b','f(x) = a/x','f(x) = aˣ'],a:1},
          {q:'Qual é o discriminante (Δ) da equação 2x² + 3x - 2 = 0?',opts:['1','7','16','25'],a:3},
          {q:'Se f(x) = 2x - 1, qual é f(4)?',opts:['6','7','8','9'],a:1},
        ]},
      {id:'logic',name:'Lógica & Raciocínio',icon:'🧩',xpReq:200,hint:'A lógica matemática foi formalizada por George Boole no século XIX!',hintImg:'🧩',
        questions:[
          {q:'Qual é o próximo número da sequência: 2, 6, 12, 20, 30, __?',opts:['40','42','44','48'],a:1},
          {q:'Se todos os gatos são felinos e Mimi é um gato, então:',opts:['Mimi pode ser felino','Mimi é certamente felino','Mimi não é felino','Não é possível saber'],a:1},
          {q:'Quantos quadrados há em um tabuleiro de xadrez 8×8 (contando todos os tamanhos)?',opts:['64','128','204','256'],a:2},
          {q:'Qual é o valor de verdade de "P e não-P"?',opts:['Sempre verdadeiro','Sempre falso','Depende de P','Indeterminado'],a:1},
          {q:'Se hoje é quinta-feira, que dia será daqui a 100 dias?',opts:['Terça','Quarta','Quinta','Sexta'],a:1},
        ]},
    ]
  }
];

const CONQUISTAS_DATA = [
  {id:'first_quiz',icon:'🎯',name:'Primeiro Quiz',desc:'Complete seu primeiro quiz',check:()=>totalQuizzes>=1},
  {id:'ten_correct',icon:'⭐',name:'Dez Certo',desc:'Acerte 10 questões no total',check:()=>totalCorrect>=10},
  {id:'streak3',icon:'🔥',name:'Em Chamas',desc:'Acerte 3 questões seguidas',check:()=>streak>=3},
  {id:'xp100',icon:'💎',name:'Centenário',desc:'Alcance 100 XP',check:()=>xp>=100},
  {id:'xp500',icon:'👑',name:'Mestre',desc:'Alcance 500 XP',check:()=>xp>=500},
  {id:'all_cats',icon:'🌟',name:'Explorador',desc:'Jogue em todas as 4 categorias',check:()=>playedCats.size>=4},
  {id:'perfect',icon:'🏆',name:'Perfeito',desc:'Acerte 100% em um quiz',check:()=>hadPerfect},
  {id:'five_quizzes',icon:'🎮',name:'Maratonista',desc:'Complete 5 quizzes',check:()=>totalQuizzes>=5},
];

/* STATE */
let xp=0, curCat=null, curTopic=null, qIdx=0, correct=0, answered=false, roundXP=0;
let streak=0, totalCorrect=0, totalQuizzes=0, playedCats=new Set(), hadPerfect=false;
const done = new Set();
const LETTERS = ['A','B','C','D'];

function level()  { return Math.floor(xp/100)+1; }
function xpInLv() { return xp%100; }

/* ══════════════════════════════════
   NAVIGATION
══════════════════════════════════ */
function goToApp(catId) {
  updateLandingStats();
  const l = document.getElementById('screen-landing');
  l.classList.add('fade-out');
  setTimeout(() => {
    l.classList.remove('active');
    document.getElementById('screen-app').classList.add('active');
    updateNav();
    if (catId) {
      const cat = CATEGORIES.find(c => c.id === catId);
      if (cat) { curCat=cat; renderTopics(); showSub('topics'); updateSidebar(cat); }
      else renderHome();
    } else renderHome();
  }, 280);
}

function backToLanding() {
  updateLandingStats();
  const a = document.getElementById('screen-app');
  a.classList.add('fade-out');
  setTimeout(() => {
    a.classList.remove('active');
    document.getElementById('screen-landing').classList.add('active');
  }, 280);
}

function goToCat(catId) {
  const cat = CATEGORIES.find(c => c.id === catId);
  if (!cat) return;
  curCat = cat;
  updateSidebar(cat);
  renderTopics();
  showSub('topics');
}

function showConquistas() {
  clearSidebarActive();
  document.getElementById('sb-conquistas').classList.add('active-nav');
  renderConquistas();
  showSub('conquistas');
  hideTopic();
}

function showRanking() {
  clearSidebarActive();
  document.getElementById('sb-ranking').classList.add('active-nav');
  renderRanking();
  showSub('ranking');
  hideTopic();
}

function hideTopic() {
  document.getElementById('tb-topic-pill').style.display='none';
  document.getElementById('tb-streak').style.display='none';
}

function updateSidebar(cat) {
  clearSidebarActive();
  const map = {science:'sb-sci',math:'sb-math',lang:'sb-lang',anim:'sb-anim'};
  const el = document.getElementById(map[cat.id]);
  if (el) el.classList.add('active-cat', cat.cls);
}

function clearSidebarActive() {
  document.querySelectorAll('.sb-item').forEach(el => {
    el.classList.remove('active-cat','sci','math','lang','anim','active-nav');
  });
}

function showSub(id) {
  document.querySelectorAll('.sub').forEach(s => s.classList.remove('active'));
  document.getElementById('sub-'+id).classList.add('active');
  const backBtn = document.getElementById('nav-back-btn');
  if (id==='home') backBtn.classList.remove('visible');
  else backBtn.classList.add('visible');
}

function navBack() {
  const active = document.querySelector('.sub.active');
  if (!active) return;
  const id = active.id;
  if (id==='sub-topics') goHome();
  else if (id==='sub-quiz') goTopics();
  else if (id==='sub-result') goTopics();
  else goHome();
}

function goHome()   { clearSidebarActive(); hideTopic(); renderHome(); showSub('home'); }
function goTopics() { renderTopics(); showSub('topics'); }

/* ══════════════════════════════════
   UPDATE NAV/STATS
══════════════════════════════════ */
function updateNav() {
  document.getElementById('nav-level').textContent = 'Nível '+level();
  document.getElementById('xp-fill').style.width   = xpInLv()+'%';
  document.getElementById('xp-lbl').textContent    = xpInLv()+' / 100';
  document.getElementById('xp-total').textContent  = '💎 '+xp+' XP';
}

function updateLandingStats() {
  document.getElementById('ls-seq').textContent    = streak;
  document.getElementById('ls-acertos').textContent= totalCorrect;
  document.getElementById('ls-quizzes').textContent= totalQuizzes;
  document.getElementById('ls-xp').textContent     = xp+' XP';
  // category progress
  CATEGORIES.forEach(cat => {
    const total = cat.topics.reduce((s,t)=>s+t.questions.length,0);
    const doneQ = cat.topics.filter(t=>done.has(t.id)).reduce((s,t)=>s+t.questions.length,0);
    const pct = total>0 ? Math.round(doneQ/total*100) : 0;
    const key = cat.cls;
    const bar = document.getElementById('lpf-'+key);
    const num = document.getElementById('lp-'+key);
    if (bar) bar.style.width = pct+'%';
    if (num) num.textContent = doneQ+'/'+total;
  });
}

function toast(msg) {
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),1800);
}

/* ══════════════════════════════════
   RENDER HOME
══════════════════════════════════ */
function renderHome() {
  const grid = document.getElementById('cat-grid');
  grid.innerHTML='';
  document.getElementById('home-sub').textContent = xp>0
    ? xp+' XP · Nível '+level()+' · '+done.size+' temas concluídos'
    : 'responda questões · ganhe XP · desbloqueie temas';

  CATEGORIES.forEach(cat => {
    const c = cat.color;
    const allTopics = cat.topics;
    const unlockedCount = allTopics.filter(t=>xp>=t.xpReq).length;
    const doneCount = allTopics.filter(t=>done.has(t.id)).length;

    const card = document.createElement('div');
    card.className='cat-card '+cat.cls;

    const tagHtml = allTopics.map(t => {
      const locked = xp<t.xpReq;
      return `<span class="topic-tag" style="background:${locked?'rgba(255,255,255,0.04)':c.tag};border-color:${locked?'rgba(255,255,255,0.1)':c.tagBorder};color:${locked?'var(--muted)':c.text};${locked?'opacity:.5;text-decoration:line-through':''}">${t.icon} ${t.name}</span>`;
    }).join('');

    card.innerHTML=`
      <div class="cat-card-top">
        <div class="cat-card-icon">${cat.icon}</div>
        <div>
          <div class="cat-card-title ${cat.cls}">${cat.name}</div>
          <div class="cat-card-desc">${cat.desc}</div>
        </div>
      </div>
      <div class="cat-card-topics">${tagHtml}</div>
      <div class="cat-card-footer">
        <div class="cat-card-meta" style="color:${c.meta}">${unlockedCount}/${allTopics.length} desbloqueados · ${doneCount} concluídos</div>
        <div class="cat-card-arrow" style="color:${c.text}">→</div>
      </div>`;

    card.onclick=()=>{ curCat=cat; updateSidebar(cat); renderTopics(); showSub('topics'); };
    grid.appendChild(card);
  });
}

/* ══════════════════════════════════
   RENDER TOPICS
══════════════════════════════════ */
function renderTopics() {
  if (!curCat) return;
  const c = curCat.color;
  document.getElementById('topics-icon').textContent  = curCat.icon;
  document.getElementById('topics-title').textContent = curCat.name;
  document.getElementById('topics-desc').textContent  = curCat.topics.length+' temas · escolha um para jogar';
  document.getElementById('tb-topic-icon').textContent = curCat.icon;
  document.getElementById('tb-topic-name').textContent = curCat.name;
  document.getElementById('tb-topic-pill').style.display='flex';
  document.getElementById('tb-streak').style.display='flex';
  document.getElementById('tb-streak-val').textContent = streak;

  const grid = document.getElementById('topics-grid');
  grid.innerHTML='';
  curCat.topics.forEach(t => {
    const locked = xp<t.xpReq;
    const isDone = done.has(t.id);
    const card = document.createElement('div');
    card.className='topic-card'+(locked?' locked':'');
    if (!locked) card.style.borderColor=c.border;

    let badge = locked
      ? `<div class="topic-status s-locked">🔒 ${t.xpReq} xp</div>`
      : isDone ? `<div class="topic-status s-done">✓ feito</div>`
               : `<div class="topic-status s-new">novo</div>`;

    card.innerHTML=`${badge}
      <span class="topic-icon">${t.icon}</span>
      <div class="topic-name" style="color:${locked?'var(--muted)':c.text}">${t.name}</div>
      <div class="topic-meta">${t.questions.length} perguntas · +${t.questions.length*20} xp</div>
      ${locked?'<div class="lock-icon">🔒</div>':''}`;

    if (!locked) card.onclick=()=>startQuiz(t);
    grid.appendChild(card);
  });
}

/* ══════════════════════════════════
   QUIZ
══════════════════════════════════ */
function startQuiz(t) {
  curTopic=t; qIdx=0; correct=0; roundXP=0;
  if (curCat) playedCats.add(curCat.id);
  document.getElementById('quiz-topic-name').textContent=t.icon+' '+t.name;
  // set progress circle color
  const fill = document.getElementById('qpc-fill');
  fill.className='qpc-fill '+(curCat?curCat.cls:'sci');
  showSub('quiz');
  renderQ();
}

function retryQuiz() { startQuiz(curTopic); }

function renderQ() {
  const q = curTopic.questions[qIdx];
  answered=false;
  const tot = curTopic.questions.length;
  document.getElementById('q-num').textContent      = `Pergunta ${qIdx+1} de ${tot}`;
  document.getElementById('q-text').textContent     = q.q;
  document.getElementById('quiz-counter').textContent = `${qIdx+1} / ${tot}`;
  document.getElementById('q-progress').style.width   = (qIdx/tot*100)+'%';

  // Progress circle
  const pct = Math.round((qIdx/tot)*100);
  const circ = 2*Math.PI*46;
  const offset = circ - (circ*pct/100);
  document.getElementById('qpc-fill').style.strokeDashoffset=offset;
  document.getElementById('qpc-val').textContent=pct+'%';
  document.getElementById('quiz-prog-sub').textContent=qIdx+' / '+tot+' perguntas';

  // Hint
  const hintCard = document.getElementById('hint-card');
  if (curTopic.hint) {
    document.getElementById('hint-body').textContent=curTopic.hint;
    document.getElementById('hint-img').textContent=curTopic.hintImg||'';
    hintCard.style.display='block';
  } else { hintCard.style.display='none'; }

  const ol=document.getElementById('options');
  ol.innerHTML='';
  q.opts.forEach((opt,i)=>{
    const b=document.createElement('button');
    b.className='opt-btn';
    b.innerHTML=`<span class="opt-letter">${LETTERS[i]}</span>${opt}`;
    b.onclick=()=>pick(i,b);
    ol.appendChild(b);
  });

  const fb=document.getElementById('feedback');
  fb.className='feedback-strip'; fb.textContent='';
  const nb=document.getElementById('next-btn');
  nb.className='next-btn';
  nb.textContent=qIdx<curTopic.questions.length-1?'Próxima pergunta →':'Ver resultado →';
}

function pick(idx,btn) {
  if (answered) return;
  answered=true;
  const q=curTopic.questions[qIdx];
  document.querySelectorAll('.opt-btn').forEach(b=>b.disabled=true);
  const fb=document.getElementById('feedback');
  if (idx===q.a) {
    btn.classList.add('correct');
    correct++; roundXP+=20; xp+=20; totalCorrect++; streak++;
    if (streak>3) toast('🔥 Sequência de '+streak+'!');
    else toast('+20 XP ⚡');
    fb.textContent='✓ Resposta correta! +20 XP';
    fb.className='feedback-strip correct show';
    updateNav();
  } else {
    btn.classList.add('wrong');
    document.querySelectorAll('.opt-btn')[q.a].classList.add('correct');
    fb.textContent='✗ Incorreta. A resposta certa está destacada.';
    fb.className='feedback-strip wrong show';
    streak=0;
  }
  document.getElementById('tb-streak-val').textContent=streak;
  document.getElementById('next-btn').classList.add('show');
}

function nextQ() {
  qIdx++;
  if (qIdx>=curTopic.questions.length) showResult();
  else renderQ();
}

/* ══════════════════════════════════
   RESULT
══════════════════════════════════ */
function showResult() {
  done.add(curTopic.id);
  totalQuizzes++;
  const tot=curTopic.questions.length;
  const pct=Math.round(correct/tot*100);
  if (pct===100) hadPerfect=true;

  // Final progress circle
  const circ=2*Math.PI*46;
  document.getElementById('qpc-fill').style.strokeDashoffset=circ-(circ*pct/100);
  document.getElementById('qpc-val').textContent=pct+'%';
  document.getElementById('quiz-prog-sub').textContent=correct+' / '+tot+' perguntas';

  document.getElementById('r-correct').textContent=correct;
  document.getElementById('r-total').textContent  =tot;
  document.getElementById('r-pct').textContent    =pct+'%';
  document.getElementById('r-xp').textContent     ='+'+roundXP+' XP ganhos';

  const [emoji,title,sub]=pct===100
    ?['🏆','Perfeito!','Acertou todas as questões!']
    :pct>=80?['🎉','Muito bem!','Ótimo desempenho!']
    :pct>=60?['👍','Bom trabalho!','Continue praticando!']
    :['📚','Continue estudando!','Tente novamente para melhorar!'];

  document.getElementById('r-emoji').textContent=emoji;
  document.getElementById('r-title').textContent=title;
  document.getElementById('r-sub').textContent  =sub;

  const prevXP=xp-roundXP;
  const allTopics=CATEGORIES.flatMap(c=>c.topics);
  const unlocked=allTopics.filter(t=>t.xpReq>prevXP&&t.xpReq<=xp);
  const banner=document.getElementById('result-unlock');
  if (unlocked.length) {
    document.getElementById('unlock-msg').innerHTML=
      `<strong>Novo tema desbloqueado!</strong> Agora você pode jogar: `+
      unlocked.map(t=>t.icon+' '+t.name).join(', ');
    banner.classList.add('show');
  } else banner.classList.remove('show');

  showSub('result');
  updateLandingStats();
}

/* ══════════════════════════════════
   CONQUISTAS
══════════════════════════════════ */
function renderConquistas() {
  const grid=document.getElementById('conquistas-grid');
  grid.innerHTML='';
  CONQUISTAS_DATA.forEach(c=>{
    const unlocked=c.check();
    const card=document.createElement('div');
    card.style.cssText=`background:${unlocked?'rgba(255,255,255,.05)':'rgba(255,255,255,.02)'};border:1px solid ${unlocked?'rgba(255,209,102,.3)':'var(--border)'};border-radius:18px;padding:22px;text-align:center;transition:transform .2s;`;
    card.innerHTML=`
      <div style="font-size:44px;margin-bottom:12px;filter:${unlocked?'':'grayscale(1) opacity(.3)'}">${c.icon}</div>
      <div style="font-size:14px;font-weight:800;color:${unlocked?'var(--gold)':'var(--muted)'}">${c.name}</div>
      <div style="font-size:11px;color:var(--muted);margin-top:5px;font-family:var(--mono);line-height:1.5">${c.desc}</div>
      <div style="margin-top:10px;font-size:11px;font-weight:700;color:${unlocked?'var(--green)':'var(--muted)'}">${unlocked?'✓ Desbloqueada':'🔒 Bloqueada'}</div>`;
    grid.appendChild(card);
  });
}

/* ══════════════════════════════════
   RANKING
══════════════════════════════════ */
function renderRanking() {
  const el=document.getElementById('ranking-content');
  const rows=CATEGORIES.map(cat=>{
    const total=cat.topics.reduce((s,t)=>s+t.questions.length,0);
    const doneQ=cat.topics.filter(t=>done.has(t.id)).reduce((s,t)=>s+t.questions.length,0);
    const pct=total>0?Math.round(doneQ/total*100):0;
    return {cat,total,doneQ,pct};
  }).sort((a,b)=>b.pct-a.pct);

  el.innerHTML=rows.map((r,i)=>`
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:18px;padding:22px;margin-bottom:12px;display:flex;align-items:center;gap:18px;">
      <div style="font-size:28px;font-weight:900;color:${i===0?'var(--gold)':i===1?'rgba(255,255,255,.5)':i===2?'rgba(200,150,50,.8)':'var(--muted)'};font-family:var(--mono);width:36px;text-align:center">${i===0?'🥇':i===1?'🥈':i===2?'🥉':'#'+(i+1)}</div>
      <div style="font-size:36px">${r.cat.icon}</div>
      <div style="flex:1">
        <div style="font-size:16px;font-weight:900">${r.cat.name}</div>
        <div style="font-size:11px;color:var(--muted);font-family:var(--mono);margin-top:3px">${r.doneQ} / ${r.total} questões respondidas</div>
        <div style="margin-top:8px;height:5px;background:var(--surface2);border-radius:99px;overflow:hidden">
          <div style="height:100%;width:${r.pct}%;background:${r.pct>0?'linear-gradient(90deg,var(--accent),var(--green))':'transparent'};border-radius:99px;transition:width .8s ease"></div>
        </div>
      </div>
      <div style="font-size:22px;font-weight:900;font-family:var(--mono);color:${r.pct===100?'var(--green)':'var(--text)'}">${r.pct}%</div>
    </div>`).join('');
}

updateNav();
updateLandingStats();

/* ══════════════════════════════════════════════════════
   AUTH & ADMIN SYSTEM
   Dados persistidos no storage do artifact
══════════════════════════════════════════════════════ */

/* ── CREDENCIAIS ADMIN (altere aqui) ── */
const ADMIN_USER = 'admin';
const ADMIN_PASS = 'admin123';

/* ── STORAGE HELPERS ── */
async function dbGet(key) {
  try { const r = await window.storage.get(key); return r ? JSON.parse(r.value) : null; } catch { return null; }
}
async function dbSet(key, val) {
  try { await window.storage.set(key, JSON.stringify(val)); } catch(e) { console.warn('storage set failed',e); }
}

/* ── STATE ── */
let currentUser = null; // {username, type: 'user'|'guest'|'admin'}

/* ── UI HELPERS ── */
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => { s.classList.remove('active'); });
  document.getElementById('screen-' + id).classList.add('active');
}

function showLoginError(msg) {
  const el = document.getElementById('login-error');
  document.getElementById('login-error-msg').textContent = msg;
  el.classList.add('show');
  setTimeout(() => el.classList.remove('show'), 3500);
}

function switchTab(tab) {
  ['login','register','admin-login'].forEach(t => {
    document.getElementById('form-'+t.replace('-','_').replace('admin_login','admin-login')).style.display = 'none';
  });
  document.getElementById('form-login').style.display         = tab === 'login'        ? 'block' : 'none';
  document.getElementById('form-register').style.display      = tab === 'register'     ? 'block' : 'none';
  document.getElementById('form-admin-login').style.display   = tab === 'admin-login'  ? 'block' : 'none';
  document.querySelectorAll('.login-tab').forEach(t => t.classList.remove('active'));
  if (tab === 'login')    document.getElementById('tab-login').classList.add('active');
  if (tab === 'register') document.getElementById('tab-register').classList.add('active');
  document.getElementById('login-error').classList.remove('show');
}

function togglePass(id, btn) {
  const inp = document.getElementById(id);
  if (inp.type === 'password') { inp.type = 'text'; btn.textContent = '🙈'; }
  else { inp.type = 'password'; btn.textContent = '👁'; }
}

/* ── RECORD ACCESS ── */
async function recordAccess(username, type, action) {
  const log = (await dbGet('qm:access-log')) || [];
  log.unshift({ username, type, action, ts: new Date().toISOString() });
  if (log.length > 200) log.length = 200;
  await dbSet('qm:access-log', log);

  // update daily counters
  const today = new Date().toISOString().slice(0,10);
  const daily = (await dbGet('qm:daily')) || {};
  daily[today] = (daily[today] || 0) + 1;
  await dbSet('qm:daily', daily);

  // total access counter
  const totals = (await dbGet('qm:totals')) || { total:0, users:0, guests:0 };
  totals.total++;
  if (type === 'guest') totals.guests++; else totals.users++;
  await dbSet('qm:totals', totals);
}

/* ── REGISTER USERS LIST ── */
async function registerUserInDB(username, type) {
  const users = (await dbGet('qm:users')) || [];
  const exists = users.find(u => u.username === username);
  if (!exists) {
    users.push({ username, type, registeredAt: new Date().toISOString() });
    await dbSet('qm:users', users);
  }
}

/* ── ACTIONS ── */
async function doLogin() {
  const user = document.getElementById('li-user').value.trim();
  const pass = document.getElementById('li-pass').value;
  if (!user || !pass) { showLoginError('Preencha usuário e senha.'); return; }

  const users = (await dbGet('qm:accounts')) || {};
  if (!users[user]) { showLoginError('Usuário não encontrado. Cadastre-se primeiro!'); return; }
  if (users[user].pass !== pass) { showLoginError('Senha incorreta.'); return; }

  currentUser = { username: user, type: 'user' };
  await recordAccess(user, 'user', 'login');
  await registerUserInDB(user, 'user');
  goToLanding();
}

async function doRegister() {
  const user = document.getElementById('reg-user').value.trim();
  const pass = document.getElementById('reg-pass').value;
  if (!user) { showLoginError('Digite um nome de usuário.'); return; }
  if (pass.length < 4) { showLoginError('Senha deve ter pelo menos 4 caracteres.'); return; }
  if (user.toLowerCase() === ADMIN_USER.toLowerCase()) { showLoginError('Esse nome de usuário é reservado.'); return; }

  const users = (await dbGet('qm:accounts')) || {};
  if (users[user]) { showLoginError('Usuário já existe. Tente outro nome.'); return; }

  users[user] = { pass, registeredAt: new Date().toISOString() };
  await dbSet('qm:accounts', users);

  currentUser = { username: user, type: 'user' };
  await recordAccess(user, 'user', 'register');
  await registerUserInDB(user, 'user');
  goToLanding();
}

async function doGuest() {
  const guestId = 'visitante_' + Math.random().toString(36).slice(2,7);
  currentUser = { username: guestId, type: 'guest' };
  await recordAccess(guestId, 'guest', 'guest-access');
  goToLanding();
}

async function doAdminLogin() {
  const user = document.getElementById('adm-user').value.trim();
  const pass = document.getElementById('adm-pass').value;
  if (user !== ADMIN_USER || pass !== ADMIN_PASS) {
    showLoginError('Credenciais de admin incorretas.');
    return;
  }
  currentUser = { username: ADMIN_USER, type: 'admin' };
  await recordAccess(ADMIN_USER, 'admin', 'admin-login');
  openAdminPanel();
}

function goToLanding() {
  showScreen('landing');
  // update landing display name
  const pill = document.getElementById('admin-user-pill');
  if(pill) pill.textContent = currentUser.username;
  updateLandingStats();
}

function adminLogout() {
  currentUser = null;
  showScreen('login');
  switchTab('login');
}

/* ── OPEN ADMIN PANEL ── */
async function openAdminPanel() {
  document.getElementById('admin-username-display').textContent = currentUser.username;
  document.getElementById('admin-last-update').textContent = 'Última atualização: ' + new Date().toLocaleString('pt-BR');
  showScreen('admin');
  await renderAdminDashboard();
}

/* ── ADMIN DASHBOARD ── */
async function refreshAdmin() {
  document.getElementById('admin-last-update').textContent = 'Última atualização: ' + new Date().toLocaleString('pt-BR');
  await renderAdminDashboard();
}

async function renderAdminDashboard() {
  const log     = (await dbGet('qm:access-log')) || [];
  const daily   = (await dbGet('qm:daily')) || {};
  const totals  = (await dbGet('qm:totals')) || { total:0, users:0, guests:0 };
  const users   = (await dbGet('qm:users')) || [];
  const accounts= (await dbGet('qm:accounts')) || {};

  const totalAccess  = totals.total;
  const totalUsers   = Object.keys(accounts).length;
  const totalGuests  = totals.guests;
  const todayKey     = new Date().toISOString().slice(0,10);
  const todayCount   = daily[todayKey] || 0;
  const uniqueUsers  = users.filter(u => u.type !== 'guest').length;

  // KPIs
  const kpisEl = document.getElementById('admin-kpis');
  kpisEl.innerHTML = [
    { cls:'blue',  icon:'👥', val: totalAccess,  lbl:'Total de Acessos',    trend:'↑ desde o início',         trendCls:'up'  },
    { cls:'green', icon:'📅', val: todayCount,   lbl:'Acessos Hoje',        trend:'no dia de hoje',            trendCls:'neu' },
    { cls:'gold',  icon:'🧑', val: totalUsers,   lbl:'Usuários Cadastrados',trend:'contas registradas',        trendCls:'up'  },
    { cls:'red',   icon:'👤', val: totalGuests,  lbl:'Acessos Visitantes',  trend:'sem cadastro',              trendCls:'neu' },
    { cls:'purple',icon:'🏆', val: uniqueUsers,  lbl:'Usuários Ativos',     trend:'fizeram login ao menos 1x', trendCls:'up'  },
  ].map(k => `
    <div class="kpi-card ${k.cls}">
      <span class="kpi-icon">${k.icon}</span>
      <div class="kpi-val">${k.val.toLocaleString('pt-BR')}</div>
      <div class="kpi-lbl">${k.lbl}</div>
      <div class="kpi-trend ${k.trendCls}">${k.trend}</div>
    </div>`).join('');

  // CHART — last 7 days
  const days = [];
  for (let i = 6; i >= 0; i--) {
    const d = new Date(); d.setDate(d.getDate() - i);
    const key = d.toISOString().slice(0,10);
    const label = d.toLocaleDateString('pt-BR', { weekday:'short' }).slice(0,3);
    days.push({ key, label, count: daily[key] || 0 });
  }
  const maxCount = Math.max(...days.map(d => d.count), 1);
  document.getElementById('admin-chart-bars').innerHTML = days.map(d => {
    const h = Math.round((d.count / maxCount) * 100);
    return `<div class="admin-bar-col">
      <div class="admin-bar-val">${d.count}</div>
      <div class="admin-bar" style="height:${Math.max(h,4)}px"></div>
      <div class="admin-bar-lbl">${d.label}</div>
    </div>`;
  }).join('');

  // ACCESS LOG TABLE
  const tbody = document.getElementById('admin-log-body');
  if (log.length === 0) {
    tbody.innerHTML = `<tr><td colspan="5" class="admin-empty"><span class="admin-empty-icon">📭</span>Nenhum acesso registrado ainda.</td></tr>`;
  } else {
    tbody.innerHTML = log.slice(0, 50).map((entry, i) => {
      const dt = new Date(entry.ts).toLocaleString('pt-BR');
      const badgeCls = entry.type === 'admin' ? 'admin' : entry.type === 'guest' ? 'guest' : 'user';
      const actionLabel = {
        'login': '🔑 Login', 'register': '✨ Cadastro',
        'guest-access': '👤 Visitante', 'admin-login': '🔐 Admin Login'
      }[entry.action] || entry.action;
      return `<tr>
        <td style="font-family:var(--mono);color:var(--muted);font-size:11px">${log.length - i}</td>
        <td style="font-weight:700">${entry.username}</td>
        <td><span class="admin-badge ${badgeCls}">${entry.type}</span></td>
        <td style="font-family:var(--mono);font-size:11px;color:var(--muted)">${dt}</td>
        <td style="font-size:12px">${actionLabel}</td>
      </tr>`;
    }).join('');
  }

  // REGISTERED USERS
  const usersGrid = document.getElementById('admin-users-grid');
  const allAccounts = Object.entries(accounts).map(([u, v]) => ({ username: u, ...v }));
  document.getElementById('admin-users-count').textContent = allAccounts.length + ' usuários cadastrados';

  if (allAccounts.length === 0) {
    usersGrid.innerHTML = `<div class="admin-empty" style="grid-column:1/-1"><span class="admin-empty-icon">👥</span>Nenhum usuário cadastrado ainda.</div>`;
  } else {
    const colors = ['#7c6df0','#00e5a0','#ffd166','#ff5c7a','#ff9f43','#a89bf5'];
    usersGrid.innerHTML = allAccounts.map((u, i) => {
      const color = colors[i % colors.length];
      const initial = u.username[0].toUpperCase();
      const dt = u.registeredAt ? new Date(u.registeredAt).toLocaleDateString('pt-BR') : '—';
      return `<div class="admin-user-card">
        <div class="admin-user-avatar" style="background:${color}22;color:${color};border:2px solid ${color}44">${initial}</div>
        <div>
          <div class="admin-user-name">${u.username}</div>
          <div class="admin-user-meta">Cadastrado em ${dt}</div>
        </div>
      </div>`;
    }).join('');
  }
}

/* ── INIT ── */
(function init() {
  showScreen('login');
  switchTab('login');
})();
</script>
</body>
</html>
