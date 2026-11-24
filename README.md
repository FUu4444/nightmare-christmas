
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>噩梦 · 圣诞夜</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { height: 100%; }
  body {
    background: #050506; color: #cfc9bf;
    font-family: Georgia, "Songti SC", "SimSun", serif;
    display: flex; align-items: center; justify-content: center;
    min-height: 100vh; padding: 24px; line-height: 1.9;
    position: relative; overflow: hidden; transition: background .8s;
  }
  body::after {
    content: ""; position: fixed; inset: 0; pointer-events: none;
    background: radial-gradient(ellipse at center, transparent 35%, rgba(30,0,0,.85) 100%);
    animation: breathe 7s ease-in-out infinite;
    transition: opacity 1s;
  }
  @keyframes breathe { 0%,100%{opacity:.8} 50%{opacity:1} }
  #game { width: 100%; max-width: 680px; position: relative; z-index: 1; }
  #meta {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 26px; font-size: 12px; letter-spacing: 2px; color: #6a5a58;
  }
  #noise { display: flex; align-items: center; gap: 8px; transition: opacity .6s; }
  #noisebar { width: 84px; height: 3px; background: #1c1414; border-radius: 2px; overflow: hidden; }
  #noisefill { height: 100%; width: 0%; background: linear-gradient(90deg,#5e3a38,#8a1a1a); transition: width .5s ease; }
  #mute {
    background: transparent; border: 1px solid #332626; color: #6a5a58;
    font-family: inherit; font-size: 11px; letter-spacing: 1px;
    padding: 4px 8px; cursor: pointer; transition: .3s;
  }
  #mute:hover { color: #cfc9bf; border-color: #6f5a5a; }
  #text p {
    opacity: 0; transform: translateY(4px); margin: 0 0 14px;
    animation: rise .9s ease forwards; font-size: 17px;
  }
  @keyframes rise { to { opacity: 1; transform: translateY(0); } }
  #choices { margin-top: 30px; display: flex; flex-direction: column; gap: 10px; }
  .choice {
    background: transparent; border: 1px solid #2b2222; color: #cfc9bf;
    padding: 13px 18px; font-family: inherit; font-size: 15px;
    text-align: left; cursor: pointer; transition: .3s;
    opacity: 0; animation: rise .7s ease forwards;
  }
  .choice:hover { border-color: #7a4a4a; background: #130e0e; color: #e6d8d2; }

  @keyframes shakeA {
    0%,100% { transform: translate(0,0); }
    20% { transform: translate(-3px,2px); }
    40% { transform: translate(3px,-2px); }
    60% { transform: translate(-2px,-1px); }
    80% { transform: translate(2px,1px); }
  }
  @keyframes shakeB {
    0%,100% { transform: translate(0,0); }
    20% { transform: translate(6px,-3px) rotate(.15deg); }
    40% { transform: translate(-6px,3px) rotate(-.15deg); }
    60% { transform: translate(4px,2px) rotate(.1deg); }
    80% { transform: translate(-4px,-2px) rotate(-.1deg); }
  }
  .shake { animation: shakeA .38s infinite; }
  .shake-hard { animation: shakeB .2s infinite; }

  #intro {
    position: fixed; inset: 0; background: #050506; z-index: 10;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    text-align: center; padding: 24px; transition: opacity 1s;
  }
  #intro h1 { font-size: 26px; letter-spacing: 6px; color: #9a6a6a; margin-bottom: 18px; font-weight: normal; }
  #intro p { font-size: 13px; color: #6a5a58; letter-spacing: 1px; margin-bottom: 10px; line-height: 2; }
  #intro .off { color: #8a4a4a; margin-bottom: 36px; letter-spacing: 2px; }
  #intro button {
    background: transparent; border: 1px solid #4a3030; color: #cfc9bf;
    padding: 12px 36px; font-family: inherit; font-size: 15px; letter-spacing: 4px;
    cursor: pointer; transition: .3s;
  }
  #intro button:hover { border-color: #9a6a6a; color: #e6d8d2; }

  #flood { position: fixed; inset: 0; z-index: 50; pointer-events: none; overflow: hidden; }
  .floodword {
    position: absolute; white-space: nowrap; opacity: 0;
    color: #c1121f; font-family: Georgia, serif;
    text-shadow: 0 0 14px rgba(193,18,31,.7);
    animation: floodIn .5s ease forwards;
    will-change: opacity, transform;
  }
  @keyframes floodIn { to { opacity: .9; } }

  #whiteout {
    position: fixed; inset: 0; background: #fff; z-index: 60;
    opacity: 0; transition: opacity 1.0s ease; pointer-events: none;
  }
  body.wake {
    background: #f2efe9 !important; color: #2a2622;
    display: block; padding: 0; overflow: auto;
  }
  body.wake::after { display: none; }
  .wake {
    min-height: 100vh; display: flex; flex-direction: column;
    align-items: center; justify-content: center; text-align: center;
    padding: 40px 28px; animation: wakein 2s ease forwards; opacity: 0;
  }
  @keyframes wakein { to { opacity: 1; } }
  .wake p { font-size: 16px; margin: 0 0 16px; color: #3a342e; }
  .wake .first { font-size: 22px; letter-spacing: 4px; margin-bottom: 34px; color: #2a2622; }
  .wake .small { font-size: 13px; color: #7a7268; margin-top: 28px; }
  .wake button {
    margin-top: 40px; background: transparent; border: 1px solid #b8b0a4;
    color: #4a4238; padding: 10px 28px; font-family: inherit;
    font-size: 13px; letter-spacing: 3px; cursor: pointer; transition: .3s;
  }
  .wake button:hover { background: #e6e0d6; border-color: #8a8070; }
</style>
</head>
<body>

<div id="intro">
  <h1>噩梦 · 圣诞夜</h1>
  <p>建议戴耳机。</p>
  <p class="off">今晚，所有人都在。</p>
  <button id="startBtn">进 入</button>
</div>

<div id="game">
  <div id="meta">
    <span id="chapter"></span>
    <div style="display:flex;align-items:center;gap:14px">
      <div id="noise"><span>声音</span><div id="noisebar"><div id="noisefill"></div></div></div>
      <button id="mute">静音</button>
    </div>
  </div>
  <div id="text"></div>
  <div id="choices"></div>
</div>

<script>
/* ================= 音频 ================= */
let ctx = null, master = null, started = false, muted = false, drone = null;

function initAudio() {
  if (ctx) return;
  const AC = window.AudioContext || window.webkitAudioContext;
  ctx = new AC();
  master = ctx.createGain();
  master.gain.value = 0.75;
  master.connect(ctx.destination);
}
function now() { return ctx.currentTime; }
function noiseBuffer(dur) {
  const len = Math.floor(ctx.sampleRate * dur);
  const b = ctx.createBuffer(1, len, ctx.sampleRate);
  const d = b.getChannelData(0);
  for (let i = 0; i < len; i++) d[i] = Math.random() * 2 - 1;
  return b;
}
function startAmbient() {
  if (started || !ctx) return;
  started = true;
  const g = ctx.createGain(); g.gain.value = 0.03; g.connect(master);
  const o1 = ctx.createOscillator(); o1.type = 'sine'; o1.frequency.value = 48;
  const o2 = ctx.createOscillator(); o2.type = 'sine'; o2.frequency.value = 48.7;
  const f = ctx.createBiquadFilter(); f.type = 'lowpass'; f.frequency.value = 150;
  o1.connect(f); o2.connect(f); f.connect(g); o1.start(); o2.start();
  const lfo = ctx.createOscillator(); lfo.frequency.value = 0.13;
  const lg = ctx.createGain(); lg.gain.value = 0.012;
  lfo.connect(lg); lg.connect(g.gain); lfo.start();
  const n = ctx.createBufferSource(); n.buffer = noiseBuffer(4); n.loop = true;
  const nf = ctx.createBiquadFilter(); nf.type = 'lowpass'; nf.frequency.value = 240;
  const ng = ctx.createGain(); ng.gain.value = 0.006;
  n.connect(nf); nf.connect(ng); ng.connect(master); n.start();
  const ro = ctx.createOscillator(); ro.type = 'sawtooth'; ro.frequency.value = 32;
  const rf = ctx.createBiquadFilter(); rf.type = 'lowpass'; rf.frequency.value = 100;
  const rg = ctx.createGain(); rg.gain.value = 0;
  ro.connect(rf); rf.connect(rg); rg.connect(master); ro.start();
  drone = { osc: ro, gain: rg, filter: rf };
}
function risingDrone(duration) {
  if (!drone) return;
  const t = now();
  drone.gain.gain.cancelScheduledValues(t);
  drone.gain.gain.setValueAtTime(Math.max(drone.gain.gain.value, 0.001), t);
  drone.gain.gain.linearRampToValueAtTime(0.13, t + duration);
  drone.osc.frequency.linearRampToValueAtTime(62, t + duration);
  drone.filter.frequency.linearRampToValueAtTime(300, t + duration);
}
function whisperTick() {
  if (!ctx) return;
  const t = now();
  const n = ctx.createBufferSource(); n.buffer = noiseBuffer(0.3);
  const f = ctx.createBiquadFilter();
  f.type = 'bandpass'; f.frequency.value = 1100 + Math.random() * 2200; f.Q.value = 9;
  const g = ctx.createGain();
  g.gain.setValueAtTime(0, t);
  g.gain.linearRampToValueAtTime(0.022 * (0.5 + Math.random()), t + 0.05);
  g.gain.exponentialRampToValueAtTime(0.001, t + 0.26);
  n.connect(f); f.connect(g); g.connect(master); n.start(t); n.stop(t + 0.3);
}
function heartbeat(vol) {
  if (!ctx) return;
  const t0 = now();
  [0, 0.17].forEach((off, i) => {
    const t = t0 + off;
    const o = ctx.createOscillator(); o.type = 'sine';
    o.frequency.setValueAtTime(64, t);
    o.frequency.exponentialRampToValueAtTime(36, t + 0.12);
    const g = ctx.createGain();
    g.gain.setValueAtTime(0, t);
    g.gain.linearRampToValueAtTime(vol * (i ? 0.5 : 1), t + 0.01);
    g.gain.exponentialRampToValueAtTime(0.001, t + 0.16);
    o.connect(g); g.connect(master); o.start(t); o.stop(t + 0.2);
  });
}
function accelerateHeartbeat() {
  let interval = 900;
  const step = () => {
    if (interval < 180) return;
    heartbeat(0.4);
    interval *= 0.89;
    setTimeout(step, interval);
  };
  step();
}
function creak() {
  if (!ctx) return;
  const t = now();
  const o = ctx.createOscillator(); o.type = 'sawtooth';
  o.frequency.setValueAtTime(190, t);
  o.frequency.linearRampToValueAtTime(120, t + 0.6);
  o.frequency.linearRampToValueAtTime(210, t + 1.1);
  const f = ctx.createBiquadFilter(); f.type = 'bandpass'; f.frequency.value = 700; f.Q.value = 8;
  const g = ctx.createGain();
  g.gain.setValueAtTime(0, t);
  g.gain.linearRampToValueAtTime(0.07, t + 0.12);
  g.gain.linearRampToValueAtTime(0.045, t + 0.8);
  g.gain.linearRampToValueAtTime(0, t + 1.2);
  o.connect(f); f.connect(g); g.connect(master); o.start(t); o.stop(t + 1.25);
}
function breathSoft() {
  if (!ctx) return;
  const t = now();
  const n = ctx.createBufferSource(); n.buffer = noiseBuffer(1.4);
  const f = ctx.createBiquadFilter(); f.type = 'bandpass'; f.frequency.value = 700; f.Q.value = 1.2;
  const g = ctx.createGain();
  g.gain.setValueAtTime(0, t);
  g.gain.linearRampToValueAtTime(0.03, t + 0.4);
  g.gain.linearRampToValueAtTime(0.008, t + 1.0);
  g.gain.linearRampToValueAtTime(0, t + 1.35);
  n.connect(f); f.connect(g); g.connect(master); n.start(t); n.stop(t + 1.4);
}
function fadeMaster(seconds, target) {
  if (!ctx || !master) return;
  master.gain.linearRampToValueAtTime(target, ctx.currentTime + seconds);
}

/* ================= 剧情 ================= */
const nodes = {
  n_start: {
    chapter: "圣诞夜",
    sound: () => breathSoft(),
    text: [
      "八岁那年的圣诞夜，外祖母家的房子很暖。",
      "餐厅里坐满了人。长桌上摆着烤鹅、布丁、热红酒。",
      "你坐在桌子的这一头。博卡斯在你旁边，脑袋一点一点地，快睡着了。",
      "你抬起头，看了一眼桌边的人。",
      "他们的脸，有点看不清。"
    ],
    choices: [
      { label: "带博卡斯上楼睡觉", to: "n_bed" },
      { label: "再坐一会儿", to: "n_more" }
    ]
  },
  n_more: {
    chapter: "圣诞夜",
    text: [
      "你又坐了一会儿。",
      "楼下很吵，笑声很大。",
      "但你听不清他们在笑什么。",
      "博卡斯的脑袋彻底歪到了一边。",
      "外祖母看了你一眼——如果那真的是外祖母的话。",
      "“带他上去睡吧。”她说。"
    ],
    choices: [ { label: "带博卡斯上楼", to: "n_bed" } ]
  },
  n_bed: {
    chapter: "二楼",
    sound: () => whisperTick(),
    text: [
      "二楼最里面的房间。你帮博卡斯脱了鞋，他钻进被窝，很快就睡着了。",
      "你坐在床边。",
      "楼下安静下来了。",
      "然后，有人叫你的名字。"
    ],
    choices: [
      { label: "出去看看", to: "n_hall" },
      { label: "叫醒博卡斯", to: "n_wake" },
      { label: "躲进衣柜", to: "n_closet" }
    ]
  },
  n_hall: {
    chapter: "走廊",
    sound: () => { whisperTick(); setTimeout(creak, 900); },
    text: [
      "你走出房间。",
      "走廊比记忆里长。",
      "尽头有一点光。",
      "那个声音又响起来，很温柔，叫你的名字。",
      "你往里走。",
      "你走不到尽头。"
    ],
    choices: [ { label: "回头", to: "n_closet" } ]
  },
  n_wake: {
    chapter: "半夜",
    sound: () => whisperTick(),
    text: [
      "你摇醒博卡斯。",
      "他睁开眼，看着你。",
      "“怎么了？”他问。",
      "然后他笑了一下。",
      "“他们在楼下等你呢。”",
      "那不是博卡斯的声音。"
    ],
    choices: [ { label: "拉着他躲进衣柜", to: "n_closet" } ]
  },
  n_closet: {
    chapter: "衣柜",
    sound: () => { breathSoft(); setTimeout(heartbeat, 600); setTimeout(creak, 1600); },
    text: [
      "衣柜不大，塞满了冬天的衣服，有樟脑丸的味道。",
      "你把柜门带上，留了一条极细的缝。",
      "博卡斯缩在你怀里，在发抖。",
      "门把手转动了。",
      "门外有人说：“是我。开门。”"
    ],
    choices: [
      { label: "捂住博卡斯的嘴，不出声", to: "n_flood1" },
      { label: "打开柜门", to: "n_flood2" },
      { label: "盯着那条缝，不动", to: "n_flood3" }
    ]
  },
  n_flood1: {
    chapter: "衣柜",
    text: [ "你捂住他的嘴。", "门外的声音停了。", "然后，它开始叫博卡斯的名字。" ],
    choices: [ { label: "——", to: "FLOOD" } ]
  },
  n_flood2: {
    chapter: "衣柜",
    text: [ "你推开柜门。", "外面没有人。", "走廊上很暗。" ],
    choices: [ { label: "——", to: "FLOOD" } ]
  },
  n_flood3: {
    chapter: "衣柜",
    text: [ "你盯着那条缝。", "缝外有东西在动。", "它也在看你。" ],
    choices: [ { label: "——", to: "FLOOD" } ]
  }
};

const FLOOD_WORDS = [
  "别出声", "博卡斯", "你活下来了", "他们都死了", "衣柜", "血",
  "为什么是你", "你没有救他们", "门锁着", "脚步声", "别回头",
  "你听见了吗", "他还在楼下", "八岁", "圣诞夜", "这不是你的错",
  "你活下来了", "你活下来了", "你活下来了", "开门", "是我",
  "你走不到尽头", "那不是博卡斯的声音", "缝外有东西", "它在看你",
  "一", "二", "三", "别出声", "别出声", "博卡斯", "博卡斯",
  "你活下来了", "你活下来了", "为什么是你", "你听见了吗"
];

let state = { noise: 0 };
const elText = document.getElementById('text');
const elChoices = document.getElementById('choices');
const elChapter = document.getElementById('chapter');
const elNoise = document.getElementById('noise');
const elNoiseFill = document.getElementById('noisefill');

function updateNoise() {
  const pct = Math.min(state.noise / 4 * 100, 100);
  elNoiseFill.style.width = pct + '%';
  elNoise.style.opacity = state.noise > 0 ? 1 : 0.35;
}

function go(id) {
  if (id === 'FLOOD') { startFlood(); return; }
  const node = nodes[id];
  if (!node) return;
  elChapter.textContent = node.chapter || '';
  elText.innerHTML = '';
  elChoices.innerHTML = '';
  if (node.sound) node.sound();
  node.text.forEach((p, i) => {
    const para = document.createElement('p');
    para.textContent = p;
    para.style.animationDelay = (i * 0.5) + 's';
    elText.appendChild(para);
  });
  const delay = node.text.length * 500 + 400;
  setTimeout(() => {
    node.choices.forEach((c, i) => {
      const btn = document.createElement('button');
      btn.className = 'choice';
      btn.textContent = c.label;
      btn.style.animationDelay = (i * 0.12) + 's';
      btn.onclick = () => {
        if (c.effect) c.effect(state);
        updateNoise();
        go(c.to);
      };
      elChoices.appendChild(btn);
    });
  }, delay);
}

/* ================= 血字淹没 ================= */
function startFlood() {
  elChapter.textContent = "";
  elText.innerHTML = '';
  elChoices.innerHTML = '';
  const game = document.getElementById('game');
  game.style.transition = 'opacity 2.5s';
  game.style.opacity = '0.08';

  risingDrone(9);
  accelerateHeartbeat();

  const flood = document.createElement('div');
  flood.id = 'flood';
  document.body.appendChild(flood);

  game.classList.add('shake');
  flood.classList.add('shake');

  const COLS = 20, ROWS = 18;
  const cells = [];
  for (let r = 0; r < ROWS; r++) {
    for (let c = 0; c < COLS; c++) {
      cells.push({ c, r });
    }
  }
  for (let i = cells.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [cells[i], cells[j]] = [cells[j], cells[i]];
  }

  const TOTAL = 720;
  let count = 0;

  const spawn = () => {
    if (count >= TOTAL) {
      setTimeout(triggerWhite, 400);
      return;
    }

    const cell = cells[count % cells.length];
    const jitterX = (Math.random() - 0.5) * (100 / COLS) * 0.95;
    const jitterY = (Math.random() - 0.5) * (100 / ROWS) * 0.95;
    const left = (cell.c + 0.5) * (100 / COLS) + jitterX;
    const top = (cell.r + 0.5) * (100 / ROWS) + jitterY;

    const el = document.createElement('div');
    el.className = 'floodword';
    el.textContent = FLOOD_WORDS[Math.floor(Math.random() * FLOOD_WORDS.length)];
    el.style.left = Math.max(2, Math.min(98, left)) + '%';
    el.style.top = Math.max(2, Math.min(98, top)) + '%';
    el.style.fontSize = (6 + Math.random() * 32) + 'px';
    el.style.transform = 'translate(-50%,-50%) rotate(' + (Math.random() * 24 - 12) + 'deg)';
    el.style.animationDelay = '0s';
    flood.appendChild(el);

    if (Math.random() < 0.4) whisperTick();
    count++;

    // 铺字总时长已砍半：约 8.3 秒
    const t = count / TOTAL;
    const base = 14.5 - t * 12;
    setTimeout(spawn, Math.max(2, base + Math.random() * 6));

    if (count === 320) {
      game.classList.remove('shake');
      flood.classList.remove('shake');
      game.classList.add('shake-hard');
      flood.classList.add('shake-hard');
    }
  };
  spawn();
}

function triggerWhite() {
  fadeMaster(0.5, 0.0001);
  setTimeout(() => { if (ctx) ctx.suspend(); }, 700);

  const w = document.createElement('div');
  w.id = 'whiteout';
  document.body.appendChild(w);
  requestAnimationFrame(() => { w.style.opacity = '1'; });

  setTimeout(() => {
    const flood = document.getElementById('flood');
    if (flood) flood.remove();
    const wo = document.getElementById('whiteout');
    if (wo) wo.remove();
    const g = document.getElementById('game');
    if (g) g.remove();
    document.body.classList.add('wake');

    const wrap = document.createElement('div');
    wrap.className = 'wake';
    wrap.innerHTML = `
      <p class="first">罗赛因特醒了。</p>
      <p>房间很暗。被子好好地盖在身上。</p>
      <p>旁边有呼吸声。博卡斯睡得很沉，一只手搭在他肩上。</p>
      <p>他躺了很久，没有动。</p>
      <p class="small">窗外，伦敦在下雨。他没有把那个梦告诉博卡斯。</p>
      <button id="again">躺下重睡</button>
    `;
    document.body.appendChild(wrap);

    document.getElementById('again').addEventListener('click', () => {
      location.reload();
    });
  }, 2600);
}

/* ================= 启动 ================= */
document.getElementById('startBtn').addEventListener('click', () => {
  initAudio();
  if (ctx.state === 'suspended') ctx.resume();
  startAmbient();
  const intro = document.getElementById('intro');
  intro.style.opacity = '0';
  setTimeout(() => intro.remove(), 1000);
  go('n_start');
});

document.getElementById('mute').addEventListener('click', (e) => {
  if (!ctx) return;
  muted = !muted;
  master.gain.linearRampToValueAtTime(muted ? 0.0001 : 0.75, ctx.currentTime + 0.2);
  e.target.textContent = muted ? '取消静音' : '静音';
});

updateNoise();
</script>
</body>
</html>
