# android-prankk
<!DOCTYPE html>
<html>
<head>
<title>Android System</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
html,body{margin:0;background:#000;color:#0f0;font-family:Arial;overflow:hidden}
#screen{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;flex-direction:column;text-align:center}
#circle{width:120px;height:120px;border:6px solid #0f0;border-top-color:transparent;border-radius:50%;animation:spin 1.2s linear infinite}
@keyframes spin{100%{transform:rotate(360deg)}}
#title{margin-top:18px;font-size:20px}
#sub{font-size:13px;opacity:.8}
#percent{font-size:26px;margin-top:8px}
#hint{position:fixed;top:10px;right:12px;font-size:10px;opacity:.2}
#toast{display:none;position:fixed;bottom:18px;left:50%;transform:translateX(-50%);
background:#111;color:#fff;padding:8px 14px;border-radius:12px;font-size:13px}
#alert{display:none;position:fixed;inset:0;background:#000;color:#f33;align-items:center;justify-content:center;text-align:center}
#boxAlert{border:2px solid #f33;padding:18px;border-radius:12px}
#restart{display:none;position:fixed;inset:0;background:#000;color:#0f0;align-items:center;justify-content:center;flex-direction:column}
#recount{font-size:28px;margin-top:10px}
#overheat{display:none;position:fixed;inset:0;background:#200;color:#ffb}
#overheatBox{position:absolute;bottom:0;left:0;right:0;background:#300;padding:18px;text-align:center}
#battery{position:fixed;top:10px;left:10px;font-size:12px;opacity:.9}
#storage{position:fixed;top:28px;left:10px;font-size:12px;opacity:.9}
.beep{display:none}
</style>
</head>
<body onclick="document.documentElement.requestFullscreen()">

<div id="screen">
  <div id="circle"></div>
  <div id="title">Updating Android System</div>
  <div id="percent">1%</div>
  <div id="sub">Do not turn off your device</div>
</div>

<div id="toast">System Busy…</div>

<div id="alert">
  <div id="boxAlert">
    <h2>UPDATE FAILED</h2>
    <p>Critical system error detected</p>
    <p>Code: 0xA0D11</p>
  </div>
</div>

<div id="restart">
  <div>RESTARTING...</div>
  <div id="recount">10</div>
</div>

<div id="overheat">
  <div id="overheatBox">
    ⚠ DEVICE OVERHEATING <br>
    Temperature: <span id="temp">43</span>°C<br>
    Shutting down apps...
  </div>
</div>

<div id="battery">🔋 Battery: <span id="batt">48</span>%</div>
<div id="storage">💾 Storage: <span id="stor">94</span>% Full</div>

<div id="hint">SAFE MODE ACTIVE</div>

<audio id="beep" class="beep">
  <source src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAESsAACJWAAACABAAZGF0YQAAAAA=" type="audio/wav">
</audio>

<script>
let p=1, phase=0, typed="", batt=48, stor=94, temp=43;
const percent=o("percent"), title=o("title"), toast=o("toast"),
alertBox=o("alert"), restart=o("restart"), recount=o("recount"),
overheat=o("overheat"), tempEl=o("temp"), battEl=o("batt"), storEl=o("stor"),
beep=o("beep");

function o(id){return document.getElementById(id)}

// Fake progress
let prog = setInterval(()=>{
  if(phase!==0) return;
  p += Math.random()*3;
  if(p>=100){p=100; clearInterval(prog); setTimeout(()=>phase1(),1300)}
  percent.innerText = Math.floor(p)+"%";
},420);

// Phase 1: Update failed
function phase1(){
  phase=1;
  alertBox.style.display="flex";
  playBeep();
  setTimeout(()=>{alertBox.style.display="none"; phase2()},3000);
}

// Phase 2: Restart loop
function phase2(){
  phase=2;
  restart.style.display="flex";
  let c=10;
  recount.innerText=c;
  let t=setInterval(()=>{
    c--; recount.innerText=c;
    if(c<=0){ clearInterval(t); restart.style.display="none"; phase3() }
  },1000);
}

// Phase 3: Overheat
function phase3(){
  phase=3;
  overheat.style.display="block";
  let th=setInterval(()=>{
    temp+=1; tempEl.innerText=temp;
    if(temp>=55){ clearInterval(th); setTimeout(()=>phase4(),1800) }
  },700);
}

// Phase 4: Back to update (loop)
function phase4(){
  overheat.style.display="none";
  phase=0;
  p=1; percent.innerText="1%";
  title.innerText="Repairing Android System";
  prog=setInterval(()=>{
    if(phase!==0) return;
    p+=Math.random()*4;
    if(p>=100){p=100; clearInterval(prog); setTimeout(()=>phase1(),1300)}
    percent.innerText=Math.floor(p)+"%";
  },400);
}

// Battery & Storage drain illusion
setInterval(()=>{
  batt = Math.max(1, batt - (Math.random()<.6?1:0));
  stor = Math.min(99, stor + (Math.random()<.5?1:0));
  battEl.innerText=batt; storEl.innerText=stor;
},2200);

// Fake block keys & back
document.addEventListener("keydown", e=>{
  toastOn();
  typed += e.key;
  if(typed.includes("9900")) exitPrank();
});
history.pushState(null,null,location.href);
window.addEventListener("popstate",()=>{ toastOn(); history.pushState(null,null,location.href) });

function toastOn(){
  toast.style.display="block";
  setTimeout(()=>toast.style.display="none",1200);
}

function playBeep(){
  try{beep.currentTime=0;beep.play()}catch(e){}
}

function exitPrank(){
  alert("PRANK DIMATIKAN ✅");
  location.reload();
}

// Vibration (mobile)
navigator.vibrate && setInterval(()=>navigator.vibrate(60),5000);
</script>
</body>
</html>
