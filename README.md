@@ -0,0 +1,1385 @@
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SIPAT Quiz 2026 — Balaska Equipamentos</title>

<!-- Fontes 100% offline: Impact (títulos) + Segoe UI (texto) -->

<style>
/* ============================================================
   TOKENS
   ============================================================ */
:root {
  --navy:   #040810;
  --navy2:  #07122A;
  --blue:   #0D3B8C;
  --blue2:  #1450B8;
  --gold:   #1A6FE8;
  --amber:  #0D3B8C;
  --green:  #00E676;
  --red:    #FF1744;
  --white:  #E8EEFF;
  --muted:  #4D6899;
  --card:   rgba(7,18,42,0.85);
  --border: rgba(20,80,184,0.35);
  --radius: 18px;
  --glow-g: 0 0 28px rgba(0,230,118,0.45);
  --glow-r: 0 0 28px rgba(255,23,68,0.45);
  --glow-y: 0 0 28px rgba(20,80,184,0.5);
  /* Balaska brand */
  --balaska-blue: #1450B8;
  --balaska-dark: #07122A;
  --accent: #FFFFFF;
}

/* ============================================================
   RESET / BASE
   ============================================================ */
/* CORREÇÃO 1: asterisco restaurado antes da vírgula */
*,::before,::after { margin:0; padding:0; box-sizing:border-box; }
html, body { width:100%; height:100%; overflow:hidden; }
body {
  font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
  background: var(--navy);
  color: var(--white);
  display: flex; align-items: center; justify-content: center;
}

/* ============================================================
   BACKGROUND — animated mesh + grid
   ============================================================ */
#bg {
  position: fixed; inset: 0; z-index: 0;
  background:
    radial-gradient(ellipse 80% 60% at 15% 20%, rgba(13,59,140,0.4) 0%, transparent 55%),
    radial-gradient(ellipse 60% 80% at 85% 75%, rgba(20,80,184,0.15) 0%, transparent 50%),
    radial-gradient(ellipse 100% 100% at 50% 50%, #040810 35%, #000408 100%);
}
#bg::after {
  content: ''; position: absolute; inset: 0;
  background-image:
    linear-gradient(rgba(20,80,184,0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(20,80,184,0.06) 1px, transparent 1px);
  background-size: 52px 52px;
}
.orb {
  position: fixed; border-radius: 50%; filter: blur(90px);
  pointer-events: none; z-index: 0; animation: orbDrift 18s ease-in-out infinite alternate;
}
.orb1 { width:500px; height:500px; background:rgba(13,59,140,0.22); top:-10%; left:-5%; }
.orb2 { width:400px; height:400px; background:rgba(20,80,184,0.12); bottom:-5%; right:-5%; animation-delay:-9s; }
@keyframes orbDrift {
  0%   { transform: translate(0,0) scale(1); }
  100% { transform: translate(40px,30px) scale(1.08); }
}

/* ============================================================
   SCREENS
   ============================================================ */
.screen {
  position: relative; z-index: 1;
  display: none; width: 100vw; height: 100vh;
  flex-direction: column; align-items: center; justify-content: center;
  padding: 24px; overflow: hidden;
}
.screen.active { display: flex; }

/* ============================================================
   KEYFRAMES
   ============================================================ */
@keyframes fadeUp   { from{opacity:0;transform:translateY(28px)} to{opacity:1;transform:translateY(0)} }
@keyframes fadeIn   { from{opacity:0} to{opacity:1} }
@keyframes scaleIn  { from{opacity:0;transform:scale(.7)} to{opacity:1;transform:scale(1)} }
@keyframes pulse    { 0%,100%{transform:scale(1)} 50%{transform:scale(1.06)} }
@keyframes shimmer  {
  0%   { background-position: 200% center; }
  100% { background-position: -200% center; }
}
@keyframes correctBurst {
  0%   { box-shadow: 0 0 0 0   rgba(0,230,118,0); }
  50%  { box-shadow: 0 0 0 20px rgba(0,230,118,0.25); }
  100% { box-shadow: 0 0 0 40px rgba(0,230,118,0); }
}
@keyframes wrongBurst {
  0%   { box-shadow: 0 0 0 0   rgba(255,23,68,0); }
  50%  { box-shadow: 0 0 0 20px rgba(255,23,68,0.25); }
  100% { box-shadow: 0 0 0 40px rgba(255,23,68,0); }
}
@keyframes countIn  { from{transform:scale(2.5);opacity:0} to{transform:scale(1);opacity:1} }
@keyframes rankSlide{ from{opacity:0;transform:translateX(-50px)} to{opacity:1;transform:translateX(0)} }
@keyframes trophy {
  0%   { transform: rotate(-10deg) scale(0.5); opacity:0; }
  60%  { transform: rotate(6deg) scale(1.2);   opacity:1; }
  100% { transform: rotate(0deg) scale(1);     opacity:1; }
}
@keyframes scoreCount { from{transform:scale(1.5);filter:brightness(2)} to{transform:scale(1);filter:brightness(1)} }

.au1{animation:fadeUp .5s .05s both}
.au2{animation:fadeUp .5s .15s both}
.au3{animation:fadeUp .5s .25s both}
.au4{animation:fadeUp .5s .35s both}
.au5{animation:fadeUp .5s .45s both}
.au6{animation:fadeUp .5s .55s both}

/* ============================================================
   LOGO
   ============================================================ */
.logo-ring {
  width:180px; height:80px; border-radius:14px;
  background: white;
  box-shadow: 0 0 0 3px var(--balaska-blue), 0 0 24px rgba(20,80,184,0.4);
  display: flex; align-items: center; justify-content: center;
  overflow: hidden; flex-shrink: 0;
  animation: pulse 3.5s ease-in-out infinite;
}
.logo-ring img { width:92%; height:92%; object-fit:contain; }
.logo-ring .logo-emoji { font-size: 42px; }

/* ============================================================
   SCREEN: START
   ============================================================ */
