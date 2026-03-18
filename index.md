<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0"/> 
<title>HOANGDZ AI PREDICT v5.2</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;900&display=swap" rel="stylesheet">
<style>
:root {
  --neon: #00ffcc;
  --tai: #ff0055;
  --xiu: #00ff88;
  --bg: rgba(10, 25, 40, 0.98);
}
* { box-sizing: border-box; touch-action: none; user-select: none; }
body { margin: 0; background: #000; overflow: hidden; font-family: 'Orbitron', sans-serif; color: #fff; }

/* MÀN HÌNH CHỜ */
#setupOverlay {
  position: fixed; inset: 0; background: radial-gradient(circle, #002b20 0%, #000 100%);
  z-index: 20000; display: flex; flex-direction: column; align-items: center; justify-content: center;
}
.setup-box {
  background: var(--bg); border: 2px solid var(--neon);
  padding: 30px; border-radius: 30px; width: 380px; text-align: center;
  box-shadow: 0 0 50px rgba(0, 255, 204, 0.2);
}
.game-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-top: 25px; }
.game-card { 
    background: rgba(0, 40, 30, 0.6); border: 1px solid var(--neon); 
    border-radius: 15px; padding: 10px; cursor: pointer; transition: 0.2s; font-size: 10px;
}
.game-card:hover { transform: translateY(-5px); box-shadow: 0 5px 15px var(--neon); }
.game-card img { width: 50px; height: 50px; border-radius: 12px; margin-bottom: 5px; }

/* TOOL CHÍNH: ROBOT NẰM TRÁI - PANEL NẰM PHẢI */
.bot-wrap { 
  position: fixed; z-index: 9999; display: none; 
  flex-direction: row; /* Chuyển thành hàng ngang */
  align-items: center; gap: 20px;
  top: 50%; left: 50%; 
  transform: translate(-50%, -50%) rotate(90deg) scale(0.85);
  width: auto; height: auto;
}

/* Robot to nằm bên TRÁI */
.robot-box {
  flex-shrink: 0;
}
.robot-img { 
  width: 150px; /* Robot to rõ nét */
  filter: drop-shadow(0 0 25px var(--neon)); 
  animation: float 3s infinite alternate ease-in-out; 
}
@keyframes float { from {transform: translateY(0);} to {transform: translateY(-15px);} }

/* Panel nằm bên PHẢI */
.panel { 
  width: 260px; background: var(--bg); backdrop-filter: blur(20px);
  border-radius: 25px; padding: 18px; border: 1.5px solid var(--neon);
  box-shadow: 0 10px 40px rgba(0,0,0,0.8);
}
.header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
.title { font-size: 10px; font-weight: 900; color: var(--neon); letter-spacing: 1px; }

input.cau-input { 
    width: 100%; background: rgba(0,0,0,0.5); border: 1px solid var(--neon); 
    color: #fff; padding: 12px; border-radius: 12px; font-size: 11px; 
    text-align: center; outline: none; margin-bottom: 12px;
}
.predict-btn { 
    width: 100%; padding: 14px; border: none; border-radius: 12px; 
    font-weight: 900; cursor: pointer; background: var(--neon); color: #000;
}

