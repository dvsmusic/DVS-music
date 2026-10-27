# DVS-music

<!DOCTYPE html>
<html lang="ru"><head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#0b0b0b">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="DVS&music">
<title>DVS&amp;music</title>
<style>
:root{--g:#d4af37;--g2:#f3d77a;--l:#2a2a2a;--m:#8a8a8a}
*{box-sizing:border-box;margin:0;-webkit-tap-highlight-color:transparent}
body{background:#0b0b0b;color:#f2f2f2;font-family:-apple-system,system-ui,sans-serif}
header{position:fixed;top:0;left:0;right:0;height:calc(52px + env(safe-area-inset-top));padding:env(safe-area-inset-top) 16px 0;background:#0b0b0b;border-bottom:1px solid var(--l);display:flex;align-items:center;justify-content:space-between;z-index:5}
.logo{font-weight:800;font-size:20px;letter-spacing:1px;color:var(--g)}
.logo i{color:#bbb;font-style:normal;font-weight:400}
#v{position:fixed;top:calc(52px + env(safe-area-inset-top));bottom:calc(60px + env(safe-area-inset-bottom));left:0;right:0;overflow-y:auto}
#v.r{scroll-snap-type:y mandatory}
nav{position:fixed;bottom:0;left:0;right:0;height:calc(60px + env(safe-area-inset-bottom));padding-bottom:env(safe-area-inset-bottom);background:#0b0b0b;border-top:1px solid var(--l);display:flex;z-index:5}
nav button{flex:1;background:none;border:0;color:var(--m);font-size:12px}
nav button b{display:block;font-size:20px}
nav .a{color:var(--g)}
.ban{margin:12px;padding:12px;border:1px solid var(--g);border-radius:12px;background:#15130a}
.ban small{color:var(--m)}
.bar{height:6px;background:var(--l);border-radius:3px;margin-top:8px}
.bar div{height:100%;background:linear-gradient(90deg,var(--g),var(--g2));border-radius:3px}
.card{border-bottom:1px solid var(--l);padding-bottom:12px}
.who{display:flex;justify-content:space-between;padding:12px 14px}
.who span,.tt span{color:var(--m);font-size:13px}
.cov{aspect-ratio:1;position:relative}
.pl{position:absolute;inset:0;margin:auto;width:72px;height:72px;border-radius:50%;border:2px solid #fff8;background:#0008;color:var(--g2);font-size:28px}
.row{display:flex;gap:10px;align-items:center;padding:10px 14px}
.row button,.side button{background:#1a1a1a;border:1px solid var(--l);color:#ddd;border-radius:20px;padding:8px 14px;font-size:15px}
.row button.on,.side button.on{color:#000;background:var(--g);border-color:var(--g)}
.row span{color:var(--m);margin-left:auto}
.tt{padding:0 14px;font-weight:600}
.reel{height:100%;scroll-snap-align:start;position:relative;display:flex;align-items:flex-end;padding:18px}
.reel .info{flex:1;text-shadow:0 1px 6px #000}
.reel h2{font-size:22px}
.side{display:flex;flex-direction:column;gap:12px}
.rk{display:flex;align-items:center;gap:12px;padding:12px 14px;border-bottom:1px solid var(--l)}
.rk .n{font-size:22px;font-weight:800;color:var(--g);width:28px}
.rk .sc{margin-left:auto;color:var(--g2);font-weight:700}
.pad{padding:16px}
h3{color:var(--g);margin:0 0 8px}
.priv{border:1px dashed var(--g);border-radius:12px;padding:14px;margin-top:14px;color:#ccc}
#t{position:fixed;left:50%;bottom:90px;transform:translateX(-50%);background:var(--g);color:#000;padding:8px 16px;border-radius:20px;font-size:14px;opacity:0;transition:.3s;z-index:9}
</style></head><body>
<header><div class="logo">DVS<i>&amp;</i>music</div></header>
<div id="v"></div><div id="t"></div>
<nav id="nav"></nav>
<script>
const T=[
{id:1,t:'Золотой час',a:'Nova Keys',g:'Synthwave',c:['#d4af37','#2a1f00'],p:1240,l:310,v:42},
{id:2,t:'Ночной рейс',a:'DJ Marlo',g:'House',c:['#8a8a8a','#111'],p:980,l:220,v:31},
{id:3,t:'Тихий бит',a:'Ayla',g:'Lo-fi',c:['#f3d77a','#2a2a2a'],p:760,l:180,v:19},
{id:4,t:'Эхо города',a:'Rem & Co',g:'Hip-hop',c:['#b8962e','#000'],p:1500,l:400,v:55},
{id:5,t:'Волна',a:'Sunya',g:'Pop',c:['#cfcfcf','#262626'],p:640,l:150,v:12}];
let S;try{S=JSON.parse(localStorage.getItem('dvs1'))}catch(e){}
S=S||{start:Date.now(),like:{},vote:{},play:{}};
const save=()=>{try{localStorage.setItem('dvs1',JSON.stringify(S))}catch(e){}};
const L=t=>t.l+(S.like[t.id]?1:0),V=t=>t.v+(S.vote[t.id]?1:0),P=t=>t.p+(S.play[t.id]||0);
const sc=t=>V(t)*3+L(t)+Math.floor(P(t)/10);
const bg=t=>`linear-gradient(135deg,${t.c[0]},${t.c[1]})`;
let tab='feed';
const day=()=>Math.min(90,Math.floor((Date.now()-S.start)/864e5)+1);
const btns=t=>`<button class="${S.like[t.id]?'on':''}" onclick="like(${t.id})">♥ ${L(t)}</button><button class="${S.vote[t.id]?'on':''}" onclick="vote(${t.id})">★ ${V(t)}</button>`;
const views={
feed:()=>`<div class="ban"><b>Сезон 1</b> · день ${day()} из 90<div class="bar"><div style="width:${day()/.9}%"></div></div></div>`+
T.map(t=>`<div class="card"><div class="who"><b>${t.a}</b><span>${t.g}</span></div><div class="cov" style="background:${bg(t)}"><button class="pl" onclick="play(${t.id})">▶</button></div><div class="row">${btns(t)}<span>▶ ${P(t)}</span></div><p class="tt">${t.t}</p></div>`).join(''),
reels:()=>T.map(t=>`<div class="reel" style="background:${bg(t)}"><div class="info"><h2>${t.t}</h2><div>${t.a} · ${t.g}</div><div style="margin-top:6px">▶ ${P(t)}</div></div><div class="side">${btns(t)}<button onclick="play(${t.id})">▶</button></div></div>`).join(''),
top:()=>`<div class="pad"><h3>Рейтинг сезона</h3></div>`+[...T].sort((a,b)=>sc(b)-sc(a)).map((t,i)=>`<div class="rk"><div class="n">${i+1}</div><div><b>${t.t}</b><br><small style="color:#8a8a8a">${t.a}</small></div><div class="sc">${sc(t)}</div></div>`).join(''),
me:()=>`<div class="pad"><h3>Мой профиль</h3><p>Сезон 1: день ${day()} из 90</p><p style="margin-top:8px">Голосов осталось: ${3-Object.keys(S.vote).length} из 3</p><div class="priv"><b style="color:#f3d77a">Мой кабинет</b><br>Эти данные видите только вы. Сведения о вознаграждениях появятся здесь после запуска аккаунтов.</div></div>`};
const tabs=[['feed','⌂','Лента'],['reels','▶','Reels'],['top','★','Рейтинг'],['me','☺','Профиль']];
function draw(){const e=document.getElementById('v'),s=e.scrollTop;e.className=tab=='reels'?'r':'';e.innerHTML=views[tab]();e.scrollTop=s;
document.getElementById('nav').innerHTML=tabs.map(x=>`<button class="${x[0]==tab?'a':''}" onclick="go('${x[0]}')"><b>${x[1]}</b>${x[2]}</button>`).join('')}
function go(x){tab=x;document.getElementById('v').scrollTop=0;draw()}
function toast(m){const e=document.getElementById('t');e.textContent=m;e.style.opacity=1;setTimeout(()=>e.style.opacity=0,1600)}
function like(i){S.like[i]=!S.like[i];save();draw()}
function vote(i){if(S.vote[i])delete S.vote[i];else if(Object.keys(S.vote).length>=3)return toast('Максимум 3 голоса за сезон');else S.vote[i]=1;save();draw()}
function play(i){S.play[i]=(S.play[i]||0)+1;save();toast('Прослушивание засчитано');draw()}
draw();
</script></body></html>
