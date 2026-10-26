[roulette_3d.html](https://github.com/user-attachments/files/33949005/roulette_3d.html)
# my-website
ルーレット
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>3Dルーレット</title>
<link href="https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Rye&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
:root{--gold:#ffc83d;color-scheme:dark;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
*{box-sizing:border-box}
html,body{margin:0;height:100%}
body{background:radial-gradient(circle at 50% 40%,#3a1470,#12082a 70%);color:#fff6e0;font-family:"Hiragino Sans","Yu Gothic",Meiryo,sans-serif;
display:flex;flex-direction:column;align-items:center;justify-content:safe center;gap:8px;overflow-x:hidden;overflow-y:auto;padding:8px 0 100px}
h1{font:400 clamp(20px,5vw,32px)/1.1 Rye,Georgia,serif;margin:0;z-index:3;text-shadow:0 0 6px #fff,0 0 18px #ff2a45,0 0 40px #ff2a45}
#stage{position:relative;width:min(98vw,78vh,900px);aspect-ratio:1/.8;z-index:3;border-radius:16px}
#stage canvas{width:100%;height:100%;display:block}
#msg{min-height:1.4em;font:400 clamp(18px,4.6vw,26px)/1.2 "Dela Gothic One",sans-serif;text-align:center;z-index:3;text-shadow:0 2px 6px #000}
#legend{font-size:12px;opacity:.85;text-align:center;z-index:3}
#go{font:400 24px Rye,Georgia,serif;padding:12px 46px;border:4px dashed #fff;border-radius:999px;z-index:3;background:radial-gradient(circle at 50% 30%,#f0314b,#b10f26);
color:#fff;cursor:pointer;box-shadow:0 6px 0 #5e0916,0 10px 18px #0008}
#go:active{transform:translateY(4px)}#go:disabled{opacity:.55}
button:focus-visible{outline:3px solid var(--gold);outline-offset:3px}
#fx{position:fixed;inset:0;width:100%;height:100%;pointer-events:none;z-index:6}
#bulbs{position:fixed;inset:6px;border:8px dotted #ffd54a;border-radius:20px;pointer-events:none;z-index:4;animation:blink 1s steps(2) infinite}
#vig{position:fixed;inset:0;pointer-events:none;opacity:0;transition:opacity .25s;z-index:2;background:radial-gradient(circle,transparent 30%,#ff1a3aaa 100%)}
#flash{position:fixed;inset:0;background:#fff;opacity:0;pointer-events:none;z-index:12}
#banner,#banner2{position:fixed;left:50%;top:38%;transform:translate(-50%,-50%) scale(0);z-index:8;pointer-events:none;white-space:nowrap;
font:400 clamp(44px,15vw,130px)/1 "Dela Gothic One",sans-serif;-webkit-text-stroke:3px #3a0610;paint-order:stroke fill;
background:linear-gradient(100deg,#f00,#ff0,#0f0,#0ff,#00f,#f0f,#f00);background-size:250% 100%;-webkit-background-clip:text;background-clip:text;color:transparent;
filter:drop-shadow(0 0 22px #fff)}
#banner.show,#banner2.show{animation:bpop .9s cubic-bezier(.2,1.6,.4,1) forwards,shine 1.2s linear infinite}
#banner.sad{animation:sad 1.4s forwards;filter:grayscale(1)}
#cutin{position:fixed;left:-5%;right:-5%;top:34%;height:26vh;z-index:9;pointer-events:none;transform:skewY(-6deg) translateX(120%);display:flex;align-items:center;justify-content:center;
background:repeating-linear-gradient(90deg,#fff2 0 6px,#0000 6px 40px),linear-gradient(90deg,#c0001a,#ff7a00,#c0001a);border-block:6px solid #fff;
gap:2vw;font:400 clamp(30px,9vw,84px)/1.1 "Dela Gothic One",sans-serif;color:#fff;-webkit-text-stroke:4px #200;paint-order:stroke fill;text-shadow:0 0 30px #fff}
#cutin svg{width:clamp(120px,30vw,250px);height:auto;margin-top:-10vh;transform:skewY(6deg);filter:drop-shadow(0 6px 10px #0009)}
#cutin.go{animation:cut 1.5s forwards}
#push{position:fixed;left:50%;top:50%;z-index:10;display:none;transform:translate(-50%,-50%);border:0;background:none;cursor:pointer;text-align:center;padding:0}
#push.on{display:block;animation:pshake .12s infinite}
#push .lab{display:block;font:400 clamp(26px,8vw,52px)/1.1 "Dela Gothic One",sans-serif;color:#fff;-webkit-text-stroke:3px #200;paint-order:stroke fill;animation:blink2 .25s steps(2) infinite}
#push .btn{display:block;margin:8px auto;width:clamp(150px,36vmin,260px);aspect-ratio:1;border-radius:50%;border:10px solid #ffc83d;
background:radial-gradient(circle at 40% 30%,#ff8a8a,#e0112b 55%,#7d0014);box-shadow:0 12px 0 #5e0916,0 0 60px 20px #ff2a45;color:#fff;
font:400 clamp(36px,10vw,60px)/clamp(130px,32vmin,230px) Rye,Georgia,serif}
#push:active .btn{transform:translateY(8px);box-shadow:0 4px 0 #5e0916,0 0 60px 20px #ff2a45}
#pbar{width:clamp(150px,36vmin,260px);height:10px;margin:0 auto;background:#0008;border-radius:9px;overflow:hidden}
#pbar i{display:block;height:100%;background:#ffc83d;width:100%}
#pbar.run i{animation:bar linear forwards}
body.reach #vig{opacity:1;animation:pulse .4s infinite alternate}
body.reach #bulbs{animation-duration:.18s;border-color:#ff3b4e}
body.reach #stage{animation:shake .1s infinite}
body.rainbow #vig{opacity:1;background:radial-gradient(circle,transparent 25%,#fff8 100%);animation:hue 1s linear infinite}
body.win #bulbs{animation-duration:.2s}
#banner2{top:64%}
#cutin{left:auto;right:0;top:auto;bottom:0;height:auto;width:clamp(170px,44vw,330px);display:block;transform:translateX(120%);background:none;border:0}
#cutin.go{animation:cut2 1.8s forwards}
#cutin svg{display:block;width:100%;height:auto;margin:0;transform:none;filter:drop-shadow(0 8px 14px #000a)}
#cutin::before{content:"";position:absolute;inset:-6% -30% 0 -30%;z-index:-1;background:repeating-conic-gradient(from 0deg at 55% 75%,#ff2a4599 0 7deg,#ffc83d88 7deg 14deg);-webkit-mask:radial-gradient(circle at 55% 75%,#000 25%,transparent 68%);mask:radial-gradient(circle at 55% 75%,#000 25%,transparent 68%)}
#cutin span{position:absolute;right:62%;bottom:88%;white-space:nowrap;background:#fff;color:#222;border:3px solid #222;border-radius:16px;padding:8px 14px;font:400 clamp(18px,5.4vw,34px)/1.2 "Dela Gothic One",sans-serif;-webkit-text-stroke:0;text-shadow:none}
#npc svg{width:clamp(80px,20vw,130px)}
@keyframes cut2{0%{transform:translateX(120%)}14%,82%{transform:translateX(0)}100%{transform:translateX(120%)}}
#cutin{left:0;right:0;top:0;bottom:0;width:auto;display:flex;align-items:flex-end;justify-content:center;transform:none;opacity:0;background:radial-gradient(circle at 50% 60%,#ff2a4566,#150008dd)}
#cutin.go{animation:cut3 1.8s forwards}
#cutin svg{width:auto;height:100%;max-width:100%}
#cutin::before{inset:0;background:repeating-conic-gradient(from 0deg at 50% 65%,#ff2a4599 0 7deg,#ffc83d88 7deg 14deg);-webkit-mask:radial-gradient(circle at 50% 65%,#000 20%,transparent 75%);mask:radial-gradient(circle at 50% 65%,#000 20%,transparent 75%)}
#cutin span{left:50%;right:auto;top:6%;bottom:auto;transform:translateX(-50%);font-size:clamp(28px,8vw,64px)}
@keyframes cut3{0%{opacity:0;transform:scale(1.35)}10%{opacity:1;transform:scale(1)}82%{opacity:1;transform:scale(1.04)}100%{opacity:0;transform:scale(1.1)}}
#bunny{position:fixed;inset:0;z-index:5;pointer-events:none;display:none;align-items:flex-end;justify-content:center;background:radial-gradient(circle at 50% 60%,#ff7ab866,#1a0830dd)}
#bunny.go{display:flex;animation:bun 2.6s forwards}
#bunny svg{width:auto;height:100%;max-width:100%}
#bunny span{position:absolute;left:50%;top:7%;transform:translateX(-50%);white-space:nowrap;background:#fff;color:#c2185b;border:3px solid #ff7ab8;border-radius:18px;padding:8px 16px;font:400 clamp(24px,7vw,56px)/1.2 "Dela Gothic One",sans-serif}
@keyframes bun{0%{opacity:0;transform:translateY(40%)}12%{opacity:1;transform:translateY(0)}24%,48%,72%{transform:translateY(-2.5%)}36%,60%,84%{transform:translateY(0)}90%{opacity:1}100%{opacity:0}}
#npc{position:fixed;left:6px;bottom:6px;z-index:5;pointer-events:none;display:flex;align-items:flex-end;gap:4px}
#npc svg{width:clamp(64px,17vw,110px);height:auto}
#npc .bub{background:#fff;color:#222;border-radius:14px;padding:6px 10px;font:700 13px "Hiragino Sans",sans-serif;max-width:46vw;margin-bottom:46px;box-shadow:0 2px 8px #0007}
.row{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;z-index:3;max-width:96vw}
.row button,.row select{font:700 14px "Hiragino Sans",sans-serif;padding:6px 12px;border-radius:999px;border:2px solid #ffc83d;background:#241046;color:#fff6e0;cursor:pointer}
.row [aria-pressed=true]{background:#ffc83d;color:#2a1a00}
#bank{z-index:3;font:700 18px "Hiragino Sans",sans-serif}#bank b{color:#ffc83d;font-size:24px}
#hist{display:flex;gap:4px;z-index:3;min-height:24px}
#hist i{font:700 12px/24px Georgia,serif;width:26px;height:24px;border-radius:50%;text-align:center;font-style:normal;color:#fff}
#cardT{z-index:3;font:700 13px "Hiragino Sans",sans-serif;text-align:center;max-width:94vw}
#card{display:grid;grid-template-columns:repeat(3,58px);gap:5px;z-index:3}
#card b{height:50px;border-radius:10px;background:#241046;border:2px solid #6a4fb0;display:flex;align-items:center;justify-content:center;font:700 20px Georgia,serif;cursor:pointer}
#card b.m{background:radial-gradient(circle,#ffe27a,#e8a010);color:#2a1a00;border-color:#fff}
#card b.r{animation:blink2 .5s steps(2) infinite;border-color:#ff3b4e}
@keyframes blink{50%{opacity:.25}}@keyframes blink2{50%{opacity:.4}}@keyframes pulse{to{opacity:.4}}
@keyframes hue{to{filter:hue-rotate(360deg)}}@keyframes shine{to{background-position:-250% 0}}
@keyframes bpop{0%{transform:translate(-50%,-50%) scale(0) rotate(-12deg)}100%{transform:translate(-50%,-50%) scale(1)}}
@keyframes sad{0%{transform:translate(-50%,-50%) scale(.7);opacity:1}100%{transform:translate(-50%,-30%) scale(.7);opacity:0}}
@keyframes cut{0%{transform:skewY(-6deg) translateX(120%)}18%,78%{transform:skewY(-6deg) translateX(0)}100%{transform:skewY(-6deg) translateX(-120%)}}
@keyframes bar{to{width:0}}@keyframes flash{0%{opacity:.95}100%{opacity:0}}
@keyframes pshake{0%{transform:translate(-50%,-50%) rotate(-1.5deg)}50%{transform:translate(-50%,-50%) rotate(1.5deg)}100%{transform:translate(-50%,-50%) rotate(-1.5deg)}}
@keyframes shake{0%{transform:translate(3px,1px)}25%{transform:translate(-3px,-1px)}50%{transform:translate(1px,-3px)}75%{transform:translate(-1px,3px)}100%{transform:translate(3px,-1px)}}
@media (prefers-reduced-motion:reduce){body.reach #stage,#push.on{animation:none}}
</style>
</head>
<body>
<div id="vig"></div><div id="bulbs"></div>
<h1>★ 3D ROULETTE ★</h1>
<div id="stage"></div>
<div id="msg">ボールを投げ入れよう</div>
<div id="bank">💰 <b id="bal">1000</b> チップ</div>
<div id="hist"></div>
<div class="row" id="bets"></div>
<div class="row" id="amts"></div>
<button id="go">SPIN</button>
<div id="cardT">BINGO：出た数字を消そう（1ライン+300／全部+2000）数字をタップでその数字に賭ける</div>
<div id="card"></div>
<div id="legend">★ 0・00 … 大当たり　★ 7・11・17・21・27・33 … 当たり</div>
<canvas id="fx"></canvas><div id="flash"></div><div id="banner"></div><div id="cutin"></div><div id="bunny"></div><div id="banner2"></div>
<button id="push" aria-label="ボタンを押せ"><span class="lab">ボタンを押せ!!</span><span class="btn">PUSH</span><div id="pbar"><i></i></div></button>
<script>
const ORDER=["0","28","9","26","30","11","7","20","32","17","5","22","34","15","3","24","36","13","1","00","27","10","25","29","12","8","19","31","18","6","21","33","16","4","23","35","14","2"];
const REDS=new Set([1,3,5,7,9,12,14,16,18,19,21,23,25,27,30,32,34,36]),LUCKY=new Set(["7","11","17","21","27","33"]);
const N=ORDER.length,S=360/N,rad=d=>d*Math.PI/180;
const colorOf=n=>n==="0"||n==="00"?"green":REDS.has(+n)?"red":"black";
const kindOf=n=>n==="0"||n==="00"?2:LUCKY.has(n)?1:0;
const $=id=>document.getElementById(id);
const stage=$("stage"),fx=$("fx"),fc=fx.getContext("2d"),msg=$("msg"),btn=$("go"),banner=$("banner"),flash=$("flash"),cutin=$("cutin"),push=$("push"),pbar=$("pbar");

/* ---------- three.js ---------- */
const renderer=new THREE.WebGLRenderer({antialias:true,alpha:true});
renderer.setPixelRatio(Math.min(devicePixelRatio||1,2));stage.appendChild(renderer.domElement);
const scene=new THREE.Scene(),camera=new THREE.PerspectiveCamera(38,1,.1,60);
scene.add(new THREE.AmbientLight(0xffffff,.6));
const sun=new THREE.DirectionalLight(0xfff0d0,.9);sun.position.set(4,9,4);scene.add(sun);
const glow=new THREE.PointLight(0xffc83d,.7,14);glow.position.set(0,3,0);scene.add(glow);
function woodTex(){const o=document.createElement("canvas");o.width=o.height=256;const x=o.getContext("2d");
  x.fillStyle="#4a2410";x.fillRect(0,0,256,256);
  for(let i=0;i<180;i++){x.strokeStyle=`hsla(${16+Math.random()*8},60%,${12+Math.random()*16}%,.55)`;x.lineWidth=1+Math.random()*2;
    const y=Math.random()*256;x.beginPath();x.moveTo(0,y);x.bezierCurveTo(80,y+Math.random()*10-5,170,y+Math.random()*10-5,256,y);x.stroke();}
  const t=new THREE.CanvasTexture(o);t.wrapS=t.wrapT=THREE.RepeatWrapping;return t;}
const wood=woodTex();
const chromeM=new THREE.MeshPhongMaterial({color:0xdde3ea,specular:0xffffff,shininess:140});
const goldM=new THREE.MeshPhongMaterial({color:0xe8b030,specular:0xffffff,shininess:100});
// bowl
const bp=[[0,-.3],[3.5,-.3],[3.5,.55],[3.35,.66],[3.1,.55],[3.0,.3],[2.55,.25],[2.2,.08],[2.1,.08]].map(p=>new THREE.Vector2(p[0],p[1]));
const bowlM=new THREE.Mesh(new THREE.LatheGeometry(bp,96),new THREE.MeshPhongMaterial({map:wood,specular:0x884422,shininess:60,side:THREE.DoubleSide}));
scene.add(bowlM);
[[3.43,.6,.06],[3.02,.48,.035],[2.2,.1,.03]].forEach(([r,y,t])=>{const m=new THREE.Mesh(new THREE.TorusGeometry(r,t,12,96),chromeM);m.rotation.x=Math.PI/2;m.position.y=y;scene.add(m);});
for(let k=0;k<8;k++){const m=new THREE.Mesh(new THREE.OctahedronGeometry(.13),chromeM),a=rad(k*45+22.5);
  m.scale.set(.7,1.2,1.4);m.position.set(2.4*Math.sin(a),.22,-2.4*Math.cos(a));m.rotation.y=-a;scene.add(m);}
// wheel
const wheel=new THREE.Group();scene.add(wheel);
function faceTex(){const o=document.createElement("canvas");o.width=o.height=1024;const x=o.getContext("2d");x.translate(512,512);
  let g=x.createRadialGradient(0,0,0,0,0,512);g.addColorStop(0,"#d99556");g.addColorStop(.7,"#8f4f22");g.addColorStop(1,"#3e1d0b");
  x.beginPath();x.arc(0,0,512,0,7);x.fillStyle=g;x.fill();
  const r1=498,r0=330;
  for(let i=0;i<N;i++){const col=colorOf(ORDER[i]);x.beginPath();x.arc(0,0,r1,rad(i*S-90),rad((i+1)*S-90));x.arc(0,0,r0,rad((i+1)*S-90),rad(i*S-90),true);x.closePath();
    g=x.createRadialGradient(0,0,r0,0,0,r1);const[A,B]=col==="red"?["#7d0a1b","#e0162f"]:col==="black"?["#050505","#303036"]:["#0a5a2a","#16a34a"];
    g.addColorStop(0,A);g.addColorStop(1,B);x.fillStyle=g;x.fill();}
  x.strokeStyle="#e6eaef";x.lineWidth=5;
  for(let i=0;i<N;i++){x.save();x.rotate(rad(i*S));x.beginPath();x.moveTo(0,-r0);x.lineTo(0,-r1);x.stroke();x.restore();}
  x.lineWidth=7;x.beginPath();x.arc(0,0,r0,0,7);x.stroke();x.beginPath();x.arc(0,0,r1,0,7);x.stroke();
  x.textAlign="center";x.textBaseline="middle";
  for(let i=0;i<N;i++){const n=ORDER[i],k=kindOf(n);x.save();x.rotate(rad((i+.5)*S));x.fillStyle="#fff";x.font="bold 56px Georgia,serif";x.fillText(n,0,-430);
    if(k){x.fillStyle=k===2?"#fff":"#ffc83d";x.font="40px sans-serif";x.fillText("★",0,-360);}x.restore();}
  const t=new THREE.CanvasTexture(o);t.anisotropy=8;return t;}
const face=new THREE.Mesh(new THREE.CircleGeometry(2.15,96),new THREE.MeshPhongMaterial({map:faceTex(),shininess:80,specular:0x666666}));
face.rotation.x=-Math.PI/2;face.position.y=.08;wheel.add(face);
const edge=new THREE.Mesh(new THREE.CylinderGeometry(2.15,2.15,.3,96,1,true),goldM);edge.position.y=-.05;wheel.add(edge);
const cone=new THREE.Mesh(new THREE.LatheGeometry([[1.35,0],[1.2,.06],[.8,.2],[.4,.38],[.18,.5],[.1,.54]].map(p=>new THREE.Vector2(p[0],p[1])),64),
  new THREE.MeshPhongMaterial({map:wood,color:0xd9a070,specular:0xaa7744,shininess:60,side:THREE.DoubleSide}));cone.position.y=.08;wheel.add(cone);
const hub=new THREE.Mesh(new THREE.SphereGeometry(.17,24,16),chromeM);hub.position.y=.68;wheel.add(hub);
for(let k=0;k<2;k++){const s=new THREE.Mesh(new THREE.CylinderGeometry(.03,.03,.9,10),goldM);s.rotation.z=Math.PI/2;s.rotation.y=k*Math.PI/2;s.position.y=.7;wheel.add(s);}
for(let k=0;k<4;k++){const b=new THREE.Mesh(new THREE.SphereGeometry(.07,12,10),goldM),a=k*Math.PI/2;b.position.set(.45*Math.cos(a),.7,.45*Math.sin(a));wheel.add(b);}
const ballR=.09,ballM=new THREE.Mesh(new THREE.SphereGeometry(ballR,24,16),new THREE.MeshPhongMaterial({color:0xfaf5ea,specular:0xffffff,shininess:150}));
ballM.visible=false;scene.add(ballM);

function resize(){const w=stage.clientWidth,h=stage.clientHeight;renderer.setSize(w,h);camera.aspect=w/h;camera.updateProjectionMatrix();
  const d=Math.min(devicePixelRatio||1,2);fx.width=innerWidth*d;fx.height=innerHeight*d;}
const camBase=new THREE.Vector3(0,6.4,6.3),lookBase=new THREE.Vector3(0,0,0),camNow=camBase.clone(),lookNow=lookBase.clone(),camT=new THREE.Vector3(),lookT=new THREE.Vector3();
let theta=0,st=null,fovKick=0;
const gy=r=>r>2.55?.25:r>2.2?.08+.17*(r-2.2)/.35:.08;

/* ---------- sound / fx ---------- */
let ac;
function beep(f,d=.06,v=.06,type="square"){try{ac=ac||new (window.AudioContext||window.webkitAudioContext)();
  const o=ac.createOscillator(),g=ac.createGain();o.type=type;o.frequency.value=f;g.gain.value=v;g.gain.exponentialRampToValueAtTime(.0001,ac.currentTime+d);
  o.connect(g).connect(ac.destination);o.start();o.stop(ac.currentTime+d);}catch(e){}}
const PAL=["#ffc83d","#ff2a45","#ffffff","#3b78ff","#3bff9a","#ff7bd5"];let parts=[];
const dp=()=>Math.min(devicePixelRatio||1,2);
function addSpark(x,y,col,sp=10,life=50){const a=Math.random()*6.283,v=Math.random()*sp*dp();parts.push({x,y,vx:Math.cos(a)*v,vy:Math.sin(a)*v,life,max:life,c:col,t:0,s:(2+Math.random()*3)*dp(),g:.18*dp()});}
function addConfetti(){parts.push({x:Math.random()*fx.width,y:-20,vx:(Math.random()-.5)*4*dp(),vy:(2+Math.random()*4)*dp(),life:260,max:260,c:PAL[Math.random()*6|0],t:1,s:(5+Math.random()*7)*dp(),g:.05*dp(),rot:0,vr:(Math.random()-.5)*.4});}
function burst(x,y){const c=PAL[Math.random()*6|0];for(let i=0;i<90;i++)addSpark(x,y,c,13,70);beep(180+Math.random()*80,.3,.07,"sawtooth");}
function loopFx(){fc.clearRect(0,0,fx.width,fx.height);fc.globalCompositeOperation="lighter";
  parts=parts.filter(p=>--p.life>0&&p.y<fx.height+30);
  parts.forEach(p=>{p.vy+=p.g;p.x+=p.vx;p.y+=p.vy;p.vx*=.985;fc.globalAlpha=Math.min(1,p.life/p.max*1.5);fc.fillStyle=p.c;
    if(p.t){fc.save();fc.translate(p.x,p.y);p.rot+=p.vr;fc.rotate(p.rot);fc.fillRect(-p.s/2,-p.s/3,p.s,p.s*.66);fc.restore();}else{fc.beginPath();fc.arc(p.x,p.y,p.s,0,7);fc.fill();}});
  fc.globalAlpha=1;fc.globalCompositeOperation="source-over";requestAnimationFrame(loopFx);}
const doFlash=()=>{flash.style.animation="none";void flash.offsetWidth;flash.style.animation="flash .7s ease-out forwards";};
function screenPos(v){const p=v.clone().project(camera),r=renderer.domElement.getBoundingClientRect(),d=dp();
  return[(r.left+(p.x*.5+.5)*r.width)*d,(r.top+(-p.y*.5+.5)*r.height)*d];}

/* ---------- game ---------- */
const idxOf=(ph,th)=>Math.floor((((ph-th)%360)+360)%360/S);
const sm=x=>x*x*(3-2*x);
function showPush(ms){push.classList.add("on");pbar.classList.remove("run");void pbar.offsetWidth;pbar.firstElementChild.style.animationDuration=ms+"ms";pbar.classList.add("run");
  beep(880,.15,.1);setTimeout(()=>beep(1175,.2,.1),160);say("押して！！","hot");}
function hidePush(){push.classList.remove("on");}
function press(){
  if(!st||!push.classList.contains("on"))return;hidePush();st.pressed=true;
  if(st.win){doFlash();banner.textContent="激アツ!!";banner.className="";void banner.offsetWidth;banner.className="show";document.body.classList.add("rainbow");
    for(let b=0;b<4;b++)setTimeout(()=>burst(fx.width*(.15+Math.random()*.7),fx.height*(.15+Math.random()*.4)),b*160);
    [660,880,1100,1320].forEach((f,k)=>setTimeout(()=>beep(f,.2,.09,"triangle"),k*80));fovKick=1;navigator.vibrate&&navigator.vibrate([150,50,150]);
    msg.textContent="これは来たか！！";say("きたーっ！！","hot");setTimeout(()=>{if(st)banner.className="";},1600);
  }else{st.sup=true;document.body.classList.remove("reach");banner.textContent="ガセ…";banner.className="";void banner.offsetWidth;banner.className="sad";
    msg.textContent="…ガセだった";say("あれ〜…","sad");beep(160,.5,.08,"sawtooth");}
}
push.addEventListener("click",press);
addEventListener("keydown",e=>{if((e.code==="Space"||e.code==="Enter")&&push.classList.contains("on")){e.preventDefault();press();}});

function spin(){
  if(st)return;if(ac&&ac.state==="suspended")ac.resume();
  if(bet.t==="num"&&!bet.n){msg.textContent="数字を選んでね";return;}
  if(bal<=0){bal=1000;updBal();msg.textContent="チップ補充！1000枚で再スタート";say("もう一回遊ぼ！");return;}
  const stake=Math.min(amt,bal);bal-=stake;updBal();btn.disabled=true;
  document.body.classList.remove("win","reach","rainbow");banner.className="";hidePush();say("いくよ〜！");
  msg.textContent="ボールが回ってる…";
  const target=Math.floor(Math.random()*N),win=kindOf(ORDER[target])>0,pc=(target+.5)*S,mult=payout(ORDER[target]);
  const th0=theta,thF=th0+360*5+Math.random()*360,raw=thF+pc,m=Math.round((raw+360*9)/360),phF=raw-360*m;
  st={t0:performance.now(),D:14000,th0,thF,phF,target,win:win||mult>0,mult,stake,betWon:mult>0,teaser:win||mult>0||Math.random()<.4,pushed:false,pressed:false,sup:false,reach:false,cut:false,lastIdx:-1,lastBeat:0,lastTick:0};
  ballM.visible=true;
}
function finish(){
  const mult=st.mult,stake=st.stake,betWon=st.betWon,i=st.target,n=ORDER[i],k=kindOf(n),jp={green:"グリーン",red:"レッド",black:"ブラック"}[colorOf(n)];
  st=null;btn.disabled=false;btn.textContent="AGAIN";document.body.classList.remove("reach");hidePush();
  const won=Math.round(stake*mult);bal+=won;afterResult(n,won>0||k>0);
  if(k||betWon){const big=k===2||mult>=18;document.body.classList.add("win");msg.textContent=`${n} ${jp}！ `+(k?(big?"大当たり！！":"当たり！"):"的中！")+(won?` +${won-stake}`:"");
    banner.textContent=k===2?"大当たり!!":k?"当たり!!":"的中!!";banner.className="";void banner.offsetWidth;banner.className="show";doFlash();
    [523,659,784,1047,1319,1568].forEach((f,j)=>setTimeout(()=>beep(f,.3,.09,"triangle"),j*100));
    for(let b=0;b<(big?14:k?7:4);b++)setTimeout(()=>burst(fx.width*(.1+Math.random()*.8),fx.height*(.12+Math.random()*.4)),b*260);
    for(let j=0;j<(big?260:120);j++)setTimeout(addConfetti,j*(big?18:25));
    navigator.vibrate&&navigator.vibrate([250,80,250,80,400]);
    setTimeout(()=>{document.body.classList.remove("win","rainbow");banner.className="";},big?7000:5000);
  }else{document.body.classList.remove("rainbow");msg.textContent=`${n} ${jp}… -${stake}`;showBunny();beep(180,.4,.07,"sawtooth");}
}
function update(now){
  const p=Math.min((now-st.t0)/st.D,1);
  theta=st.th0+(st.thF-st.th0)*(1-Math.pow(1-p,2.3));
  let ph=st.phF*(1-Math.pow(1-p,3)),r,hop=0,q=0;
  if(p<.5)r=2.78;else if(p<.72)r=2.78-1*sm((p-.5)/.22);
  else{q=(p-.72)/.28;const e=Math.abs(Math.sin(q*Math.PI*7))*Math.pow(1-q,2);r=1.78+.25*e;hop=.22*e;ph+=Math.pow(1-q,2)*Math.sin(q*30)*S*1.3;}
  const a=rad(ph);ballM.position.set(r*Math.sin(a),gy(r)+ballR+hop,-r*Math.cos(a));
  ballM.rotation.x+=.3;
  if(p<.5&&now-st.lastTick>90){st.lastTick=now;beep(300,.03,.02);}
  const i=idxOf(ph,theta);if(p>=.5&&i!==st.lastIdx)beep(700+Math.random()*500,.05,.07,"triangle");st.lastIdx=i;
  if(p>=.95&&!st.win)st.sup=true;
  const on=st.teaser&&!st.sup&&p>.72&&p<1;
  if(on&&!st.cut){st.cut=true;cutin.innerHTML=charaSVG("hot").replace("<svg ",'<svg preserveAspectRatio="xMidYMax meet" ')+"<span>"+["来るよ…！","いい感じ！","熱いよ！！","当たるかも！？"][Math.random()*4|0]+"</span>";cutin.className="";void cutin.offsetWidth;cutin.className="go";beep(110,.5,.15,"sawtooth");navigator.vibrate&&navigator.vibrate([80,40,80,40,80]);}
  if(on&&!st.reach){st.reach=true;document.body.classList.add("reach");msg.textContent="当たりか！？";}
  if(!on&&st.reach){st.reach=false;document.body.classList.remove("reach");if(!st.pressed)msg.textContent="…";}
  if(st.teaser&&!st.pushed&&p>=.77){st.pushed=true;showPush(Math.round(st.D*.18));}
  if(push.classList.contains("on")&&p>=.95)hidePush();
  if(on){const[sx,sy]=screenPos(ballM.position);for(let k=0;k<3;k++)addSpark(sx,sy,Math.random()<.5?"#ffc83d":"#ff3b4e",6,35);
    if(now-st.lastBeat>380){st.lastBeat=now;beep(70,.18,.14,"sine");setTimeout(()=>beep(60,.18,.12,"sine"),130);}}
  if(p>=1)finish();
}
function tick(now){
  if(st)update(now);
  wheel.rotation.y=-rad(theta);
  const zoom=st&&st.reach;
  if(zoom){const b=ballM.position;camT.set(b.x*.5,2.6,b.z*.5+3);lookT.set(b.x,.2,b.z);}else{camT.copy(camBase);lookT.copy(lookBase);}
  camNow.lerp(camT,.05);lookNow.lerp(lookT,.08);camera.position.copy(camNow);camera.lookAt(lookNow);
  fovKick*=.9;camera.fov=38+fovKick*14;camera.updateProjectionMatrix();
  glow.intensity=zoom?1.6+Math.sin(now/60)*.8:.7;glow.color.set(zoom?0xff3040:0xffc83d);
  renderer.render(scene,camera);requestAnimationFrame(tick);
}
/* ---- character ---- */
function charaSVG(m="n"){
 const hot=m==="hot",sad=m==="sad";
 const eye=x=>sad?`<path d="M${x-9} 106Q${x} 98 ${x+9} 106" stroke="#222" stroke-width="3" fill="none"/>`:`<ellipse cx="${x}" cy="104" rx="10" ry="${hot?14:11}" fill="#fff" stroke="#222" stroke-width="2"/><circle cx="${x}" cy="105" r="${hot?8:6.5}" fill="#7a2cff"/><circle cx="${x+3}" cy="${hot?100:101}" r="2.6" fill="#fff"/>`;
 const mouth=hot?`<ellipse cx="100" cy="136" rx="10" ry="11" fill="#a3122a"/><ellipse cx="100" cy="141" rx="6" ry="4" fill="#ff7a8a"/>`:sad?`<path d="M89 140Q100 130 111 140" stroke="#a3122a" stroke-width="3" fill="none"/>`:`<path d="M88 132Q100 144 112 132" stroke="#a3122a" stroke-width="3" fill="none"/>`;
 return `<svg viewBox="0 0 200 240" xmlns="http://www.w3.org/2000/svg"><ellipse cx="100" cy="100" rx="74" ry="86" fill="#3b1d5a"/><ellipse cx="38" cy="150" rx="16" ry="48" fill="#3b1d5a"/><ellipse cx="162" cy="150" rx="16" ry="48" fill="#3b1d5a"/><path d="M36 240Q42 172 100 166Q158 172 164 240Z" fill="#1d1d2b"/><path d="M80 168L100 220L120 168Z" fill="#fff"/><rect x="88" y="150" width="24" height="24" fill="#ffe0c8"/><ellipse cx="100" cy="102" rx="48" ry="54" fill="#ffe0c8"/><path d="M50 98Q56 40 100 38Q146 40 150 98Q130 64 100 68Q70 64 50 98Z" fill="#3b1d5a"/><path d="M52 72Q100 46 148 72L148 80Q100 56 52 80Z" fill="#14a060"/>${eye(80)}${eye(120)}<circle cx="68" cy="124" r="7" fill="#ff9aa8" opacity=".6"/><circle cx="132" cy="124" r="7" fill="#ff9aa8" opacity=".6"/>${mouth}<path d="M88 176L100 184L88 194ZM112 176L100 184L112 194Z" fill="#d4142f"/><circle cx="100" cy="185" r="4" fill="#a3122a"/></svg>`;}
function charaSVG(m="n"){
 const hot=m==="hot",sad=m==="sad",bd=hot?-6:sad?3:0;
 const eye=x=>`<path d="M${x-12} 99Q${x} ${hot?88:93} ${x+12} 99Q${x} ${hot?110:105} ${x-12} 99Z" fill="#f4efe6" stroke="#4a3020" stroke-width="1.5"/><circle cx="${x}" cy="${sad?100:99}" r="${hot?6:5.2}" fill="#4a2f1c"/><circle cx="${x}" cy="${sad?100:99}" r="2.4" fill="#111"/><circle cx="${x+2}" cy="${sad?98:97}" r="1.5" fill="#fff"/><path d="M${x-13} 98Q${x} ${hot?86:91} ${x+13} 98" stroke="#2e1d12" stroke-width="2.6" fill="none"/><path d="M${x-11} 107Q${x} 112 ${x+11} 107" stroke="#b9805f" stroke-width="1.4" fill="none" opacity=".8"/>`;
 const brows=`<path d="M64 ${86+bd+(sad?4:0)}Q78 ${79+bd} 93 ${86+bd-(sad?6:0)}M136 ${86+bd+(sad?4:0)}Q122 ${79+bd} 107 ${86+bd-(sad?6:0)}" stroke="#707078" stroke-width="5.5" stroke-linecap="round" fill="none"/>`;
 const mouth=hot?`<path d="M86 150Q100 144 114 150Q112 168 100 168Q88 168 86 150Z" fill="#4e121b"/><path d="M89 151Q100 147 111 151L110 155Q100 152 90 155Z" fill="#f7f2ea"/><ellipse cx="100" cy="164" rx="7" ry="3.5" fill="#d9626f"/>`:sad?`<path d="M87 158Q100 148 113 158" stroke="#7a3b30" stroke-width="2.6" fill="none" stroke-linecap="round"/>`:`<path d="M87 153Q100 160 113 153" stroke="#7a3b30" stroke-width="2.6" fill="none" stroke-linecap="round"/>`;
 return `<svg viewBox="0 0 200 260" xmlns="http://www.w3.org/2000/svg"><defs><radialGradient id="sk" cx=".4" cy=".35" r=".8"><stop offset="0" stop-color="#f6cfae"/><stop offset=".65" stop-color="#e0a67f"/><stop offset="1" stop-color="#b87a58"/></radialGradient><linearGradient id="jk" x1="0" x2="1"><stop offset="0" stop-color="#2d2d3a"/><stop offset=".5" stop-color="#14141b"/><stop offset="1" stop-color="#262631"/></linearGradient><linearGradient id="rd" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#ff4a5e"/><stop offset="1" stop-color="#9c0f22"/></linearGradient></defs>`+
 `<path d="M12 260Q18 192 100 184Q182 192 188 260Z" fill="url(#jk)"/><path d="M70 186L100 244L130 186Z" fill="#f3f1ea"/><path d="M70 186L100 244L100 190Z" fill="#dcd9cf"/><path d="M58 190Q76 186 88 192L100 252L72 260L40 260Q44 214 58 190ZM142 190Q124 186 112 192L100 252L128 260L160 260Q156 214 142 190Z" fill="#0b0b10"/><circle cx="86" cy="236" r="2.6" fill="#e8b030"/><circle cx="114" cy="236" r="2.6" fill="#e8b030"/><path d="M78 150L78 184Q100 200 122 184L122 150Z" fill="#c98f6a"/><path d="M78 168Q100 182 122 168L122 178Q100 192 78 178Z" fill="#a8704f" opacity=".6"/><path d="M82 180L100 196L72 192ZM118 180L100 196L128 192Z" fill="#fff"/><ellipse cx="49" cy="108" rx="8" ry="15" fill="#d99c76"/><ellipse cx="151" cy="108" rx="8" ry="15" fill="#d99c76"/><path d="M52 96Q50 50 100 46Q150 50 148 96Q150 132 128 154Q100 172 72 154Q50 132 52 96Z" fill="url(#sk)"/><path d="M60 128Q70 162 100 168Q130 162 140 128Q128 154 100 158Q72 154 60 128Z" fill="#3a3a40" opacity=".3"/><path d="M51 98Q44 70 58 56Q56 80 62 100ZM149 98Q156 70 142 56Q144 80 138 100Z" fill="#8d8d95"/><path d="M64 58Q100 34 136 58Q100 46 64 58Z" fill="#a5a5ad"/><ellipse cx="92" cy="62" rx="22" ry="7" fill="#fff" opacity=".22"/><path d="M72 74Q100 68 128 74M70 80Q100 74 130 80" stroke="#b9805f" stroke-width="1.6" fill="none" opacity="${hot?.55:.25}"/>`+
 `${brows}${eye(80)}${eye(120)}<path d="M100 98Q95 118 89 128Q100 135 111 128Q105 118 100 98" stroke="#b27350" stroke-width="2" fill="none"/><ellipse cx="94" cy="130" rx="2.4" ry="1.6" fill="#7a4a35"/><ellipse cx="106" cy="130" rx="2.4" ry="1.6" fill="#7a4a35"/><path d="M82 124Q76 140 84 152M118 124Q124 140 116 152" stroke="#bf8561" stroke-width="2" fill="none"/>${mouth}<path d="M74 142Q88 132 100 139Q112 132 126 142Q112 152 100 147Q88 152 74 142Z" fill="#74747c"/><path d="M82 188L100 196L82 208ZM118 188L100 196L118 208Z" fill="url(#rd)"/><rect x="95" y="190" width="10" height="12" rx="3" fill="#b3122a"/>${sad?'<path d="M138 62Q143 72 138 78Q133 72 138 62Z" fill="#8fd3ff"/>':''}</svg>`;}
function bunnySVG(){return `<svg viewBox="0 0 200 300" preserveAspectRatio="xMidYMax meet" xmlns="http://www.w3.org/2000/svg"><defs><radialGradient id="bs" cx=".45" cy=".4" r=".75"><stop offset="0" stop-color="#fff0e4"/><stop offset=".7" stop-color="#ffd8c0"/><stop offset="1" stop-color="#eeb090"/></radialGradient><linearGradient id="bl" x1="0" x2="1"><stop offset="0" stop-color="#2a2a33"/><stop offset=".5" stop-color="#08080c"/><stop offset="1" stop-color="#25252e"/></linearGradient></defs>`+
 `<path d="M72 92Q44 14 70 8Q96 14 92 92Z" fill="#0c0c10"/><path d="M128 92Q156 14 130 8Q104 14 108 92Z" fill="#0c0c10"/><path d="M76 84Q58 24 70 16Q84 24 86 84Z" fill="#ff9ab8"/><path d="M124 84Q142 24 130 16Q116 24 114 84Z" fill="#ff9ab8"/>`+
 `<path d="M44 130Q36 80 100 70Q164 80 156 130Q166 200 150 252L50 252Q34 200 44 130Z" fill="#7b3a26"/><path d="M40 300Q44 214 100 204Q156 214 160 300Z" fill="url(#bs)"/><path d="M86 150L86 206Q100 216 114 206L114 150Z" fill="#f2c4a6"/><path d="M44 300L46 252Q70 232 100 252Q130 232 154 252L156 300Z" fill="url(#bl)"/><path d="M80 206Q100 222 120 206L124 214Q100 232 76 214Z" fill="#fff" stroke="#ddd"/><path d="M90 218L100 224L90 234ZM110 218L100 224L110 234Z" fill="#e0162f"/><circle cx="100" cy="225" r="3.5" fill="#a30f22"/>`+
 `<path d="M58 112Q58 66 100 64Q142 66 142 112Q142 150 120 168Q100 178 80 168Q58 150 58 112Z" fill="url(#bs)"/><path d="M54 116Q48 60 100 56Q152 60 146 116Q140 84 118 78Q108 94 92 84Q70 90 54 116Z" fill="#8a3b2a"/><path d="M54 116Q50 150 60 176Q58 140 66 112ZM146 116Q150 150 140 176Q142 140 134 112Z" fill="#8a3b2a"/><path d="M58 80Q100 56 142 80" stroke="#0c0c10" stroke-width="6" fill="none"/>`+
 `<path d="M70 100Q80 94 90 98M110 98Q120 94 130 100" stroke="#6b2e20" stroke-width="2.2" fill="none"/><ellipse cx="80" cy="116" rx="9.5" ry="10.5" fill="#fff"/><circle cx="80" cy="117" r="7" fill="#c0507a"/><circle cx="80" cy="117" r="3.2" fill="#2a0f18"/><circle cx="83" cy="113" r="2.2" fill="#fff"/><path d="M69 112Q80 103 91 110" stroke="#2a1510" stroke-width="3" fill="none"/><path d="M110 118Q120 108 130 118" stroke="#2a1510" stroke-width="3.5" fill="none" stroke-linecap="round"/><ellipse cx="70" cy="138" rx="9" ry="5" fill="#ff8aa8" opacity=".5"/><ellipse cx="130" cy="138" rx="9" ry="5" fill="#ff8aa8" opacity=".5"/><path d="M100 130Q98 136 102 136" stroke="#d49a80" stroke-width="1.6" fill="none"/><path d="M88 146Q100 162 112 146Q100 150 88 146Z" fill="#b0304a"/><ellipse cx="105" cy="155" rx="5" ry="3.6" fill="#ff8aa0"/><path d="M152 100L155 108L163 111L155 114L152 122L149 114L141 111L149 108Z" fill="#fff"/></svg>`;}
function showBunny(){const b=$("bunny");
 b.innerHTML=bunnySVG()+"<span>"+["残念〜♡","ざんね〜ん♡ もう一回！","次はきっと当たるよ♡","ドンマイ♡"][Math.random()*4|0]+"</span>";
 b.className="";void b.offsetWidth;b.className="go";
 [880,740,620].forEach((f,k)=>setTimeout(()=>beep(f,.25,.07,"triangle"),k*140));
 for(let k=0;k<50;k++)setTimeout(()=>addSpark(fx.width*(.2+Math.random()*.6),fx.height*.75,"#ff7ab8",7,70),k*30);
 setTimeout(()=>{b.className="";},2700);}
function say(t,m="n"){return;$("npc").innerHTML=`<div class="bub">${t}</div>`+charaSVG(m);}
/* ---- bet / bingo ---- */
const BETS=[["red","赤 ×2"],["black","黒 ×2"],["even","偶数 ×2"],["odd","奇数 ×2"],["green","0/00 ×18"]];
const LINES=[[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]],key=n=>n==="0"?-2:n==="00"?-1:+n;
let bal=1000,amt=50,bet={t:"red"},card=[],marks=new Set(),done=new Set(),hist=[];
try{bal=+localStorage.getItem("rl_bal")||1000;}catch(e){}
function updBal(){$("bal").textContent=bal;try{localStorage.setItem("rl_bal",bal);}catch(e){}}
function payout(n){const v=+n,nn=n!=="0"&&n!=="00";
 switch(bet.t){case"red":return nn&&REDS.has(v)?2:0;case"black":return nn&&!REDS.has(v)?2:0;case"even":return nn&&v%2===0?2:0;case"odd":return nn&&v%2===1?2:0;case"green":return nn?0:18;case"num":return n===bet.n?36:0;}return 0;}
function renderBets(){
 $("bets").innerHTML=BETS.map(b=>`<button data-t="${b[0]}" aria-pressed="${bet.t===b[0]}">${b[1]}</button>`).join("")+`<select id="numsel" aria-label="数字に賭ける ×36"><option value="">数字 ×36 ▾</option>${ORDER.slice().sort((a,b)=>key(a)-key(b)).map(n=>`<option value="${n}"${bet.t==="num"&&bet.n===n?" selected":""}>${n}</option>`).join("")}</select>`;
 $("amts").innerHTML=[10,50,100,500].map(a=>`<button data-a="${a}" aria-pressed="${amt===a}">${a}</button>`).join("");}
function newCard(){card=Array.from({length:36},(_,i)=>String(i+1)).sort(()=>Math.random()-.5).slice(0,9);marks=new Set();done=new Set();renderCard();}
function renderCard(){const need=new Set();
 LINES.forEach(l=>{if(l.filter(i=>marks.has(i)).length===2)l.forEach(i=>{if(!marks.has(i))need.add(i);});});
 $("card").innerHTML=card.map((n,i)=>`<b data-n="${n}" class="${marks.has(i)?"m":need.has(i)?"r":""}">${n}</b>`).join("");}
function afterResult(n,win){
 hist.unshift(n);hist=hist.slice(0,12);
 $("hist").innerHTML=hist.map(h=>`<i style="background:${{red:"#d4142f",black:"#222",green:"#16a34a"}[colorOf(h)]}">${h}</i>`).join("");
 const idx=card.indexOf(n);let bonus=0;if(idx>=0)marks.add(idx);
 LINES.forEach(l=>{const k=l.join();if(!done.has(k)&&l.every(i=>marks.has(i))){done.add(k);bonus+=300;}});
 const full=idx>=0&&marks.size===9;if(full)bonus+=2000;
 renderCard();updBal();
 say(win?"やったね！おめでとう！！":"あ〜ん、残念…",win?"hot":"sad");
 if(bonus)setTimeout(()=>{bal+=bonus;updBal();const b=$("banner2");b.textContent=full?"完全BINGO!!":"BINGO!!";b.className="";void b.offsetWidth;b.className="show";doFlash();
  for(let k=0;k<5;k++)setTimeout(()=>burst(fx.width*(.15+Math.random()*.7),fx.height*(.2+Math.random()*.4)),k*180);
  [659,784,988,1319].forEach((f,k)=>setTimeout(()=>beep(f,.25,.09,"triangle"),k*90));
  say(`ビンゴ！＋${bonus}チップ！`,"hot");msg.textContent+=` ／ ビンゴ +${bonus}`;
  setTimeout(()=>{b.className="";if(full)newCard();},3500);},win?3200:1200);}
document.addEventListener("click",e=>{if(st)return;const c=e.target.closest("#card b"),b=e.target.closest("[data-t],[data-a]");
 if(c){bet={t:"num",n:c.dataset.n};renderBets();}else if(b){if(b.dataset.t)bet={t:b.dataset.t};else amt=+b.dataset.a;renderBets();}});
document.addEventListener("change",e=>{if(e.target.id==="numsel"&&!st){bet=e.target.value?{t:"num",n:e.target.value}:{t:"red"};renderBets();}});
renderBets();newCard();updBal();say("いらっしゃい！賭けてSPINだよ！");
btn.addEventListener("click",spin);addEventListener("resize",resize);
resize();requestAnimationFrame(tick);requestAnimationFrame(loopFx);
</script>
</body>
</html>