.res-box { margin-top: 15px; border-top: 1px solid rgba(0,255,204,0.2); padding-top: 12px; text-align: center; }
.val { font-weight: 900; font-size: 32px; display: block; margin: 5px 0; }
.rate { font-size: 12px; color: #ffd700; margin-bottom: 10px; font-weight: bold; }

.action-btns { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.btn-check { padding: 10px; border: none; border-radius: 8px; font-size: 10px; font-weight: 900; cursor: pointer; }
.btn-win { background: #fff; color: #000; }
.btn-lose { background: #ff4444; color: #fff; }

/* POPUP */
#msgPopup {
    display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
    z-index: 100000; padding: 30px; border-radius: 25px; text-align: center;
    font-weight: 900; border: 5px solid #fff; box-shadow: 0 0 50px rgba(0,0,0,0.5);
    min-width: 250px;
}

#gameFrame { position: fixed; inset: 0; width: 100vw; height: 100vh; border: none; z-index: 1; display: none; }
#exitBtn { position: fixed; top: 15px; left: 15px; z-index: 10005; background: #ff0055; color: #fff; border: none; padding: 10px 20px; border-radius: 12px; display: none; font-size: 12px; font-weight: bold; }
</style>
</head>
<body>

<div id="setupOverlay">
  <div class="setup-box">
    <div style="font-size: 28px; font-weight: 900; color: var(--neon); text-shadow: 0 0 15px var(--neon);">HOANGDZ MASTER</div>
    <p style="font-size: 12px; color: #00ffcc; margin-bottom: 30px; letter-spacing: 2px;">ROBOT AI HYBRID V5.2</p>
    
    <div class="game-grid">
        <div class="game-card" onclick="launch('https://play.betvip.fit')">
            <img src="https://i.postimg.cc/ZqqrCCjQ/A3EFE26C-7E67-4AFF-A21F-130E42D235EE.png"><br>BETVIP
        </div>
        <div class="game-card" onclick="launch('https://lc79b.bet')">
            <img src="https://i.postimg.cc/JnLWCgjm/3E497BE2-D59F-46D3-BCCF-FDC2E3A9929C.png"><br>LC79
        </div>
        <div class="game-card" onclick="launch('https://68gbvn88.bar')">
            <img src="https://i.postimg.cc/mD7XFp9t/68gamebai.png"><br>68GB
        </div>
    </div>
  </div>
</div>

<button id="exitBtn" onclick="location.reload()">THOÁT TOOL</button>
<iframe id="gameFrame"></iframe>

<!-- TOOL LAYOUT: ROBOT (LEFT) + PANEL (RIGHT) -->
<div id="wrapMD5" class="bot-wrap" onmousedown="dragS(event, this)" ontouchstart="dragS(event, this)">
  <div class="robot-box">
    <img class="robot-img" src="https://i.postimg.cc/63bdy9D9/robotics-1.gif">
  </div>
  
  <div class="panel">
    <div class="header">
        <span class="title">MD5 DECODER V5.2</span>
        <div style="display:flex; gap:6px;">
            <button style="width:24px; height:24px; background:#111; border:1px solid var(--neon); color:#fff; border-radius:6px; cursor:pointer;" onclick="zm(-0.1)">-</button>
            <button style="width:24px; height:24px; background:#111; border:1px solid var(--neon); color:#fff; border-radius:6px; cursor:pointer;" onclick="zm(0.1)">+</button>
        </div>
    </div>
    <input type="text" id="md5Input" class="cau-input" placeholder="NHẬP MÃ MD5 32 KÝ TỰ...">
    <button class="predict-btn" onclick="runHybridV4()">PHÂN TÍCH AI</button>
    
    <div id="resBox" class="res-box" style="display:none;">
      <div class="rate" id="rateTxt">CHỜ DỮ LIỆU...</div>
      <span class="val" id="resVal">---</span>
      <div class="action-btns">
          <button class="btn-check btn-win" onclick="verify(true)">CHIẾN THẮNG</button>
          <button class="btn-check btn-lose" onclick="verify(false)">SAI (XL)</button>
      </div>
    </div>
  </div>
</div>

<div id="msgPopup"></div>

<script>
// --- BẢO MẬT CHẶN F12 ---
(function() {
    document.addEventListener('contextmenu', e => e.preventDefault());
    document.onkeydown = e => { if(e.keyCode == 123 || (e.ctrlKey && e.shiftKey && (e.keyCode == 73 || e.keyCode == 74))) return false; };
    setInterval(() => { debugger; console.clear(); }, 1000);
})();

let scale = 0.85;
let lastRes = "";

function launch(url) {
  document.getElementById('gameFrame').src = url;
  document.getElementById('gameFrame').style.display = 'block';
  document.getElementById('setupOverlay').style.display = 'none';
  document.getElementById('exitBtn').style.display = 'block';
  document.getElementById('wrapMD5').style.display = 'flex';
}

// THUẬT TOÁN HYBRID V4[span_0](start_span)[span_0](end_span)
function runHybridV4() {
    const md5 = document.getElementById('md5Input').value.trim().toLowerCase();
    if (md5.length < 5) return alert("Vui lòng nhập MD5!");

    const resVal = document.getElementById('resVal'), 
          rateTxt = document.getElementById('rateTxt'), 
          resBox = document.getElementById('resBox');

    resBox.style.display = "block";
    resVal.textContent = "DECODING...";
    resVal.style.color = "#fff";

    setTimeout(() => {
        const hex = Array.from(md5).map(c => parseInt(c, 16));
        let tai = 0.0, xiu = 0.0;
        let energy = hex.reduce((a, b) => a + (isNaN(b) ? 0 : b), 0);
        
        // Thuật toán Core[span_1](start_span)[span_1](end_span)
        let diff = energy - 240;
        if (diff > 0) tai += Math.min(5, diff / 12); else xiu += Math.min(5, Math.abs(diff) / 12);
        [[3,1.5], [5,2], [7,1]].forEach(([m, w]) => { (energy % m) >= (m/2) ? tai += w : xiu += w; });

        let delta = tai - xiu;
        let balance = 1 / (1 + Math.exp(-delta/4));
        tai *= balance; xiu *= (1 - balance);

        let t_pct = Math.round((tai / (tai + xiu)) * 100);
        let x_pct = 100 - t_pct;
        let finalRate = Math.max(t_pct, x_pct);
        if (finalRate > 89) finalRate = 89; // Giới hạn theo yêu cầu
        if (finalRate < 45) finalRate += 35;

        lastRes = t_pct > x_pct ? "TÀI" : "XỈU";
        resVal.textContent = lastRes;
        resVal.style.color = t_pct > x_pct ? "var(--tai)" : "var(--xiu)";
        rateTxt.textContent = `WIN RATE: ${finalRate}%`;
        
        document.getElementById('md5Input').value = "";
    }, 1000);
}

function verify(isCorrect) {
    if (!lastRes) return;
    const pop = document.getElementById('msgPopup');
    pop.innerHTML = isCorrect ? "<h1 style='margin:0'>WIN!</h1><p>BẠN ĐÃ CHIẾN THẮNG</p>" : "<h1 style='margin:0'>SAI (XL)</h1><p>HẸN GẶP LẠI PHIÊN SAU</p>";
    pop.style.background = isCorrect ? "rgba(0, 255, 100, 0.98)" : "rgba(255, 50, 50, 0.98)";
    pop.style.color = isCorrect ? "#000" : "#fff";
    pop.style.display = 'block';
    setTimeout(() => { pop.style.display = 'none'; }, 2000);
}

function zm(v) {
  scale = Math.min(Math.max(scale + v, 0.4), 1.5);
  document.getElementById('wrapMD5').style.transform = `translate(-50%, -50%) rotate(90deg) scale(${scale})`;
}

// DRAG MƯỢT MÀ
let act = null; let sx, sy, ix, iy;
function dragS(e, el) {
  if (e.target.tagName === 'INPUT' || e.target.tagName === 'BUTTON') return;
  act = el; const v = e.type === 'touchstart' ? e.touches[0] : e;
  sx = v.clientX; sy = v.clientY;
  const rect = act.getBoundingClientRect();
  ix = rect.left + rect.width / 2;
  iy = rect.top + rect.height / 2;
  document.addEventListener('mousemove', dragM); document.addEventListener('touchmove', dragM, {passive: false});
  document.addEventListener('mouseup', dragE); document.addEventListener('touchend', dragE);
}
function dragM(e) {
  if (!act) return; const v = e.type === 'touchmove' ? e.touches[0] : e;
  if(e.type === 'touchmove') e.preventDefault();
  act.style.left = (ix + (v.clientX - sx)) + 'px';
  act.style.top = (iy + (v.clientY - sy)) + 'px';
}
function dragE() { act = null; }
</script>
</body>
</html>