#screen-start .brand {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: clamp(15px,2vw,20px);
  letter-spacing: 5px; color: #ffffff;
  text-transform: uppercase; margin-top: 14px;
}
#screen-start .badge {
  background: linear-gradient(90deg,var(--balaska-blue),var(--navy2));
  border: 1px solid rgba(255,255,255,0.2);
  font-size: 11px; letter-spacing: 3px; color: #ffffff;
  padding: 4px 18px; border-radius: 20px; margin: 6px 0 20px;
  font-weight: 700; text-transform: uppercase;
}
.big-title {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: clamp(80px,13vw,140px);
  line-height: .9; letter-spacing: 6px;
  background: linear-gradient(135deg,#ffffff 0%,#7EB8FF 45%,var(--balaska-blue) 100%);
  background-size: 200% auto;
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  animation: shimmer 5s linear infinite, fadeUp .7s .1s both;
}
.big-sub {
  font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif; font-weight: 900;
  font-size: clamp(13px,1.8vw,17px); letter-spacing: 7px;
  color: var(--muted); text-transform: uppercase; margin: 6px 0 32px;
}
.stats-row { display:flex; gap:14px; flex-wrap:wrap; justify-content:center; margin-bottom:36px; }
.stat {
  background: var(--card); border: 1px solid var(--border);
  border-radius: 14px; padding: 14px 22px; text-align: center;
  backdrop-filter: blur(12px);
}
.stat b {
  display: block; font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: 38px; color: #7EB8FF; line-height: 1;
}
.stat span { font-size:10px; color:var(--muted); letter-spacing:1.5px; text-transform:uppercase; }

.btn {
  font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif; font-weight: 900;
  font-size: 18px; letter-spacing: 3px; text-transform: uppercase;
  border: none; border-radius: 50px; cursor: pointer;
  padding: 18px 60px; transition: transform .15s, box-shadow .15s;
}
.btn:hover  { transform: translateY(-2px) scale(1.04); }
.btn:active { transform: scale(.97); }
.btn-gold {
  background: linear-gradient(135deg,var(--balaska-blue),#1A6FE8);
  color: #ffffff; box-shadow: 0 8px 30px rgba(20,80,184,0.5);
  border: 1px solid rgba(255,255,255,0.15);
}
.btn-blue {
  background: linear-gradient(135deg,var(--blue),var(--blue2));
  color: white; box-shadow: 0 8px 30px rgba(17,85,204,0.4);
}
.btn-ghost {
  background: transparent; color: var(--muted);
  border: 1px solid rgba(255,255,255,0.1);
  padding: 12px 36px; font-size: 14px; letter-spacing: 2px;
}
.btn-ghost:hover { border-color:var(--gold); color:var(--gold); }

/* ============================================================
   SCREEN: REGISTER
   ============================================================ */
.glass-card {
  background: var(--card); backdrop-filter: blur(16px);
  border: 1px solid var(--border); border-radius: 24px;
  padding: 44px 40px; width: min(460px,90vw);
  animation: scaleIn .4s both;
}
.card-icon  { font-size:52px; margin-bottom:12px; display:block; text-align:center; }
.card-title {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: 38px;
  color: #7EB8FF; text-align: center; margin-bottom: 6px;
}
.card-sub   { color:var(--muted); font-size:14px; text-align:center; margin-bottom:28px; }
.field      { margin-bottom: 16px; }
#field-outro{ animation: fadeUp .3s both; }
.field label {
  display: block; font-size: 11px; font-weight: 700;
  letter-spacing: 2px; text-transform: uppercase;
  color: var(--muted); margin-bottom: 7px;
}
.field input, .field select {
  width: 100%; background: rgba(255,255,255,0.05);
  border: 1.5px solid rgba(255,255,255,0.1);
  border-radius: 12px; padding: 14px 16px;
  font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif; font-size: 15px;
  color: var(--white); outline: none; transition: border-color .2s;
}
.field input:focus, .field select:focus { border-color: #5BA3FF; }
.field select option { background: #0D2255; }

/* ============================================================
   SCREEN: NEXT PLAYER
   ============================================================ */
#screen-next { background: transparent; }
.next-label {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: clamp(22px,3vw,28px); letter-spacing: 6px;
  color: var(--muted); margin-bottom: 16px;
  animation: fadeIn .4s both;
}
.next-name {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: clamp(54px,9vw,96px);
  color: #7EB8FF; letter-spacing: 2px;
  animation: countIn .5s both;
  text-shadow: 0 0 28px rgba(20,80,184,0.6);
  text-align: center;
}
.next-dept {
  font-size: 16px; letter-spacing: 3px; color: var(--muted);
  text-transform: uppercase; margin-top: 8px;
  animation: fadeUp .4s .2s both;
}
.countdown-ring {
  width: 90px; height: 90px; margin-top: 36px;
  animation: fadeIn .3s .3s both;
}
.countdown-ring circle {
  fill: none; stroke-width: 5; stroke-linecap: round;
  transform: rotate(-90deg); transform-origin: 50% 50%;
}
.ring-bg   { stroke: rgba(255,255,255,0.08); }
.ring-fill { stroke: #5BA3FF; stroke-dasharray: 220; stroke-dashoffset: 0; transition: stroke-dashoffset .05s linear; }
.cd-num {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: 32px;
  fill: var(--white); dominant-baseline: middle; text-anchor: middle;
}

/* ============================================================
   SCREEN: GAME
   ============================================================ */
#screen-game { padding:0; justify-content:flex-start; overflow-y:auto; }

.game-header {
  width: 100%; background: rgba(6,12,30,.92);
  backdrop-filter: blur(14px);
  border-bottom: 2px solid rgba(255,214,0,0.15);
  padding: 12px 28px;
  display: flex; align-items: center; gap: 16px;
  position: sticky; top: 0; z-index: 20; flex-shrink: 0;
}
.gh-player  { display:flex; align-items:center; gap:10px; min-width:180px; }
.gh-avatar  {
  width:40px; height:40px; border-radius:50%;
  background: linear-gradient(135deg,var(--blue),var(--gold));
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; flex-shrink: 0;
}
.gh-name    { font-weight:700; font-size:16px; line-height:1.2; }
.gh-dept    { font-size:11px; color:var(--muted); }
.gh-progress{ flex:1; text-align:center; }
.gh-plabel  { font-size:11px; letter-spacing:2px; color:var(--muted); text-transform:uppercase; margin-bottom:5px; }
.prog-track { height:6px; background:rgba(255,255,255,0.08); border-radius:3px; overflow:hidden; }
.prog-bar   { height:100%; background:linear-gradient(90deg,var(--balaska-blue),#5BA3FF); border-radius:3px; transition:width .6s ease; }
.gh-score   {
  background: rgba(7,18,42,0.9); border: 1px solid rgba(20,80,184,0.45);
  border-radius: 12px; padding: 6px 18px; text-align: center; flex-shrink: 0;
}
.gh-score-num { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:26px; color:#7EB8FF; line-height:1; }
.gh-score-lbl { font-size:9px; color:var(--muted); letter-spacing:1px; text-transform:uppercase; }

.game-body {
  flex: 1; width: 100%; max-width: 820px; margin: 0 auto;
  padding: 28px 20px 20px;
  display: flex; flex-direction: column; align-items: center;
}

.cat-pill {
  font-size: 11px; font-weight: 700; letter-spacing: 3px; text-transform: uppercase;
  padding: 5px 16px; border-radius: 20px; margin-bottom: 10px;
}
.pill-seg { background:rgba(255,23,68,0.12);  color:#FF8A80; border:1px solid rgba(255,23,68,0.2); }
.pill-epi { background:rgba(0,230,118,0.10);  color:#69F0AE; border:1px solid rgba(0,230,118,0.18); }

.q-meta   { display:flex; align-items:center; gap:10px; margin-bottom:16px; }
.q-num    { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:15px; color:var(--muted); letter-spacing:2px; }
.diff-tag {
  font-size: 10px; font-weight: 700; letter-spacing: 2px;
  padding: 3px 12px; border-radius: 10px; text-transform: uppercase;
}
.easy { background:rgba(0,230,118,0.1);  color:#69F0AE; }
.hard { background:rgba(255,23,68,0.1);  color:#FF8A80; }

.q-card {
  background: var(--card); backdrop-filter: blur(12px);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: var(--radius); padding: 30px 32px;
  width: 100%; margin-bottom: 20px;
  animation: fadeUp .35s both;
}
.q-text { font-size:clamp(18px,2.5vw,23px); font-weight:700; line-height:1.35; color:var(--white); }

.opts { display:grid; grid-template-columns:1fr 1fr; gap:12px; width:100%; }
@media(max-width:560px){ .opts { grid-template-columns:1fr; } }

.opt {
  background: var(--card); backdrop-filter: blur(8px);
  border: 2px solid rgba(255,255,255,0.07);
  border-radius: 14px; padding: 16px 20px;
  cursor: pointer; transition: border-color .15s, background .15s, transform .12s;
  display: flex; align-items: center; gap: 14px;
  text-align: left; font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
  font-size: 14px; color: var(--white);
  animation: fadeUp .35s both;
}
.opt:nth-child(1){ animation-delay:.04s }
.opt:nth-child(2){ animation-delay:.08s }
.opt:nth-child(3){ animation-delay:.12s }
.opt:nth-child(4){ animation-delay:.16s }
.opt:hover:not(:disabled) {
  border-color: #5BA3FF; background: rgba(20,80,184,0.1); transform: translateY(-2px);
}
.opt:disabled { cursor: default; }
.opt-letter {
  width:34px; height:34px; border-radius:9px;
  background: rgba(255,255,255,0.08);
  display: flex; align-items: center; justify-content: center;
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: 18px; flex-shrink: 0;
  transition: background .15s;
}
.opt.correct              { border-color:var(--green); background:rgba(0,230,118,0.1); animation:correctBurst .5s ease; }
.opt.correct .opt-letter  { background:var(--green); color:var(--navy); }
.opt.wrong                { border-color:var(--red);   background:rgba(255,23,68,0.1);  animation:wrongBurst .5s ease; }
.opt.wrong .opt-letter    { background:var(--red); color:#fff; }
.opt.reveal-correct       { border-color:var(--green); background:rgba(0,230,118,0.06); }
.opt.reveal-correct .opt-letter { background:var(--green); color:var(--navy); }

.hint-area  { width:100%; margin-top:4px; }
.btn-hint {
  background: rgba(20,80,184,0.12); border: 1.5px solid rgba(91,163,255,0.3);
  border-radius: 50px; padding: 10px 26px;
  font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif; font-weight: 700; font-size: 13px;
  letter-spacing: 1.5px; text-transform: uppercase;
  color: #7EB8FF; cursor: pointer; transition: all .2s; display: block; margin: 0 auto;
}
.btn-hint:hover    { background: rgba(20,80,184,0.22); }
.btn-hint:disabled { opacity:.35; cursor:default; }
.hint-box {
  background: rgba(20,80,184,0.1); border: 1px solid rgba(91,163,255,0.25);
  border-radius: 12px; padding: 14px 20px; margin-top: 10px;
  font-size: 14px; color: #A8D4FF; line-height: 1.5; display: none;
  animation: fadeUp .3s both;
}

/* ============================================================
   FEEDBACK OVERLAY
   ============================================================ */
/* CORREÇÃO 2: comentário corrigido + display:none garantido */
#feedback-overlay {
  position: fixed; inset: 0; z-index: 100;
  display: none;
  flex-direction: column; align-items: center; justify-content: center;
  pointer-events: none; opacity: 0; transition: opacity .2s;
}
#feedback-overlay.show        { display:flex; opacity:1; }
.fb-emoji { font-size:110px; animation:scaleIn .35s both; display:block; }
.fb-word  {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: clamp(48px,8vw,80px);
  letter-spacing: 4px; margin-top: 8px;
  animation: fadeUp .3s .1s both;
}
#feedback-overlay.fb-correct              { background: rgba(0,230,118,0.12); }
#feedback-overlay.fb-wrong                { background: rgba(255,23,68,0.12); }
#feedback-overlay.fb-correct .fb-word    { color:var(--green); text-shadow:var(--glow-g); }
#feedback-overlay.fb-wrong   .fb-word    { color:var(--red);   text-shadow:var(--glow-r); }

/* ============================================================
   SCREEN: RESULT
   ============================================================ */
.result-card {
  background: var(--card); backdrop-filter: blur(18px);
  border: 1px solid var(--border); border-radius: 28px;
  padding: 48px 40px; width: min(500px,90vw); text-align: center;
  animation: scaleIn .5s both;
}
.res-trophy   { font-size:72px; display:block; margin-bottom:8px; animation:trophy .6s both; }
.res-name     { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:32px; color:#7EB8FF; margin-bottom:2px; }
.res-big {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: 90px; line-height: 1;
  background: linear-gradient(135deg,#ffffff,#7EB8FF);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  animation: scoreCount .6s both;
}
.res-pts-label{ font-size:12px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-bottom:6px; }
.res-acertos  { color:var(--muted); font-size:14px; margin-bottom:20px; }
.res-track    { background:rgba(255,255,255,0.06); border-radius:8px; height:10px; margin-bottom:24px; overflow:hidden; }
.res-bar      { height:100%; background:linear-gradient(90deg,var(--balaska-blue),#5BA3FF); border-radius:8px; transition:width 1.3s ease; width:0%; }
.res-msg      { font-size:20px; margin-bottom:28px; }
.btn-stack    { display:flex; flex-direction:column; gap:10px; }

/* ============================================================
   SCREEN: RANKING
   ============================================================ */
#screen-ranking { overflow-y:auto; justify-content:flex-start; padding-top:40px; }
.rank-header    { text-align:center; margin-bottom:32px; }
.rank-title {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif; font-size: clamp(56px,9vw,80px);
  letter-spacing: 5px; line-height: 1;
  background: linear-gradient(135deg,#ffffff,#7EB8FF);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.rank-sub  { font-size:12px; color:var(--muted); letter-spacing:3px; text-transform:uppercase; margin-top:4px; }
.rank-list { width:min(600px,90vw); }
.rank-item {
  background: var(--card); backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: 16px; padding: 18px 22px;
  display: flex; align-items: center; gap: 14px;
  margin-bottom: 10px;
  animation: rankSlide .4s both;
}
.rank-item:nth-child(1)  { border-color:rgba(91,163,255,0.4); background:rgba(20,80,184,0.1); animation-delay:.05s; }
.rank-item:nth-child(2)  { border-color:rgba(192,192,192,0.25); animation-delay:.1s; }
.rank-item:nth-child(3)  { border-color:rgba(205,127,50,0.25);  animation-delay:.15s; }
.rank-item:nth-child(n+4){ animation-delay:.2s; }
.rank-pos   { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:28px; width:40px; text-align:center; flex-shrink:0; }
.rank-pos.p1{ color:#7EB8FF; text-shadow:0 0 20px rgba(91,163,255,0.6); }
.rank-pos.p2{ color:#C0C0C0; }
.rank-pos.p3{ color:#CD7F32; }
.rank-pos.pN{ color:var(--muted); }
.rank-medal { font-size:26px; flex-shrink:0; }
.rank-info  { flex:1; }
.rank-name  { font-weight:700; font-size:17px; }
.rank-dept  { font-size:12px; color:var(--muted); margin-top:2px; }
.rank-right { text-align:right; }
.rank-score { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:32px; color:#7EB8FF; line-height:1; }
.rank-hits  { font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; }
.empty-rank { color:var(--muted); text-align:center; padding:60px 20px; font-size:18px; line-height:1.8; }

/* ============================================================
   SCREEN: BLOCK RANKING (mid-game)
   ============================================================ */
#screen-block-rank { overflow-y:auto; justify-content:flex-start; padding-top:32px; }
.br-header         { text-align:center; margin-bottom:24px; animation:fadeUp .5s both; }
.br-round-badge {
  display: inline-block;
  background: linear-gradient(90deg,var(--balaska-blue),var(--navy2));
  border: 1px solid rgba(255,255,255,0.2);
  font-size: 11px; letter-spacing: 3px; color: #ffffff;
  padding: 4px 18px; border-radius: 20px; margin-bottom: 10px;
  font-weight: 700; text-transform: uppercase;
}
.br-title {
  font-family: Impact, 'Arial Narrow', Arial, sans-serif;
  font-size: clamp(48px,7vw,70px); letter-spacing: 4px; line-height: 1;
  background: linear-gradient(135deg,#ffffff,#7EB8FF);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.br-sub { font-size:13px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-top:4px; }

.br-my-result {
  background: rgba(20,80,184,0.12); border: 1.5px solid rgba(91,163,255,0.4);
  border-radius: 20px; padding: 20px 28px; margin-bottom: 20px;
  display: flex; align-items: center; gap: 16px;
  width: min(580px,90vw); animation: scaleIn .5s .1s both;
}
.br-my-trophy { font-size:44px; flex-shrink:0; }
.br-my-info   { flex:1; }
.br-my-name   { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:26px; color:#7EB8FF; }
.br-my-pts    { font-size:14px; color:var(--muted); margin-top:2px; }
.br-my-score  { font-family:Impact, 'Arial Narrow', Arial, sans-serif; font-size:48px; color:#7EB8FF; line-height:1; text-shadow:0 0 24px rgba(91,163,255,0.5); }

.br-list { width:min(580px,90vw); margin-bottom:8px; }
.br-divider {
  font-size: 10px; font-weight: 700; letter-spacing: 3px; color: var(--muted);
  text-transform: uppercase; text-align: center; margin: 12px 0 10px;
  display: flex; align-items: center; gap: 10px;
}
.br-divider::before, .br-divider::after {
  content: ''; flex: 1; height: 1px; background: rgba(255,255,255,0.07);
}

.block-dots    { display:flex; gap:8px; justify-content:center; margin-bottom:20px; animation:fadeUp .4s .2s both; }
.block-dot     { width:10px; height:10px; border-radius:50%; background:rgba(255,255,255,0.12); transition:all .3s; }
.block-dot.done    { background: var(--green); }
.block-dot.current { background:#5BA3FF; box-shadow:0 0 12px rgba(91,163,255,0.6); transform:scale(1.3); }
.block-dot.upcoming{ background: rgba(255,255,255,0.12); }

.br-participants-label {
  font-size: 12px; letter-spacing: 2px; text-transform: uppercase;
  color: var(--muted); text-align: center; margin-bottom: 14px;
  padding: 8px 0; border-bottom: 1px solid rgba(255,255,255,0.06);
  width: 100%;
}
.rank-item-me {
  border-color: rgba(91,163,255,0.55) !important;
  background: rgba(20,80,184,0.18) !important;
}
.me-tag {
  display: inline-block;
  background: var(--balaska-blue);
  color: #fff; font-size: 9px; font-weight: 700;
  letter-spacing: 1.5px; padding: 2px 7px; border-radius: 6px;
  vertical-align: middle; margin-left: 6px;
}

/* scrollbar */
::-webkit-scrollbar       { width: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: rgba(20,80,184,0.5); border-radius: 3px; }

/* ============================================================
   AJUSTE OFFLINE — Impact como substituto de Bebas Neue
   ============================================================ */
/* Impact já é condensado por natureza — reduz letter-spacing nos títulos grandes */
.big-title    { letter-spacing: 2px !important; }
.rank-title   { letter-spacing: 2px !important; }
.br-title     { letter-spacing: 2px !important; }
.next-name    { letter-spacing: 0px !important; }
.res-big      { letter-spacing: 0px !important; }
.br-my-score  { letter-spacing: 0px !important; }
.card-title   { letter-spacing: 0px !important; }
.btn          { letter-spacing: 2px !important; }

</style>
</head>

<body>
<div id="bg"></div>
<div class="orb orb1"></div>
<div class="orb orb2"></div>

<!-- ===================== START ===================== -->
<div id="screen-start" class="screen active">
  <div class="logo-ring au1">
    <!--
      CORREÇÃO 3: substitua o src abaixo pelo caminho correto da logo na apresentação.
      O onerror garante fallback com emoji caso a imagem não carregue.
    -->
    <img src="logo-balaska.png" alt="Logotipo Balaska"
         onerror="this.style.display='none'; this.parentNode.innerHTML='<span class=logo-emoji>🏭</span>'">
  </div>
  <div class="brand au2">Balaska Equipamentos</div>
  <div class="badge au2">✦ SIPAT 2026 ✦</div>
  <div class="big-title">QUIZ</div>
  <div class="big-sub au3">Segurança do Trabalho</div>
  <div class="stats-row au4">
    <div class="stat"><b>50</b><span>Perguntas</span></div>
    <div class="stat"><b>10</b><span>Grupos</span></div>
    <div class="stat"><b>4</b><span>Alternativas</span></div>
    <div class="stat"><b>💡</b><span>Dica/Pergunta</span></div>
  </div>
  <button class="btn btn-gold au5" onclick="showRegister()">▶ JOGAR AGORA</button>
  <button class="btn btn-ghost au6" onclick="showRanking()" style="margin-top:12px">🏆 VER RANKING</button>
</div>

<!-- ===================== REGISTER ===================== -->
<div id="screen-register" class="screen">
  <div class="glass-card">
    <span class="card-icon">🦺</span>
    <div class="card-title">Quem é você?</div>
    <div class="card-sub">Cadastre-se para entrar no ranking da SIPAT!</div>
    <div class="field">
      <label for="inp-name">Nome completo</label>
      <input id="inp-name" type="text" placeholder="Ex: João da Silva" maxlength="40" autocomplete="off">
    </div>
    <div class="field">
      <label for="inp-dept">Setor / Departamento</label>
      <select id="inp-dept">
        <option value="">Selecione seu setor...</option>
        <option>Produção</option>
        <option>Manutenção</option>
        <option>Logística</option>
        <option>Administrativo</option>
        <option>Qualidade</option>
        <option>Segurança do Trabalho</option>
        <option>RH</option>
        <option>TI</option>
        <option>Vendas</option>
        <option>Compras</option>
        <option>Outro</option>
      </select>
    </div>
    <div class="field" id="field-outro" style="display:none">
      <label for="inp-outro">Qual é o seu setor?</label>
      <input id="inp-outro" type="text" placeholder="Digite o nome do seu setor..." maxlength="40" autocomplete="off">
    </div>
    <button class="btn btn-gold" style="width:100%;margin-top:4px" onclick="startGame()">COMEÇAR! 🚀</button>
    <button class="btn btn-ghost" style="width:100%;margin-top:10px" onclick="goTo('screen-start')">← Voltar</button>
  </div>
</div>

<!-- ===================== NEXT PLAYER ===================== -->
<div id="screen-next" class="screen">
  <div class="next-label">PRÓXIMO PARTICIPANTE</div>
  <div class="next-name" id="next-name">—</div>
  <div class="next-dept" id="next-dept">—</div>
  <svg class="countdown-ring" viewBox="0 0 90 90">
    <circle class="ring-bg"   cx="45" cy="45" r="35"/>
    <circle class="ring-fill" cx="45" cy="45" r="35" id="ring-fill"/>
    <text class="cd-num" x="45" y="45" id="cd-num">3</text>
  </svg>
</div>

<!-- ===================== GAME ===================== -->
<div id="screen-game" class="screen">
  <div class="game-header">
    <div class="gh-player">
      <div class="gh-avatar" id="gh-avatar">👷</div>
      <div>
        <div class="gh-name" id="gh-name">—</div>
        <div class="gh-dept" id="gh-dept">—</div>
      </div>
    </div>
    <div class="gh-progress">
      <div class="gh-plabel" id="gh-plabel">Pergunta 1 de 5</div>
      <div class="prog-track"><div class="prog-bar" id="prog-bar" style="width:2%"></div></div>
    </div>
    <div class="gh-score">
      <div class="gh-score-num" id="gh-score">0</div>
      <div class="gh-score-lbl">pontos</div>
    </div>
  </div>

  <div class="game-body">
    <div class="cat-pill" id="cat-pill">Grupo 1</div>
    <div class="q-meta">
      <div class="q-num" id="q-num">PERGUNTA 1</div>
      <div class="diff-tag easy" id="diff-tag">⚡ FÁCIL</div>
    </div>
    <div class="q-card">
      <div class="q-text" id="q-text">Carregando...</div>
    </div>
    <div class="opts" id="opts"></div>
    <div class="hint-area" style="margin-top:16px">
      <button class="btn-hint" id="btn-hint" onclick="useHint()">💡 Usar Dica</button>
      <div class="hint-box" id="hint-box"></div>
    </div>
  </div>
</div>

<!-- ===================== RESULT ===================== -->
<div id="screen-result" class="screen">
  <div class="result-card">
    <span class="res-trophy" id="res-trophy">🏆</span>
    <div class="res-name" id="res-name">—</div>
    <div class="res-big" id="res-big">0</div>
    <div class="res-pts-label">pontos</div>
    <div class="res-acertos" id="res-acertos">— acertos de 5 perguntas</div>
    <div class="res-track"><div class="res-bar" id="res-bar"></div></div>
    <div class="res-msg" id="res-msg">—</div>
    <div class="btn-stack">
      <button class="btn btn-blue" onclick="showRegister()">👤 Próximo Participante</button>
      <button class="btn btn-gold" onclick="showRanking()">🏆 Ver Ranking</button>
      <button class="btn btn-ghost" onclick="goTo('screen-start')">🏠 Início</button>
    </div>
  </div>
</div>

<!-- ===================== RANKING ===================== -->
<div id="screen-ranking" class="screen">
  <div class="rank-header au1">
    <div class="logo-ring" style="width:140px;height:60px;border-radius:10px;margin:0 auto 14px">
      <img src="logo-balaska.png" alt="Logotipo Balaska"
           onerror="this.innerHTML='<span class=logo-emoji>🏭</span>'">
    </div>
    <div class="rank-title">RANKING</div>
    <div class="rank-sub">✦ SIPAT 2026 — Balaska Equipamentos ✦</div>
  </div>
  <div class="rank-list" id="rank-list"></div>
  <button class="btn btn-gold" style="margin:28px 0 40px" onclick="goTo('screen-start')">🏠 Início</button>
</div>

<!-- ===================== BLOCK RANKING (mid-game) ===================== -->
<div id="screen-block-rank" class="screen">
  <div class="br-header">
    <div class="br-round-badge" id="br-badge">✦ BLOCO 1 CONCLUÍDO ✦</div>
    <div class="br-title">PLACAR</div>
    <div class="br-sub">Ranking parcial da rodada</div>
    <div class="block-dots" id="block-dots"></div>
  </div>
  <div class="br-my-result">
    <div class="br-my-trophy" id="br-my-trophy">🎯</div>
    <div class="br-my-info">
      <div class="br-my-name" id="br-my-name">—</div>
      <div class="br-my-pts" id="br-my-pts">— acertos neste bloco</div>
    </div>
    <div class="br-my-score" id="br-my-score">0</div>
  </div>
  <div class="br-list" id="br-list"></div>
  <button class="btn btn-gold" style="margin:4px 0 40px" id="br-continue-btn" onclick="continueAfterBlock()">
    ▶ PRÓXIMO PARTICIPANTE
  </button>
</div>

<!-- ===================== FEEDBACK OVERLAY ===================== -->
<div id="feedback-overlay">
  <span class="fb-emoji" id="fb-emoji"></span>
  <div class="fb-word" id="fb-word"></div>
</div>

<!-- ===================== SCRIPT ===================== -->
<script>
/* ============================================================
   QUESTIONS — 50 total, 10 grupos de 5 (3 fáceis + 2 difíceis)
   cat: 'seg' | 'epi'   diff: 'easy' | 'hard'
   ============================================================ */
const Q = [
  // ——— GRUPO 1 — Segurança do Trabalho ———
  {g:1,cat:'seg',diff:'easy',
   q:'O que significa a sigla NR no contexto de segurança do trabalho?',
   a:['Norma Regulamentadora','Nota de Risco','Nível de Risco','Norma de Responsabilidade'],
   c:0, h:'São criadas pelo Ministério do Trabalho e são obrigatórias para todas as empresas.'},
  {g:1,cat:'seg',diff:'easy',
   q:'Qual é a obrigação do trabalhador em relação ao EPI fornecido pela empresa?',
   a:['Guardar para usar só quando quiser','Usar corretamente e conservar','Devolver intacto após o turno','Comprar o próprio EPI'],
   c:1, h:'A lei prevê que o trabalhador deve usar, guardar e zelar pelo equipamento.'},
  {g:1,cat:'seg',diff:'easy',
   q:'O que deve ser feito IMEDIATAMENTE após um acidente de trabalho?',
   a:['Esperar o turno acabar para relatar','Comunicar o supervisor e prestar socorro','Continuar trabalhando normalmente','Ligar para casa antes de qualquer coisa'],
   c:1, h:'A comunicação imediata é fundamental para o registro correto e assistência médica.'},
  {g:1,cat:'seg',diff:'hard',
   q:'Qual NR regulamenta especificamente os serviços em instalações elétricas?',
   a:['NR-10','NR-12','NR-35','NR-18'],
   c:0, h:'Esta NR trata de segurança em instalações e serviços em eletricidade.'},
  {g:1,cat:'seg',diff:'hard',
   q:'O que é a CIPA e qual seu principal objetivo?',
   a:['Controle Interno de Produção e Acidentes','Comissão Interna de Prevenção de Acidentes','Central de Informações para Prevenção de Acidentes','Comissão Interna de Processos e Atividades'],
   c:1, h:'É formada por representantes da empresa e dos empregados — regulada pela NR-5.'},

  // ——— GRUPO 2 — EPI e EPC ———
  {g:2,cat:'epi',diff:'easy',
   q:'O que significa a sigla EPI?',
   a:['Equipamento de Proteção Individual','Equipamento de Prevenção Interna','Equipamento Padrão de Inspeção','Equipamento de Produção Industrial'],
   c:0, h:'A palavra "Individual" indica que é de uso pessoal e intransferível.'},
  {g:2,cat:'epi',diff:'easy',
   q:'O capacete de segurança protege principalmente contra qual risco?',
   a:['Riscos químicos','Impactos e quedas de objetos sobre a cabeça','Choques elétricos apenas','Ruído excessivo'],
   c:1, h:'Pense na parte do corpo que ele cobre e nos riscos mecânicos.'},
  {g:2,cat:'epi',diff:'easy',
   q:'Qual EPI é utilizado para proteção dos olhos contra respingos químicos?',
   a:['Óculos escuros comuns','Óculos de proteção com vedação lateral','Protetor facial simples','Máscara facial inteira'],
   c:1, h:'Deve vedar completamente a área dos olhos — não pode ter frestas.'},
  {g:2,cat:'epi',diff:'hard',
   q:'O que é o CA (Certificado de Aprovação) em um EPI?',
   a:['Código de Armazenamento do equipamento','Certificado emitido pelo MTE aprovando o EPI para uso','Controle de Avarias do equipamento','Código de Autorização de uso interno'],
   c:1, h:'Sem CA, o EPI não pode ser legalmente fornecido pela empresa ao trabalhador.'},
  {g:2,cat:'epi',diff:'hard',
   q:'Qual a diferença fundamental entre EPI e EPC?',
   a:['EPI é mais caro que EPC','EPI protege individualmente; EPC protege coletivamente','EPC é obrigatório, EPI é opcional','Não há diferença prática entre eles'],
   c:1, h:'O "C" em EPC significa Coletivo — protege todos sem necessidade de uso individual.'},

  // ——— GRUPO 3 — Segurança do Trabalho ———
  {g:3,cat:'seg',diff:'easy',
   q:'O que é um mapa de risco?',
   a:['Planta baixa da fábrica apenas','Representação gráfica dos riscos no ambiente de trabalho','Lista dos funcionários em áreas de risco','Documento de seguro contra acidentes'],
   c:1, h:'Geralmente exibido em planta com círculos coloridos e de tamanhos diferentes.'},
  {g:3,cat:'seg',diff:'easy',
   q:'Qual a cor padrão usada para identificar equipamentos de combate a incêndio?',
   a:['Amarelo','Verde','Vermelho','Azul'],
   c:2, h:'É a mesma cor dos extintores — cor de perigo e urgência.'},
  {g:3,cat:'seg',diff:'easy',
   q:'O que significa a sinalização de "saída de emergência" na cor verde?',
   a:['Área contaminada','Rota segura de fuga e evacuação','Localização de equipamentos elétricos','Área de descanso'],
   c:1, h:'O verde em segurança representa segurança, saída e primeiros socorros.'},
  {g:3,cat:'seg',diff:'hard',
   q:'Qual o prazo para o empregador emitir a CAT (Comunicação de Acidente de Trabalho)?',
   a:['30 dias corridos','15 dias úteis','No primeiro dia útil após o acidente','7 dias corridos'],
   c:2, h:'A lei exige emissão até o 1º dia útil após a ocorrência do acidente.'},
  {g:3,cat:'seg',diff:'hard',
   q:'O que é APR (Análise Preliminar de Riscos)?',
   a:['Avaliação Periódica de Resultados de segurança','Técnica preventiva para identificar riscos ANTES de iniciar uma atividade','Relatório mensal de acidentes do trabalho','Auditoria de Procedimentos de Risco'],
   c:1, h:'É feita ANTES de começar a tarefa — identifica perigos e define controles.'},

  // ——— GRUPO 4 — EPI e EPC ———
  {g:4,cat:'epi',diff:'easy',
   q:'Qual EPI usar ao trabalhar com produtos químicos que oferecem risco de inalação?',
   a:['Apenas óculos de proteção','Respirador ou máscara adequada ao agente químico','Luvas de borracha','Protetor auricular'],
   c:1, h:'O risco é de inalação — proteja as vias respiratórias.'},
  {g:4,cat:'epi',diff:'easy',
   q:'Para que serve o cinto de segurança tipo paraquedas (talabarte)?',
   a:['Proteção contra quedas em trabalho em altura','Suporte lombar em trabalho físico pesado','Proteção contra choques elétricos','Imobilização do tronco em acidentes'],
   c:0, h:'Regulamentado pela NR-35 — trabalho em altura acima de 2 metros.'},
  {g:4,cat:'epi',diff:'easy',
   q:'O protetor auricular protege o trabalhador de qual risco?',
   a:['Risco químico','Risco biológico','Risco físico — ruído excessivo','Risco ergonômico'],
   c:2, h:'Pense no órgão do sentido que ele protege — o ouvido.'},
  {g:4,cat:'epi',diff:'hard',
   q:'O extintor de CO₂ (gás carbônico) é indicado para qual(is) classe(s) de incêndio?',
   a:['Classe A — materiais sólidos','Classes B e C — líquidos inflamáveis e equipamentos elétricos','Classe D — metais combustíveis','Classe K — óleos e gorduras em fritadeiras'],
   c:1, h:'Não deixa resíduo e não conduz eletricidade — ótimo para equipamentos elétricos.'},
  {g:4,cat:'epi',diff:'hard',
   q:'O que é o EPC "guarda-corpo" e quando é obrigatório?',
   a:['Proteção individual para vigilantes','Barreira física coletiva em aberturas e bordas a partir de 2m de altura','Equipamento usado em trabalho noturno','Nome informal do cinto de segurança'],
   c:1, h:'É uma barreira física instalada para proteger TODOS — não precisa ser colocada individualmente.'},

  // ——— GRUPO 5 — Segurança do Trabalho ———
  {g:5,cat:'seg',diff:'easy',
   q:'O que é o PPRA — Programa de Prevenção de Riscos Ambientais?',
   a:['Programa de pagamento por adicionais de risco','Programa obrigatório para identificar e controlar riscos no ambiente de trabalho','Programa de premiação por acidente zero','Plano de Proteção e Reciclagem Ambiental'],
   c:1, h:'Hoje substituído pelo PGR (NR-1/2021), mas o conceito de identificar e controlar riscos permanece.'},
  {g:5,cat:'seg',diff:'easy',
   q:'Qual o principal objetivo das sinalizações de segurança nas empresas?',
   a:['Decorar o ambiente de trabalho','Alertar, orientar e prevenir acidentes','Atender exigências estéticas da gestão','Indicar apenas banheiros e refeitório'],
   c:1, h:'A sinalização comunica perigos, orienta condutas e indica equipamentos de emergência.'},
  {g:5,cat:'seg',diff:'easy',
   q:'Trabalho em altura, conforme a NR-35, é aquele realizado acima de quantos metros?',
   a:['1 metro','2 metros','3 metros','5 metros'],
   c:1, h:'Este é o limite exato definido pela NR-35.'},
  {g:5,cat:'seg',diff:'hard',
   q:'O que é o LTCAT — Laudo Técnico das Condições do Ambiente de Trabalho?',
   a:['Licença para Trabalho em Condições de Alto Risco','Laudo que avalia agentes nocivos para fins previdenciários (aposentadoria especial)','Lista de Técnicos Certificados em Ambiente de Trabalho','Laudo de Treinamento e Capacitação Técnica'],
   c:1, h:'Tem relação direta com a aposentadoria especial do INSS.'},
  {g:5,cat:'seg',diff:'hard',
   q:'Na hierarquia de controle de riscos, qual medida tem MAIOR prioridade?',
   a:['Uso de EPI','Procedimentos administrativos e treinamentos','Eliminação do risco na fonte','Sinalização de advertência'],
   c:2, h:'A hierarquia vai do mais ao menos eficaz — eliminar é sempre melhor do que proteger.'},

  // ——— GRUPO 6 — EPI e EPC ———
  {g:6,cat:'epi',diff:'easy',
   q:'Luvas de raspa (couro) são indicadas para proteção contra qual tipo de risco?',
   a:['Produtos químicos corrosivos','Abrasão, cortes e calor moderado','Choques elétricos de alta tensão','Agentes biológicos e microbiológicos'],
   c:1, h:'São feitas de couro bovino — resistentes ao atrito e pequenos cortes.'},
  {g:6,cat:'epi',diff:'easy',
   q:'A botina de segurança com biqueira de aço protege principalmente contra:',
   a:['Quedas de altura','Queda de objetos pesados sobre os pés','Choques elétricos','Escorregamentos em piso úmido'],
   c:1, h:'Pense na parte reforçada da botina — a biqueira cobre os dedos.'},
  {g:6,cat:'epi',diff:'easy',
   q:'O que diferencia um EPC de um EPI na prática?',
   a:['EPC é mais resistente que EPI','EPC protege vários trabalhadores ao mesmo tempo, sem uso individual','EPC é de uso pessoal e intransferível','EPI é instalado permanentemente no ambiente'],
   c:1, h:'Exemplos de EPC: tela de proteção, guarda-corpo, sinalização, chuveiro lava-olhos.'},
  {g:6,cat:'epi',diff:'hard',
   q:'Qual norma regulamentadora trata especificamente do EPI?',
   a:['NR-6','NR-9','NR-15','NR-17'],
   c:0, h:'É uma das NRs mais aplicadas e tem número baixo.'},
  {g:6,cat:'epi',diff:'hard',
   q:'Em qual situação o empregador pode ser autuado por não cumprir obrigações quanto ao EPI?',
   a:['Apenas quando há acidente registrado','Quando não fornece, não orienta e não fiscaliza o uso correto','Somente durante fiscalização do MTE','Quando o trabalhador solicitar formalmente por escrito'],
   c:1, h:'A responsabilidade do empregador é tríplice: fornecer, treinar E fiscalizar o uso.'},

  // ——— GRUPO 7 — Segurança do Trabalho ———
  {g:7,cat:'seg',diff:'easy',
   q:'O que é o procedimento de "bloqueio e etiquetagem" (Lockout/Tagout)?',
   a:['Sistema de ponto eletrônico para operadores','Procedimento para isolar fontes de energia antes de manutenção','Método de controle de qualidade de produto','Sistema de alarme de incêndio automático'],
   c:1, h:'Protege quem realiza manutenção de equipamentos de energização acidental.'},
  {g:7,cat:'seg',diff:'easy',
   q:'Qual o número do SAMU no Brasil?',
   a:['190','192','193','199'],
   c:1, h:'É um número de três dígitos — essencial memorizar em emergências.'},
  {g:7,cat:'seg',diff:'easy',
   q:'O que deve conter obrigatoriamente uma caixa de primeiros socorros?',
   a:['Apenas curativos e álcool 70%','Itens básicos: bandagens, antisséptico, tesoura, luvas descartáveis','Medicamentos para dor e anti-inflamatórios','Desfibrilador portátil (DEA)'],
   c:1, h:'Sem medicamentos controlados — foco em cuidados imediatos.'},
  {g:7,cat:'seg',diff:'hard',
   q:'Qual o objetivo principal do PCMSO — Programa de Controle Médico de Saúde Ocupacional?',
   a:['Controlar e reduzir faltas médicas dos funcionários','Promover e preservar a saúde dos trabalhadores com exames periódicos','Gerenciar o plano de saúde corporativo','Controlar os custos com afastamentos e licenças médicas'],
   c:1, h:'Exigido pela NR-7 — inclui exames admissional, periódico, retorno e demissional.'},
  {g:7,cat:'seg',diff:'hard',
   q:'O que é "insalubridade" no contexto trabalhista?',
   a:['Trabalho perigoso com risco direto de morte','Exposição a agentes nocivos à saúde acima dos limites de tolerância','Trabalho físico pesado além da capacidade humana','Ambiente de trabalho desconfortável ou desorganizado'],
   c:1, h:'Gera adicional salarial e é regulado pela NR-15 — agentes químicos, físicos e biológicos.'},

  // ——— GRUPO 8 — EPI e EPC ———
  {g:8,cat:'epi',diff:'easy',
   q:'Para que serve a máscara PFF2 (respirador de partículas)?',
   a:['Proteção contra gases e vapores orgânicos','Filtração de partículas sólidas e aerossóis líquidos','Proteção exclusiva contra poeira de cimento','Uso exclusivo em áreas hospitalares e de saúde'],
   c:1, h:'O "P" de PFF significa "Peça Facial Filtrante" — foca em partículas.'},
  {g:8,cat:'epi',diff:'easy',
   q:'Qual a função da proteção lateral nos óculos de segurança?',
   a:['Melhorar a visão periférica do operador','Proteger os olhos de partículas vindas de qualquer direção','Proteção apenas contra luz intensa e UV','Uso obrigatório apenas em trabalho noturno'],
   c:1, h:'A proteção lateral complementa a frontal — partículas vêm de vários ângulos.'},
  {g:8,cat:'epi',diff:'easy',
   q:'Qual EPC é usado para isolar fisicamente uma área de obra ou risco?',
   a:['Cone de sinalização e fita zebrada','Capacete de segurança do operador','Extintor de incêndio posicionado no local','Cinto de segurança do trabalhador'],
   c:0, h:'Você já viu esses itens em obras na rua — impedem a entrada de pessoas não autorizadas.'},
  {g:8,cat:'epi',diff:'hard',
   q:'O que é o NRRsf — nível de atenuação de ruído — em protetores auriculares?',
   a:['Nível de Resistência ao Risco em situação física','Medida que indica quanto o protetor reduz o ruído que chega ao ouvido','Norma de Regulamentação de Ruído seguro para indústrias','Número de Registro do equipamento no INMETRO'],
   c:1, h:'Quanto maior o NRRsf, mais o protetor atenua o som — usado para selecionar o EPI correto.'},
  {g:8,cat:'epi',diff:'hard',
   q:'Em trabalho de soldagem, quais EPIs são tipicamente necessários?',
   a:['Apenas avental de couro e luvas finas','Máscara de solda, avental de raspa, luvas de raspa, perneiras e botina','Somente óculos escuros e luvas de borracha','Respirador e óculos de proteção transparente'],
   c:1, h:'Soldagem envolve radiação UV, fagulhas, respingos de metal, fumos e calor intenso.'},

  // ——— GRUPO 9 — Segurança do Trabalho ———
  {g:9,cat:'seg',diff:'easy',
   q:'O que é incêndio de Classe A?',
   a:['Incêndio em líquidos inflamáveis','Incêndio em materiais sólidos que deixam brasas (madeira, papel, tecido)','Incêndio em equipamentos elétricos energizados','Incêndio em metais combustíveis como magnésio'],
   c:1, h:'É o tipo mais comum — pense em fogueira ou papel queimando.'},
  {g:9,cat:'seg',diff:'easy',
   q:'Por que NÃO se deve usar água para apagar incêndio Classe C (elétrico)?',
   a:['A água não apaga fogo em equipamentos elétricos','A água conduz eletricidade e pode eletrocutar o combatente','A água estraga irreparavelmente os equipamentos elétricos','Não há restrição ao uso de água em incêndio elétrico'],
   c:1, h:'Pense nas propriedades físicas da água — ela conduz corrente elétrica.'},
  {g:9,cat:'seg',diff:'easy',
   q:'O que representa o "Triângulo do Fogo"?',
   a:['Sinalização triangular de área de incêndio','Os três elementos necessários para o fogo existir: combustível, comburente e calor','Os três tipos de extintores mais utilizados na indústria','As três etapas do combate a incêndio'],
   c:1, h:'Retire qualquer um dos vértices e o fogo se apaga — esse é o princípio do combate.'},
  {g:9,cat:'seg',diff:'hard',
   q:'De acordo com a norma, qual a periodicidade mínima obrigatória para recarga de extintores?',
   a:['1 ano','2 anos','5 anos','Depende do fabricante, sem prazo legal definido'],
   c:0, h:'A recarga anual é obrigatória — além da inspeção visual que deve ser feita mensalmente.'},
  {g:9,cat:'seg',diff:'hard',
   q:'O que é "Permissão de Trabalho" (PT) em segurança do trabalho?',
   a:['Documento de admissão do trabalhador na função','Documento formal que autoriza e controla tarefas de alto risco com medidas de segurança','Autorização formal para realização de horas extras','Licença para trabalhar em regime de turno noturno'],
   c:1, h:'Emitida antes de atividades como espaço confinado, trabalho a quente e trabalho em altura.'},

  // ——— GRUPO 10 — EPI e EPC ———
  {g:10,cat:'epi',diff:'easy',
   q:'Qual o EPI correto para proteger as mãos ao manusear objetos cortantes?',
   a:['Luvas de látex descartáveis','Luvas de malha de aço ou raspa de couro resistente a corte','Luvas cirúrgicas de procedimento','Qualquer luva grossa disponível'],
   c:1, h:'Precisa ser resistente ao corte certificado — luva comum pode ser perfurada facilmente.'},
  {g:10,cat:'epi',diff:'easy',
   q:'O colete refletivo é utilizado principalmente para:',
   a:['Proteção contra chuva e intempéries','Aumentar a visibilidade do trabalhador em locais de baixa luminosidade ou tráfego de veículos','Proteção contra calor radiante de fornos','Identificação exclusiva de visitantes'],
   c:1, h:'Reflete a luz — essencial em trabalho noturno ou com movimentação de veículos.'},
  {g:10,cat:'epi',diff:'easy',
   q:'Qual EPC protege trabalhadores de quedas em aberturas no piso e bordas elevadas?',
   a:['Apenas fita zebrada de sinalização','Guarda-corpo e grades de proteção fixas','Cone de sinalização laranja','Somente cinto de segurança individual'],
   c:1, h:'Deve ser uma barreira física resistente — não apenas uma sinalização visual.'},
  {g:10,cat:'epi',diff:'hard',
   q:'O que é "higienização de EPI" e por que é importante?',
   a:['Processo de descarte correto e seguro após o uso','Limpeza e descontaminação para manter eficiência protetora e saúde do usuário','Procedimento administrativo para devolver EPI danificado','Registro fotográfico do estado de conservação do equipamento'],
   c:1, h:'EPI sujo pode perder eficiência protetora ou contaminar o usuário com o agente de risco.'},
  {g:10,cat:'epi',diff:'hard',
   q:'Qual a responsabilidade legal do empregador quando o trabalhador se recusa a usar o EPI?',
   a:['O empregador fica automaticamente isento de qualquer responsabilidade','Deve advertir, registrar por escrito e aplicar medidas disciplinares, comprovando a exigência','Deve demitir o trabalhador por justa causa imediatamente','Não há obrigação legal específica neste caso'],
   c:1, h:'A lei exige que o empregador COMPROVE que exigiu o uso — não basta apenas fornecer.'},
];

/* ============================================================
   CONSTANTS
   ============================================================ */
const BLOCK_SIZE = 5;
const TOTAL_Q    = Q.length; // 50

/* ============================================================
   STATE
   ============================================================ */
let S = {
  player:    {name:'', dept:''},
  pool:      [],
  poolPos:   0,
  blockQ:    [],
  bqi:       0,
  blockNum:  0,
  totalBlocks: 0,
  score:     0,
  correct:   0,
  blockCorrect: 0,
  hintUsed:  false,
  answered:  false,
  ranking:   [],
  processingNext: false
};

try { S.ranking = JSON.parse(localStorage.getItem('balaska_sipat_2026') || '[]'); } catch(e){ S.ranking = []; }

/* ============================================================
   SHUFFLE
   ============================================================ */
function shuffle(arr){
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){
    const j=Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
  return a;
}

/* ============================================================
   AUDIO ENGINE
   ============================================================ */
let actx = null;
function ac(){ if(!actx){ try{ actx = new (window.AudioContext||window.webkitAudioContext)(); }catch(e){} } return actx; }

function playTone(freqs, type='sine', dur=0.35, vol=0.22){
  try{
    const ctx = ac(); if(!ctx) return;
    freqs.forEach((f,i)=>{
      const o = ctx.createOscillator(), g = ctx.createGain();
      o.connect(g); g.connect(ctx.destination);
      o.type = type; o.frequency.value = f;
      const t = ctx.currentTime + i*0.1;
      g.gain.setValueAtTime(0,t);
      g.gain.linearRampToValueAtTime(vol,t+0.02);
      g.gain.exponentialRampToValueAtTime(0.001,t+dur);
      o.start(t); o.stop(t+dur+0.05);
    });
  }catch(e){}
}
const sfx = {
  correct: ()=> playTone([523,659,784,1047],'sine',0.38,0.22),
  wrong:   ()=> playTone([300,200],'sawtooth',0.5,0.25),
  hint:    ()=> playTone([880],'sine',0.25,0.15),
  next:    ()=> playTone([440,550],'sine',0.22,0.18),
  fanfare: ()=>{ playTone([523,659,784],'sine',0.5,0.2); setTimeout(()=>playTone([1047],'sine',0.6,0.25),300); },
  rank:    ()=>{ playTone([784,880,1047],'sine',0.45,0.18); }
};

/* ============================================================
   NAVIGATION
   ============================================================ */
function goTo(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}
function showRegister(){
  goTo('screen-register');
  document.getElementById('inp-name').value='';
  document.getElementById('inp-dept').value='';
  document.getElementById('inp-outro').value='';
  document.getElementById('field-outro').style.display='none';
}

document.addEventListener('DOMContentLoaded', ()=>{
  document.getElementById('inp-dept').addEventListener('change', function(){
    const show = this.value === 'Outro';
    document.getElementById('field-outro').style.display = show ? 'block' : 'none';
    if(show) setTimeout(()=>document.getElementById('inp-outro').focus(), 50);
  });
});

/* ============================================================
   GAME START
   ============================================================ */
function initPoolIfNeeded(){
  if(!S.pool.length || S.poolPos >= 50){
    S.pool = shuffle([...Array(TOTAL_Q).keys()]);
    S.poolPos = 0;
  }
}

function startGame(){
  const name     = document.getElementById('inp-name').value.trim();
  const deptSel  = document.getElementById('inp-dept').value;
  const deptOutro= document.getElementById('inp-outro').value.trim();
  const dept     = deptSel === 'Outro' ? (deptOutro || '') : deptSel;

  if(!name){ alert('Por favor, informe seu nome!'); return; }
  if(!deptSel){ alert('Por favor, selecione seu setor!'); return; }
  if(deptSel === 'Outro' && !deptOutro){
    alert('Por favor, informe o nome do seu setor!');
    document.getElementById('inp-outro').focus();
    return;
  }

  const duplicate = S.ranking.find(r =>
    r.name.toLowerCase() === name.toLowerCase() &&
    r.dept.toLowerCase() === dept.toLowerCase()
  );
  if(duplicate){
    const overwrite = confirm(`Já existe um participante chamado "${name}" no setor "${dept}". Deseja substituir a pontuação anterior?`);
    if(!overwrite) return;
  }

  S.player = {name, dept};
  S.score = 0; S.correct = 0; S.blockCorrect = 0;
  S.bqi = 0; S.blockNum = 1;
  S.processingNext = false;

  initPoolIfNeeded();

  S.blockQ = S.pool.slice(S.poolPos, S.poolPos + BLOCK_SIZE).map(i => {
    const q     = Q[i];
    const order = shuffle([0,1,2,3]);
    return {
      ...q,
      a: order.map(j => q.a[j]),
      c: order.indexOf(q.c)
    };
  });
  S.poolPos += BLOCK_SIZE;
  S.totalBlocks = 10;

  document.getElementById('gh-name').textContent  = name;
  document.getElementById('gh-dept').textContent  = dept;
  document.getElementById('gh-score').textContent = '0';

  showNextPlayer();
}

/* ============================================================
   NEXT PLAYER SCREEN + COUNTDOWN
   ============================================================ */
function showNextPlayer(){
  document.getElementById('next-name').textContent = S.player.name;
  document.getElementById('next-dept').textContent = S.player.dept;
  goTo('screen-next');
  sfx.next();

  let secs = 3;
  const fill = document.getElementById('ring-fill');
  const num  = document.getElementById('cd-num');
  const circ = 2 * Math.PI * 35;

  function tick(){
    num.textContent = secs;
    fill.style.strokeDashoffset = circ * (1 - secs/3);
    if(secs <= 0){ goTo('screen-game'); renderQuestion(); return; }
    secs--;
    setTimeout(tick, 1000);
  }
  fill.style.strokeDasharray  = circ;
  fill.style.strokeDashoffset = 0;
  tick();
}

/* ============================================================
   RENDER QUESTION
   ============================================================ */
function renderQuestion(){
  const q = S.blockQ[S.bqi];
  S.hintUsed = false;
  S.answered = false;

  const pct = ((S.bqi+1)/BLOCK_SIZE*100).toFixed(0);
  document.getElementById('prog-bar').style.width   = pct+'%';
  document.getElementById('gh-plabel').textContent  = `Pergunta ${S.bqi+1} de ${BLOCK_SIZE}`;

  const pill = document.getElementById('cat-pill');
  pill.textContent = q.cat==='seg' ? '🦺 Segurança do Trabalho' : '🧤 EPI e EPC';
  pill.className   = 'cat-pill '+(q.cat==='seg'?'pill-seg':'pill-epi');

  const dt = document.getElementById('diff-tag');
  dt.textContent = q.diff==='easy' ? '⚡ FÁCIL' : '🔥 DIFÍCIL';
  dt.className   = 'diff-tag '+(q.diff==='easy'?'easy':'hard');

  document.getElementById('q-num').textContent  = `PERGUNTA ${S.bqi+1}`;
  document.getElementById('q-text').textContent = q.q;

  const opts = document.getElementById('opts');
  opts.innerHTML = '';
  ['A','B','C','D'].forEach((l,i)=>{
    const btn = document.createElement('button');
    btn.className = 'opt';
    btn.setAttribute('aria-label', `Alternativa ${l}: ${q.a[i]}`);
    btn.innerHTML = `<span class="opt-letter">${l}</span><span>${q.a[i]}</span>`;
    btn.onclick = ()=>pick(i,btn);
    opts.appendChild(btn);
  });

  const bh = document.getElementById('btn-hint');
  bh.disabled    = false;
  bh.textContent = '💡 Usar Dica';
  document.getElementById('hint-box').style.display = 'none';
  document.getElementById('hint-box').textContent   = '';

  const fb = document.getElementById('feedback-overlay');
  fb.className       = '';
  fb.style.display   = 'none';

  document.getElementById('screen-game').scrollTop = 0;
}

/* ============================================================
   PICK ANSWER
   ============================================================ */
function pick(idx, btn){
  if(S.answered) return;
  S.answered = true;

  const q       = S.blockQ[S.bqi];
  const allBtns = document.querySelectorAll('.opt');
  allBtns.forEach(b=>b.disabled=true);

  const isOk = idx === q.c;

  if(isOk){
    btn.classList.add('correct');
    const pts = q.diff==='hard' ? 20 : 10;
    S.score += pts; S.correct++; S.blockCorrect++;
    document.getElementById('gh-score').textContent = S.score;
    sfx.correct();
    showFeedback(true);
  } else {
    btn.classList.add('wrong');
    allBtns[q.c].classList.add('reveal-correct');
    sfx.wrong();
    showFeedback(false);
  }

  setTimeout(()=>{
    hideFeedback();
    S.bqi++;
    if(S.bqi < BLOCK_SIZE){
      renderQuestion();
    } else {
      saveToRanking();
      showBlockRanking();
    }
  }, 1800);
}

/* ============================================================
   FEEDBACK
   ============================================================ */
const CORRECT_WORDS = ['ARRASOU!','CORRETO!','ISSO AÍ!','SHOW!','MANDOU BEM!'];
const WRONG_WORDS   = ['ERROU!','QUE PENA!','NÃO ERA ESSA!','TENTE MAIS!'];

function showFeedback(ok){
  const el = document.getElementById('feedback-overlay');
  el.style.display = 'flex';
  el.className = ok ? 'show fb-correct' : 'show fb-wrong';
  document.getElementById('fb-emoji').textContent = ok
    ? ['🎉','🔥','⭐','💥','✅'][Math.floor(Math.random()*5)]
    : ['😬','💀','😅','🙈'][Math.floor(Math.random()*4)];
  document.getElementById('fb-word').textContent = ok
    ? CORRECT_WORDS[Math.floor(Math.random()*CORRECT_WORDS.length)]
    : WRONG_WORDS[Math.floor(Math.random()*WRONG_WORDS.length)];
}
function hideFeedback(){
  const el       = document.getElementById('feedback-overlay');
  el.className   = '';
  el.style.display = 'none';
}

/* ============================================================
   HINT
   ============================================================ */
function useHint(){
  if(S.hintUsed || S.answered) return;
  S.hintUsed = true;
  sfx.hint();
  const hb       = document.getElementById('hint-box');
  hb.textContent = '💡 ' + S.blockQ[S.bqi].h;
  hb.style.display = 'block';
  document.getElementById('btn-hint').disabled = true;
}

/* ============================================================
   SAVE TO RANKING
   ============================================================ */
function saveToRanking(){
  const trophy = S.blockCorrect>=5?'🏆':S.blockCorrect>=4?'🥇':S.blockCorrect>=2?'🥈':'🥉';
  const idx    = S.ranking.findIndex(r => r.name===S.player.name && r.dept===S.player.dept);
  if(idx >= 0){
    S.ranking[idx].score   = S.score;
    S.ranking[idx].correct = S.correct;
    S.ranking[idx].rounds  = (S.ranking[idx].rounds||0)+1;
    S.ranking[idx].trophy  = trophy;
  } else {
    S.ranking.push({name:S.player.name, dept:S.player.dept, score:S.score, correct:S.correct, rounds:1, trophy});
  }
  S.ranking.sort((a,b)=>b.score-a.score);
  if(S.ranking.length>25) S.ranking=S.ranking.slice(0,25);
  try{ localStorage.setItem('balaska_sipat_2026', JSON.stringify(S.ranking)); }catch(e){}
}

/* ============================================================
   BLOCK RANKING (mid-game)
   ============================================================ */
function showBlockRanking(){
  sfx.rank();
  goTo('screen-block-rank');

  document.getElementById('br-badge').textContent = `✦ BLOCO ${S.blockNum} CONCLUÍDO ✦`;

  const dotsEl    = document.getElementById('block-dots');
  const totalDots = S.totalBlocks;
  dotsEl.innerHTML = Array.from({length:totalDots},(_,i)=>{
    const cls = i+1 < S.blockNum ? 'done' : i+1 === S.blockNum ? 'current' : 'upcoming';
    return `<div class="block-dot ${cls}"></div>`;
  }).join('');

  const trophy = S.blockCorrect>=5?'🏆':S.blockCorrect>=4?'🥇':S.blockCorrect>=2?'🥈':'🥉';
  document.getElementById('br-my-trophy').textContent = trophy;
  document.getElementById('br-my-name').textContent   = S.player.name;
  document.getElementById('br-my-pts').textContent    =
    `${S.blockCorrect} de ${BLOCK_SIZE} acertos • ${S.score} pt${S.score!==1?'s':''} no total`;
  document.getElementById('br-my-score').textContent  = S.score;

  const list   = document.getElementById('br-list');
  const MEDALS = ['🥇','🥈','🥉'];

  if(!S.ranking.length){
    list.innerHTML = '<div class="empty-rank">Nenhum participante ainda.</div>';
  } else {
    const subtitle = `<div class="br-participants-label">
      👥 ${S.ranking.length} participante${S.ranking.length>1?'s':''} registrado${S.ranking.length>1?'s':''}
    </div>`;

    const rows = S.ranking.map((p,i)=>{
      const isMe   = p.name===S.player.name && p.dept===S.player.dept;
      const totalQ = (p.rounds||1) * BLOCK_SIZE;
      const pct    = Math.round(p.correct / totalQ * 100);
      return `
        <div class="rank-item ${isMe?'rank-item-me':''}" style="animation-delay:${Math.min(i*0.06,0.8)}s">
          <div class="rank-pos ${i<3?'p'+(i+1):'pN'}">${i+1}º</div>
          <div class="rank-medal">${MEDALS[i]||p.trophy}</div>
          <div class="rank-info">
            <div class="rank-name">${p.name}${isMe?' <span class="me-tag">VOCÊ</span>':''}</div>
            <div class="rank-dept">${p.dept}</div>
          </div>
          <div class="rank-right">
            <div class="rank-score">${p.score}</div>
            <div class="rank-hits">${p.correct} acerto${p.correct!==1?'s':''} • ${pct}%</div>
          </div>
        </div>`;
    }).join('');

    list.innerHTML = subtitle + rows;
  }

  document.getElementById('br-continue-btn').textContent = '👤 PRÓXIMO PARTICIPANTE';
  S.processingNext = false;
}

/* ============================================================
   CONTINUE AFTER BLOCK
   ============================================================ */
function continueAfterBlock(){
  if(S.processingNext) return;
  S.processingNext  = true;
  S.blockCorrect    = 0;
  S.bqi             = 0;
  S.blockNum++;
  showRegister();
}

/* ============================================================
   RANKING (final / from home)
   ============================================================ */
function showRanking(){
  goTo('screen-ranking');
  const list = document.getElementById('rank-list');
  if(!S.ranking.length){
    list.innerHTML = '<div class="empty-rank">Nenhum participante ainda.<br>Seja o primeiro! 🚀</div>';
    return;
  }
  const MEDALS = ['🥇','🥈','🥉'];
  list.innerHTML = S.ranking.map((p,i)=>`
    <div class="rank-item" style="animation-delay:${Math.min(i*0.07,0.6)}s">
      <div class="rank-pos ${i<3?'p'+(i+1):'pN'}">${i+1}º</div>
      <div class="rank-medal">${MEDALS[i]||p.trophy}</div>
      <div class="rank-info">
        <div class="rank-name">${p.name}</div>
        <div class="rank-dept">${p.dept}</div>
      </div>
      <div class="rank-right">
        <div class="rank-score">${p.score}</div>
        <div class="rank-hits">${p.correct} acertos</div>
      </div>
    </div>
  `).join('');
}
</script>
</body>
</html>
