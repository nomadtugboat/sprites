Render the following interactive briefing as a series of HTML artifacts. This is an onboarding cutscene for Cogs vs Clips by Softmax.

**YOUR JOB:** Render Beat 1 RIGHT NOW as an HTML artifact. When the user says "Continue", render the next beat. When they say "Back", render the previous beat. Never summarize or describe — just render the HTML.

**COMMANDER NAME:** Beat 1 captures the user's callsign. After they click DECRYPT, they'll send "Continue". Remember whatever name they typed — insert `window.commanderName = "THEIR_NAME";` as the first line inside `<script>` in every beat after Beat 1.

**SPRITE HOSTING:** Before using this file, replace all instances of `{{SPRITE_BASE}}` with the actual base URL where the sprite PNGs are hosted (e.g. `https://raw.githubusercontent.com/yourorg/yourrepo/main/sprites`).

Required sprite files:
- `miner_large.png`, `aligner_large.png`, `scout_large.png`, `scrambler_large.png` (90px roster sprites)
- `miner_small.png`, `aligner_small.png`, `scout_small.png`, `scrambler_small.png` (112px squad lineup sprites)  
- `cog_hero.png` (hero sprite for final beat)

There are 6 beats total. Here they are:

<beat number="1" title="Comm Check">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 0 - Option 4: Compact Terminal</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:600px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="replay()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8634; REPLAY</button>
</div>
<div id="term" style="padding:32px 36px 16px;font-size:15px;color:#22c55e;line-height:1.9"></div>
<div id="input-row" style="opacity:0;margin:0 36px 32px;display:flex;gap:12px">
<input id="nameIn" type="text" placeholder="enter your callsign" style="flex:1;background:rgba(34,197,94,.04);border:1px solid rgba(34,197,94,.2);border-radius:5px;padding:14px 18px;font-family:inherit;font-size:15px;color:#ffffff;outline:none" onfocus="this.style.borderColor='rgba(34,197,94,.5)'" onblur="this.style.borderColor='rgba(34,197,94,.2)'"/>
<button onclick="handleDecrypt()" style="background:#22c55e;border:none;border-radius:5px;padding:14px 28px;font-family:inherit;font-size:14px;color:#000;font-weight:bold;letter-spacing:2px;cursor:pointer" onmouseover="this.style.background='#16a34a'" onmouseout="this.style.background='#22c55e'">DECRYPT</button>
</div>
</div>
<script>
var T=[],buf='',term=document.getElementById('term');
function clr(){T.forEach(clearTimeout);T=[];}
function cursor(){return '<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
function colorize(r){return r.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(r){return r.replace(/<\/?Y>/g,'');}
function buildPartial(raw,plain,count){var result='',pi=0,ri=0;while(pi<count&&ri<raw.length){if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}result+=raw[ri];pi++;ri++;}var opens=(result.match(/<span/g)||[]).length;var closes=(result.match(/<\/span>/g)||[]).length;if(opens>closes)result+='</span>';return result;}

function typeLine(ln,cb){
if(ln.txt===''){buf+='<br>';term.innerHTML=buf+cursor();T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';var display=colorize(ln.txt);var plain=stripTags(ln.txt);var i=0;
function tk(){if(i>=plain.length){buf+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';term.innerHTML=buf+cursor();T.push(setTimeout(cb,ln.pause));return;}i++;var p2=buildPartial(ln.txt,plain,i);term.innerHTML=buf+pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+p2+'</span>'+cursor();T.push(setTimeout(tk,28));}
tk();
}

var lines=[
{pre:'>> ',txt:'ESTABLISHING SECURE CONNECTION...',color:'#22c55e',pause:2500},
{pre:'>> ',txt:'CONNECTION ESTABLISHED.',color:'#22c55e',pause:1000},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'SOFTMAX // MISSION COMMAND',color:'#22c55e',pause:300},
{pre:'>> ',txt:'<Y>[PRIORITY: CRITICAL]</Y>',color:'#fbbf24',pause:400},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'To: Unknown Agent',color:'#22c55e',pause:200},
{pre:'>> ',txt:'From: Territorial Defense Initiative',color:'#22c55e',pause:200},
{pre:'>> ',txt:'Re: Sector breach in progress',color:'#22c55e',pause:1000},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'Encrypted transmission intercepted.',color:'#22c55e',pause:200},
{pre:'>> ',txt:'Identity verification required to decrypt.',color:'#22c55e',pause:1000},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'<Y>Agent, identify yourself</Y>',color:'#fbbf24',pause:0}
];

function run(){
buf='';term.innerHTML='';document.getElementById('input-row').style.opacity='0';document.getElementById('nameIn').value='';
var i=0;
function next(){if(i>=lines.length){T.push(setTimeout(function(){var ir=document.getElementById('input-row');ir.style.opacity='1';ir.style.animation='slideUp .4s ease-out';T.push(setTimeout(function(){document.getElementById('nameIn').focus();},200));},400));return;}
typeLine(lines[i],function(){i++;next();});}
T.push(setTimeout(next,400));
}
function replay(){clr();setTimeout(run,150);}
function handleDecrypt(){var name=document.getElementById('nameIn').value.trim();if(!name){document.getElementById('nameIn').focus();return;}if(typeof sendPrompt==='function'){window.commanderName=name;sendPrompt('Continue');}else{alert('Name captured: '+name);}}
document.getElementById('nameIn').addEventListener('keydown',function(e){if(e.key==='Enter')handleDecrypt();});
run();
</script>
</body></html>

</beat>

<beat number="2" title="The Transmission">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 1 - The Transmission</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:600px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="goBack()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8592; BACK</button>
</div>
<div id="term" style="padding:32px 36px 32px;font-size:15px;color:#22c55e;line-height:1.9"></div>
</div>
<script>
var T=[],buf='',term=document.getElementById('term'),done=false;
var cmdName=(window.commanderName||'').toUpperCase()||'COMMANDER';
function clr(){T.forEach(clearTimeout);T=[];}
function cursor(){return '<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
function colorize(r){return r.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(r){return r.replace(/<\/?Y>/g,'');}
function buildPartial(raw,plain,count){var result='',pi=0,ri=0;while(pi<count&&ri<raw.length){if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}result+=raw[ri];pi++;ri++;}var opens=(result.match(/<span/g)||[]).length;var closes=(result.match(/<\/span>/g)||[]).length;if(opens>closes)result+='</span>';return result;}
function typeLine(ln,cb){
if(ln.txt===''){buf+='<br>';term.innerHTML=buf+cursor();T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';var display=colorize(ln.txt);var plain=stripTags(ln.txt);var i=0;
function tk(){if(i>=plain.length){buf+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';term.innerHTML=buf+cursor();T.push(setTimeout(cb,ln.pause));return;}i++;var p2=buildPartial(ln.txt,plain,i);term.innerHTML=buf+pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+p2+'</span>'+cursor();T.push(setTimeout(tk,28));}
tk();}
var lines=[
{pre:'>> ',txt:'LOADING TRANSMISSION...',color:'#22c55e',pause:500},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'Transmission decrypted.',color:'#22c55e',pause:300},
{pre:'>> ',txt:cmdName+', listen carefully.',color:'#22c55e',pause:600},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'The <Y>Clips</Y> are expanding.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Junction by junction. Sector by sector.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Our perimeter is failing.',color:'#22c55e',pause:600},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:"We've lost 40% of contested territory in 72 hours.",color:'#22c55e',pause:300},
{pre:'>> ',txt:"We're running out of options.",color:'#22c55e',pause:300},
{pre:'>> ',txt:'We need a new plan.',color:'#22c55e',pause:600},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:"<Y>That's where you come in.</Y>",color:'#fbbf24',pause:0}
];
function run(){
buf='';done=false;term.innerHTML='';
var i=0;
function next(){if(i>=lines.length){T.push(setTimeout(function(){buf+='<br><span style="color:rgba(34,197,94,.4)">Press Enter to continue</span> '+cursor();term.innerHTML=buf;done=true;},500));return;}
typeLine(lines[i],function(){i++;next();});}
T.push(setTimeout(next,400));}
document.addEventListener('keydown',function(e){if(e.key==='Enter'&&done){if(typeof sendPrompt==='function')sendPrompt('Continue');else alert('Beat 1 complete');}});
function goBack(){clr();if(typeof sendPrompt==='function')sendPrompt('Back');else alert('Go back');}
run();
</script>
</body></html>

</beat>

<beat number="3" title="The Arena">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 2 - The Arena - Machina_1</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:640px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="goBack()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px;display:flex;align-items:center;gap:5px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8592; BACK</button>
</div>
<div id="term" style="padding:32px 36px 16px;font-size:15px;color:#22c55e;line-height:1.9"></div>
<div id="mapWrap" style="opacity:0;margin:0 36px;border:1px solid rgba(34,197,94,.15);border-radius:6px;overflow:hidden;background:rgba(0,0,0,.3)">
<div style="display:flex;justify-content:space-between;padding:8px 12px;border-bottom:1px solid rgba(34,197,94,.08)">
<span style="font-size:10px;letter-spacing:3px;color:rgba(34,197,94,.4)">MACHINA_1 SURFACE // LIVE FEED</span>
<span style="font-size:10px;color:rgba(34,197,94,.3)"><span style="color:#22c55e">&#9608;</span> COGS &nbsp; <span style="color:#fbbf24">&#9608;</span> CLIPS</span>
</div>
<div id="grid" style="display:grid;grid-template-columns:repeat(28,1fr);grid-template-rows:repeat(12,1fr);gap:1px;padding:8px;min-height:180px;background:rgba(0,0,0,.2)"></div>
<div style="display:flex;justify-content:space-between;padding:8px 12px;border-top:1px solid rgba(34,197,94,.08)">
<span style="font-size:10px;color:#22c55e">COGS: 32% territory</span>
<span style="font-size:10px;color:#fbbf24">CLIPS: 68% territory</span>
</div>
</div>
<div id="term2" style="padding:16px 36px 32px;font-size:15px;color:#22c55e;line-height:1.9"></div>
</div>
<script>
var T=[],term=document.getElementById('term'),term2=document.getElementById('term2'),buf='',buf2='',done=false;
function clr(){T.forEach(clearTimeout);T=[];}

function buildGrid(){
var g=document.getElementById('grid');g.innerHTML='';
var W=28,H=12;
for(var y=0;y<H;y++){
for(var x=0;x<W;x++){
var nx=x/W,ny=y/H;
var boundary=0.35+Math.sin(ny*3.5)*0.06+Math.cos(ny*5+0.3)*0.04+Math.sin(y*1.1+x*0.3)*0.03;
var isGreen=nx<boundary||(ny<0.35&&nx<boundary+0.15+Math.sin(x*0.5)*0.05);
var brightness=0.28+Math.random()*0.22;
var c=isGreen?'rgba(34,197,94,'+brightness+')':'rgba(251,191,36,'+(brightness*0.85)+')';
var d=document.createElement('div');
d.style.cssText='background:'+c+';min-height:13px';
g.appendChild(d);
}
}
}

var lines1=[
{pre:'>> ',txt:'LOADING TACTICAL DISPLAY...',color:'#22c55e',pause:500},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'This is the asteroid Machina_1.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'The Cogs and the <Y>Clips</Y> are competing for territory.',color:'#22c55e',pause:400},
];
var lines2=[
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'Green territory is yours. <Y>Yellow is theirs.</Y>',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Capture junctions to expand. Lose them and you shrink.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Your team must <Y>work together</Y> to win.',color:'#22c55e',pause:0},
];

function colorize(raw){return raw.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(raw){return raw.replace(/<\/?Y>/g,'');}
function buildPartial(raw,plain,count){
var result='',pi=0,ri=0;
while(pi<count&&ri<raw.length){
if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}
result+=raw[ri];pi++;ri++;
}
var opens=(result.match(/<span/g)||[]).length;
var closes=(result.match(/<\/span>/g)||[]).length;
if(opens>closes) result+='</span>';
return result;
}

function typeLine(el,bufRef,ln,cb){
if(ln.txt===''){if(bufRef==='b1'){buf+='<br>';el.innerHTML=buf+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}else{buf2+='<br>';el.innerHTML=buf2+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';
var display=colorize(ln.txt);
var plain=stripTags(ln.txt);
var i=0;
function tk(){
if(i>=plain.length){
if(bufRef==='b1'){buf+=pre+'<span style="color:'+ln.color+'">'+display+'</span><br>';el.innerHTML=buf+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
else{buf2+=pre+'<span style="color:'+ln.color+'">'+display+'</span><br>';el.innerHTML=buf2+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
T.push(setTimeout(cb,ln.pause));return;
}
i++;
var p2=buildPartial(ln.txt,plain,i);
var b=bufRef==='b1'?buf:buf2;
el.innerHTML=b+pre+'<span style="color:'+ln.color+'">'+p2+'</span><span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';
T.push(setTimeout(tk,28));
}
tk();
}

function showMap(){
var mw=document.getElementById('mapWrap');
buildGrid();
mw.style.opacity='1';
mw.style.animation='fadeIn .6s ease-out';
}

function run(){
buf='';buf2='';done=false;
term.innerHTML='';term2.innerHTML='';
document.getElementById('mapWrap').style.opacity='0';
var i=0;
function phase1(){
if(i>=lines1.length){T.push(setTimeout(function(){term.innerHTML=buf;showMap();T.push(setTimeout(phase2,800));},400));return;}
typeLine(term,'b1',lines1[i],function(){i++;phase1();});
}
function phase2(){
var j=0;
function next(){
if(j>=lines2.length){
T.push(setTimeout(function(){
buf2+='<br><span style="color:rgba(34,197,94,.4)">Press Enter to continue</span> <span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';
term2.innerHTML=buf2;
done=true;
},500));
return;
}
typeLine(term2,'b2',lines2[j],function(){j++;next();});
}
next();
}
T.push(setTimeout(phase1,400));
}

document.addEventListener('keydown',function(e){
if(e.key==='Enter'&&done){alert('Beat 2 complete — next: Beat 3 (squad roster)');}
});
function replay(){clr();term.innerHTML='';term2.innerHTML='';buf='';buf2='';document.getElementById('mapWrap').style.opacity='0';done=false;setTimeout(run,150);}
function goBack(){clr();if(typeof sendPrompt==="function")sendPrompt("Back");else alert("Go back");}
run();
</script>
</body></html>

</beat>

<beat number="4" title="The Roster">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 3 - Squad Roster</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideIn{from{opacity:0;transform:translateX(-20px)}to{opacity:1;transform:translateX(0)}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:800px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="goBack()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px;display:flex;align-items:center;gap:5px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8592; BACK</button>
</div>
<div id="term" style="padding:32px 36px 16px;font-size:15px;color:#22c55e;line-height:1.9"></div>
<div id="rosterWrap" style="opacity:0;margin:0 36px">
<div id="r0" style="opacity:0;display:flex;align-items:flex-start;gap:20px;padding:16px 0;border-bottom:1px solid rgba(34,197,94,.06)">
<img src="{{SPRITE_BASE}}/miner_large.png" style="height:90px;flex-shrink:0;filter:drop-shadow(0 0 10px rgba(240,153,123,.15))" alt="Miner"/>
<div style="padding-top:8px">
<div id="r0name" style="opacity:0;font-size:16px;color:#f0997b;font-weight:bold;margin-bottom:6px">MINER</div>
<div id="r0desc" style="font-size:14px;color:#22c55e;line-height:1.6"></div>
</div>
</div>
<div id="r1" style="opacity:0;display:flex;align-items:flex-start;gap:20px;padding:16px 0;border-bottom:1px solid rgba(34,197,94,.06)">
<img src="{{SPRITE_BASE}}/aligner_large.png" style="height:90px;flex-shrink:0;filter:drop-shadow(0 0 10px rgba(226,75,74,.15))" alt="Aligner"/>
<div style="padding-top:8px">
<div id="r1name" style="opacity:0;font-size:16px;color:#e24b4a;font-weight:bold;margin-bottom:6px">ALIGNER</div>
<div id="r1desc" style="font-size:14px;color:#22c55e;line-height:1.6"></div>
</div>
</div>
<div id="r2" style="opacity:0;display:flex;align-items:flex-start;gap:20px;padding:16px 0;border-bottom:1px solid rgba(34,197,94,.06)">
<img src="{{SPRITE_BASE}}/scout_large.png" style="height:90px;flex-shrink:0;filter:drop-shadow(0 0 10px rgba(93,202,165,.15))" alt="Scout"/>
<div style="padding-top:8px">
<div id="r2name" style="opacity:0;font-size:16px;color:#5dcaa5;font-weight:bold;margin-bottom:6px">SCOUT</div>
<div id="r2desc" style="font-size:14px;color:#22c55e;line-height:1.6"></div>
</div>
</div>
<div id="r3" style="opacity:0;display:flex;align-items:flex-start;gap:20px;padding:16px 0">
<img src="{{SPRITE_BASE}}/scrambler_large.png" style="height:90px;flex-shrink:0;filter:drop-shadow(0 0 10px rgba(133,183,235,.15))" alt="Scrambler"/>
<div style="padding-top:8px">
<div id="r3name" style="opacity:0;font-size:16px;color:#85b7eb;font-weight:bold;margin-bottom:6px">SCRAMBLER</div>
<div id="r3desc" style="font-size:14px;color:#22c55e;line-height:1.6"></div>
</div>
</div>
</div>
<div id="term2" style="padding:16px 36px 32px;font-size:15px;color:#22c55e;line-height:1.9"></div>
</div>
<script>
var T=[],term=document.getElementById('term'),term2=document.getElementById('term2'),buf='',buf2='',done=false;
function clr(){T.forEach(clearTimeout);T=[];}
function colorize(raw){return raw.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(raw){return raw.replace(/<\/?Y>/g,'')}
function buildPartial(raw,plain,count){
var result='',pi=0,ri=0;
while(pi<count&&ri<raw.length){if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}result+=raw[ri];pi++;ri++;}
var opens=(result.match(/<span/g)||[]).length;var closes=(result.match(/<\/span>/g)||[]).length;if(opens>closes) result+='</span>';return result;
}
function typeLine(el,bufRef,ln,cb){
if(ln.txt===''){if(bufRef==='b1'){buf+='<br>';el.innerHTML=buf+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}else{buf2+='<br>';el.innerHTML=buf2+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';var display=colorize(ln.txt);var plain=stripTags(ln.txt);var i=0;
function tk(){if(i>=plain.length){if(bufRef==='b1'){buf+=pre+'<span style="color:'+ln.color+'">'+display+'</span><br>';el.innerHTML=buf+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}else{buf2+=pre+'<span style="color:'+ln.color+'">'+display+'</span><br>';el.innerHTML=buf2+'<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}T.push(setTimeout(cb,ln.pause));return;}
i++;var p2=buildPartial(ln.txt,plain,i);var b=bufRef==='b1'?buf:buf2;el.innerHTML=b+pre+'<span style="color:'+ln.color+'">'+p2+'</span><span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';T.push(setTimeout(tk,28));}
tk();}

function typeInEl(el,txt,spd,cb){
var i=0;
function tk(){
if(i>=txt.length){el.textContent=txt;if(cb)cb();return;}
i++;el.textContent=txt.slice(0,i);
T.push(setTimeout(tk,spd));
}
tk();
}

var lines1=[
{pre:'>> ',txt:'LOADING BRIEFING...',color:'#22c55e',pause:500},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'There are 4 types of Cogs.',color:'#22c55e',pause:400},
];
var lines2=[
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'Mine resources to <Y>make hearts.</Y>',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Spend hearts to <Y>capture junctions.</Y>',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Capture junctions to <Y>hold territory.</Y>',color:'#22c55e',pause:500},
{pre:'',txt:'',pause:200},
{pre:'>> ',txt:'The team that holds the <Y>most territory</Y> for the <Y>longest time</Y> wins.',color:'#22c55e',pause:0},
];

var roles=[
{id:'r0',nameId:'r0name',descId:'r0desc',desc:'Mines resources from the asteroid surface'},
{id:'r1',nameId:'r1name',descId:'r1desc',desc:'Uses resources to make hearts, and spends them to capture junctions'},
{id:'r2',nameId:'r2name',descId:'r2desc',desc:'Finds junctions to capture'},
{id:'r3',nameId:'r3name',descId:'r3desc',desc:'Attacks enemy junctions'},
];

function showRoster(){
var rw=document.getElementById('rosterWrap');rw.style.opacity='1';
var ri=0;
function nextRole(){
if(ri>=roles.length){T.push(setTimeout(phase2,400));return;}
var r=roles[ri];
var el=document.getElementById(r.id);
el.style.opacity='1';el.style.animation='slideIn .4s ease-out';
T.push(setTimeout(function(){
var nm=document.getElementById(r.nameId);
nm.style.opacity='1';nm.style.animation='fadeIn .3s ease-out';
T.push(setTimeout(function(){
typeInEl(document.getElementById(r.descId),r.desc,25,function(){
ri++;
T.push(setTimeout(nextRole,500));
});
},200));
},300));
}
T.push(setTimeout(nextRole,300));
}

function phase2(){
var j=0;
function next(){if(j>=lines2.length){T.push(setTimeout(function(){buf2+='<br><span style="color:rgba(34,197,94,.4)">Press Enter to continue</span> <span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';term2.innerHTML=buf2;done=true;},500));return;}
typeLine(term2,'b2',lines2[j],function(){j++;next();});}
next();
}
function run(){
buf='';buf2='';done=false;term.innerHTML='';term2.innerHTML='';
document.getElementById('rosterWrap').style.opacity='0';
roles.forEach(function(r){
document.getElementById(r.id).style.opacity='0';document.getElementById(r.id).style.animation='none';
document.getElementById(r.nameId).style.opacity='0';document.getElementById(r.nameId).style.animation='none';
document.getElementById(r.descId).textContent='';
});
var i=0;
function phase1(){if(i>=lines1.length){T.push(setTimeout(function(){term.innerHTML=buf;showRoster();},400));return;}
typeLine(term,'b1',lines1[i],function(){i++;phase1();});}
T.push(setTimeout(phase1,400));
}
document.addEventListener('keydown',function(e){if(e.key==='Enter'&&done){if(typeof sendPrompt==='function')sendPrompt('Continue');else alert('Beat complete');}});
function replay(){clr();term.innerHTML='';term2.innerHTML='';buf='';buf2='';document.getElementById('rosterWrap').style.opacity='0';roles.forEach(function(r){document.getElementById(r.id).style.opacity='0';document.getElementById(r.id).style.animation='none';document.getElementById(r.nameId).style.opacity='0';document.getElementById(r.nameId).style.animation='none';document.getElementById(r.descId).textContent='';});done=false;setTimeout(run,150);}
function goBack(){clr();if(typeof sendPrompt==="function")sendPrompt("Back");else alert("Go back");}
run();
</script>
</body></html>

</beat>

<beat number="5" title="Call to Action">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 4 - Call to Action (v3)</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
@keyframes glowBorder{0%,100%{border-color:rgba(34,197,94,.2)}50%{border-color:rgba(34,197,94,.45)}}
@keyframes floatUp{0%,100%{transform:translateY(0)}50%{transform:translateY(-4px)}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:700px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="goBack()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px;display:flex;align-items:center;gap:5px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8592; BACK</button>
</div>
<div id="term" style="padding:32px 36px 16px;font-size:15px;color:#22c55e;line-height:1.9"></div>

<!-- SQUAD LINEUP - the visual anchor -->
<div id="squad" style="opacity:0;margin:0 36px;padding:8px 0 24px;display:flex;justify-content:center;gap:52px;border-bottom:1px solid rgba(34,197,94,.08)">
<div class="roleCard" style="text-align:center;opacity:0;animation:slideUp .5s ease-out 0s both">
<img src="{{SPRITE_BASE}}/miner_small.png" style="height:112px;filter:drop-shadow(0 0 8px #f0997b40);animation:floatUp 3s ease-in-out 0s infinite" alt="MINER"/>
</div>
<div class="roleCard" style="text-align:center;opacity:0;animation:slideUp .5s ease-out .15s both">
<img src="{{SPRITE_BASE}}/aligner_small.png" style="height:112px;filter:drop-shadow(0 0 8px #e24b4a40);animation:floatUp 3s ease-in-out .15s infinite" alt="ALIGNER"/>
</div>
<div class="roleCard" style="text-align:center;opacity:0;animation:slideUp .5s ease-out .3s both">
<img src="{{SPRITE_BASE}}/scout_small.png" style="height:112px;filter:drop-shadow(0 0 8px #5dcaa540);animation:floatUp 3s ease-in-out .3s infinite" alt="SCOUT"/>
</div>
<div class="roleCard" style="text-align:center;opacity:0;animation:slideUp .5s ease-out .45s both">
<img src="{{SPRITE_BASE}}/scrambler_small.png" style="height:112px;filter:drop-shadow(0 0 8px #85b7eb40);animation:floatUp 3s ease-in-out .45s infinite" alt="SCRAMBLER"/>
</div>
</div>

<!-- MISSION PANEL -->
<div id="panelWrap" style="opacity:0;margin:16px 36px 0;border:1px solid rgba(34,197,94,.2);border-radius:6px;overflow:hidden;background:linear-gradient(180deg,rgba(34,197,94,.03) 0%,rgba(0,0,0,.4) 100%);animation:glowBorder 3s ease-in-out infinite paused">
<div style="padding:12px 16px;border-bottom:1px solid rgba(34,197,94,.1);background:rgba(34,197,94,.04)">
<span style="font-size:11px;letter-spacing:4px;color:rgba(34,197,94,.5);font-weight:bold">MISSION ORDERS</span>
</div>
<div style="padding:16px 20px">
<div id="m1" style="opacity:0;padding:8px 0;font-size:14px;color:#22c55e;line-height:1.7;display:flex;gap:12px"><span style="color:rgba(34,197,94,.35);flex-shrink:0">01</span><span id="m1t"></span></div>
<div id="m2" style="opacity:0;padding:8px 0;font-size:14px;color:#22c55e;line-height:1.7;display:flex;gap:12px;border-top:1px solid rgba(34,197,94,.05)"><span style="color:rgba(34,197,94,.35);flex-shrink:0">02</span><span id="m2t"></span></div>
<div id="m3" style="opacity:0;padding:8px 0;font-size:14px;color:#22c55e;line-height:1.7;display:flex;gap:12px;border-top:1px solid rgba(34,197,94,.05)"><span style="color:rgba(34,197,94,.35);flex-shrink:0">03</span><span id="m3t"></span></div>
</div>
</div>

<div id="term2" style="padding:16px 36px 32px;font-size:15px;color:#22c55e;line-height:1.9"></div>
</div>

<script>
var T=[],buf='',buf2='',done=false;
var term=document.getElementById('term'),term2=document.getElementById('term2');
var cmdName=(window.commanderName||'').toUpperCase()||null;
var hdr=cmdName?'COMMANDER '+cmdName+', HERE IS YOUR MISSION...':'COMMANDER, HERE IS YOUR MISSION...';

function clr(){T.forEach(clearTimeout);T=[];}
function cursor(){return '<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
function colorize(r){return r.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(r){return r.replace(/<\/?Y>/g,'');}
function buildPartial(raw,plain,count){var result='',pi=0,ri=0;while(pi<count&&ri<raw.length){if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}result+=raw[ri];pi++;ri++;}var opens=(result.match(/<span/g)||[]).length;var closes=(result.match(/<\/span>/g)||[]).length;if(opens>closes)result+='</span>';return result;}

function typeLine(el,bRef,ln,cb){
if(ln.txt===''){if(bRef==='b1'){buf+='<br>';el.innerHTML=buf+cursor();}else{buf2+='<br>';el.innerHTML=buf2+cursor();}T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';var display=colorize(ln.txt);var plain=stripTags(ln.txt);var i=0;
function tk(){if(i>=plain.length){if(bRef==='b1'){buf+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';el.innerHTML=buf+cursor();}else{buf2+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';el.innerHTML=buf2+cursor();}T.push(setTimeout(cb,ln.pause));return;}i++;var p2=buildPartial(ln.txt,plain,i);var b=bRef==='b1'?buf:buf2;el.innerHTML=b+pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+p2+'</span>'+cursor();T.push(setTimeout(tk,26));}
tk();
}

function typeStep(sId,tId,txt,cb){var se=document.getElementById(sId);var el=document.getElementById(tId);se.style.opacity='1';se.style.animation='fadeIn .35s ease-out';var i=0;
function tk(){if(i>=txt.length){el.innerHTML=txt;T.push(setTimeout(cb,350));return;}i++;el.innerHTML=txt.slice(0,i)+cursor();T.push(setTimeout(tk,25));}tk();}

var lines1=[{pre:'>> ',txt:hdr,color:'#22c55e',pause:600},{pre:'',txt:'',pause:200}];
var steps=[
{id:'m1',tid:'m1t',txt:'Choose a Cog to train.',pause:300},
{id:'m2',tid:'m2t',txt:'Design a strategy (a "Policy") that teaches your Cog to be a good teammate.',pause:300},
{id:'m3',tid:'m3t',txt:'Upload your Policy to compete in Alignment League.',pause:300}
];
var lines2=[
{pre:'',txt:'',pause:150},
{pre:'>> ',txt:'Use any AI (Codex, Claude, etc.) to help design your Policy.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'Use Softmax tools to train and improve.',color:'#22c55e',pause:300},
{pre:'>> ',txt:'<Y>The fate of our species depends on you!</Y>',color:'#fbbf24',pause:0}
];

function run(){
buf='';buf2='';done=false;term.innerHTML='';term2.innerHTML='';
document.getElementById('squad').style.opacity='0';
document.getElementById('panelWrap').style.opacity='0';
['m1','m2','m3'].forEach(function(id){document.getElementById(id).style.opacity='0';});
['m1t','m2t','m3t'].forEach(function(id){document.getElementById(id).innerHTML='';});
var i=0;
function p1(){if(i>=lines1.length){term.innerHTML=buf;T.push(setTimeout(function(){document.getElementById('squad').style.opacity='1';document.getElementById('squad').style.animation='fadeIn .5s ease-out';T.push(setTimeout(function(){document.getElementById('panelWrap').style.opacity='1';document.getElementById('panelWrap').style.animation='fadeIn .4s ease-out, glowBorder 3s ease-in-out .4s infinite';T.push(setTimeout(pSteps,400));},600));},300));return;}
typeLine(term,'b1',lines1[i],function(){i++;p1();});}
function pSteps(){var si=0;function nx(){if(si>=steps.length){T.push(setTimeout(p2,300));return;}var s=steps[si];typeStep(s.id,s.tid,s.txt,function(){si++;nx();});}nx();}
function p2(){var j=0;function nx(){if(j>=lines2.length){T.push(setTimeout(function(){buf2+='<br><span style="color:rgba(34,197,94,.4)">Press Enter to begin loadout</span> '+cursor();term2.innerHTML=buf2;done=true;},400));return;}typeLine(term2,'b2',lines2[j],function(){j++;nx();});}nx();}
T.push(setTimeout(p1,400));
}
document.addEventListener('keydown',function(e){if(e.key==='Enter'&&done){if(typeof sendPrompt==='function')sendPrompt('Continue');else alert('Beat 4 complete — next: Beat 5 (the handoff)');}});
function replay(){clr();run();}
function goBack(){clr();if(typeof sendPrompt==="function")sendPrompt("Back");else alert("Go back");}
run();
</script>
</body></html>

</beat>

<beat number="6" title="The Handoff">
<!DOCTYPE html>
<html><head><meta charset="utf-8"><title>Beat 5 - The Handoff</title></head><body style="margin:0;padding:20px;background:#000">
<style>
@keyframes blink{50%{opacity:0}}
@keyframes scanDrift{0%{top:-6px}100%{top:100%}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
@keyframes glowBorder{0%,100%{box-shadow:0 0 6px rgba(34,197,94,.1),inset 0 0 6px rgba(34,197,94,.02)}50%{box-shadow:0 0 20px rgba(34,197,94,.25),inset 0 0 14px rgba(34,197,94,.06)}}
@keyframes floatUp{0%,100%{transform:translateY(0)}50%{transform:translateY(-5px)}}
@keyframes copyFlash{0%{background:rgba(34,197,94,.15)}100%{background:rgba(34,197,94,.06)}}
</style>
<div style="background:#0b1a0b;border-radius:14px;overflow:hidden;font-family:'IBM Plex Mono',Menlo,'Courier New',monospace;position:relative;min-height:640px;max-width:720px;margin:0 auto">
<div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 40%,rgba(34,197,94,.06) 0%,transparent 60%);pointer-events:none"></div>
<div style="position:absolute;left:0;right:0;height:4px;background:rgba(34,197,94,.15);box-shadow:0 0 12px rgba(34,197,94,.1);animation:scanDrift 2.5s linear infinite;pointer-events:none;z-index:20"></div>
<div style="display:flex;align-items:center;gap:8px;padding:11px 18px;background:rgba(34,197,94,.04);border-bottom:1px solid rgba(34,197,94,.1)">
<div style="width:12px;height:12px;border-radius:50%;background:#ef4444"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#eab308"></div>
<div style="width:12px;height:12px;border-radius:50%;background:#22c55e"></div>
<span style="margin-left:16px;font-size:13px;color:rgba(34,197,94,.55)">secure channel -- softmax research lab</span>
<button onclick="goBack()" style="margin-left:auto;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.2);border-radius:4px;padding:5px 12px;font-family:inherit;font-size:10px;color:rgba(34,197,94,.5);cursor:pointer;letter-spacing:1px;display:flex;align-items:center;gap:5px" onmouseover="this.style.background='rgba(34,197,94,.15)';this.style.color='#22c55e'" onmouseout="this.style.background='rgba(34,197,94,.08)';this.style.color='rgba(34,197,94,.5)'">&#8592; BACK</button>
</div>

<!-- Terminal output 1 -->
<div id="term" style="padding:32px 36px 8px;font-size:15px;color:#22c55e;line-height:1.9"></div>

<!-- HERO: Cog sprite + command box side by side -->
<div id="heroWrap" style="opacity:0;margin:0 36px;display:flex;align-items:center;gap:28px;padding:16px 0 20px">
<!-- Cog sprite -->
<div style="flex-shrink:0">
<img id="cogHero" src="{{SPRITE_BASE}}/cog_hero.png" style="height:180px;filter:drop-shadow(0 0 16px rgba(34,197,94,.3));animation:floatUp 3s ease-in-out infinite" alt="Cog"/>
</div>
<!-- Command box -->
<div style="flex:1;min-width:0">
<div id="cmdBox" style="position:relative;background:rgba(34,197,94,.06);border:1px solid rgba(34,197,94,.25);border-radius:6px;padding:16px 18px 40px;animation:glowBorder 3s ease-in-out infinite">
<div id="cmdText" style="font-size:14px;color:#ffffff;line-height:1.8;word-break:break-all;white-space:pre-wrap"></div>
<button id="copyBtn" onclick="copyCmd()" style="position:absolute;bottom:8px;right:8px;background:#22c55e;border:none;border-radius:4px;padding:6px 14px;font-family:inherit;font-size:11px;color:#000;font-weight:bold;letter-spacing:1px;cursor:pointer;opacity:0;transition:opacity .3s" onmouseover="this.style.background='#16a34a'" onmouseout="this.style.background='#22c55e'">COPY</button>
</div>
</div>
</div>

<!-- Terminal output 2 -->
<div id="term2" style="padding:8px 36px 32px;font-size:15px;color:#22c55e;line-height:1.9"></div>
</div>

<script>
var T=[],buf='',buf2='',done=false;
var term=document.getElementById('term'),term2=document.getElementById('term2');
var cmdName=(window.commanderName||'').toUpperCase()||null;
var nameStr=cmdName?' '+cmdName:'';

var CMD_TEXT='git clone https://github.com/Metta-AI/cogames.git\ncd cogames\nuv pip install cogames';

function clr(){T.forEach(clearTimeout);T=[];}
function cursor(){return '<span style="animation:blink .7s step-end infinite;color:#22c55e">&#9608;</span>';}
function colorize(r){return r.replace(/<Y>/g,'<span style="color:#fbbf24">').replace(/<\/Y>/g,'</span>');}
function stripTags(r){return r.replace(/<\/?Y>/g,'');}
function buildPartial(raw,plain,count){var result='',pi=0,ri=0;while(pi<count&&ri<raw.length){if(raw[ri]==='<'){var close=raw.indexOf('>',ri);result+=raw.slice(ri,close+1).replace(/<Y>/,'<span style="color:#fbbf24">').replace(/<\/Y>/,'</span>');ri=close+1;continue;}result+=raw[ri];pi++;ri++;}var opens=(result.match(/<span/g)||[]).length;var closes=(result.match(/<\/span>/g)||[]).length;if(opens>closes)result+='</span>';return result;}

function typeLine(el,bRef,ln,cb){
if(ln.txt===''){if(bRef==='b1'){buf+='<br>';el.innerHTML=buf+cursor();}else{buf2+='<br>';el.innerHTML=buf2+cursor();}T.push(setTimeout(cb,ln.pause));return;}
var pre='<span style="color:rgba(34,197,94,.4)">'+ln.pre+'</span>';var display=colorize(ln.txt);var plain=stripTags(ln.txt);var i=0;
function tk(){if(i>=plain.length){if(bRef==='b1'){buf+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';el.innerHTML=buf+cursor();}else{buf2+=pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+display+'</span><br>';el.innerHTML=buf2+cursor();}T.push(setTimeout(cb,ln.pause));return;}i++;var p2=buildPartial(ln.txt,plain,i);var b=bRef==='b1'?buf:buf2;el.innerHTML=b+pre+'<span style="color:'+(ln.color||'#22c55e')+'">'+p2+'</span>'+cursor();T.push(setTimeout(tk,26));}
tk();
}

function typeCmd(cb){
var el=document.getElementById('cmdText');
var chars=CMD_TEXT;var i=0;
function tk(){
if(i>=chars.length){el.textContent=CMD_TEXT;document.getElementById('copyBtn').style.opacity='1';T.push(setTimeout(cb,400));return;}
i++;el.textContent=chars.slice(0,i);el.innerHTML=el.textContent.replace(/\n/g,'<br>')+cursor();
T.push(setTimeout(tk,22));
}tk();
}

function copyCmd(){
var text=CMD_TEXT;
if(navigator.clipboard&&navigator.clipboard.writeText){
navigator.clipboard.writeText(text).then(function(){flashCopy();}).catch(function(){fallbackCopy(text);});
}else{fallbackCopy(text);}
}
function fallbackCopy(text){
var ta=document.createElement('textarea');ta.value=text;ta.style.cssText='position:fixed;left:-9999px';document.body.appendChild(ta);ta.select();
try{document.execCommand('copy');flashCopy();}catch(e){}document.body.removeChild(ta);
}
function flashCopy(){
var btn=document.getElementById('copyBtn');btn.textContent='COPIED';btn.style.background='#16a34a';
var box=document.getElementById('cmdBox');box.style.animation='copyFlash .4s ease-out';
setTimeout(function(){btn.textContent='COPY';btn.style.background='#22c55e';box.style.animation='glowBorder 3s ease-in-out infinite';},1500);
}

var lines1=[
{pre:'>> ',txt:"Now it's time to build...",color:'#22c55e',pause:500},
{pre:'',txt:'',pause:150},
{pre:'>> ',txt:'<Y>Copy this into Codex, Claude Code, Terminal, or any coding</Y>',color:'#fbbf24',pause:0},
{pre:'>> ',txt:'<Y>environment:</Y>',color:'#fbbf24',pause:300}
];
var lines2=[
{pre:'',txt:'',pause:150},
{pre:'>> ',txt:'<Y>Good luck Commander'+(cmdName?' '+cmdName:'')+'!</Y>',color:'#fbbf24',pause:400},
{pre:'>> ',txt:'Your agent will take it from here...',color:'#22c55e',pause:0}
];

function run(){
buf='';buf2='';done=false;term.innerHTML='';term2.innerHTML='';
document.getElementById('heroWrap').style.opacity='0';
document.getElementById('cmdText').textContent='';
document.getElementById('copyBtn').style.opacity='0';
var i=0;
function p1(){
if(i>=lines1.length){
term.innerHTML=buf;
T.push(setTimeout(function(){
document.getElementById('heroWrap').style.opacity='1';
document.getElementById('heroWrap').style.animation='fadeIn .5s ease-out';
T.push(setTimeout(function(){typeCmd(p2);},300));
},200));
return;
}
typeLine(term,'b1',lines1[i],function(){i++;p1();});
}
function p2(){
var j=0;
function nx(){
if(j>=lines2.length){
T.push(setTimeout(function(){
buf2+='<br><span style="color:rgba(34,197,94,.4)">End transmission</span> '+cursor();
term2.innerHTML=buf2;
done=true;
},500));
return;
}
typeLine(term2,'b2',lines2[j],function(){j++;nx();});
}nx();
}
T.push(setTimeout(p1,400));
}

document.addEventListener('keydown',function(e){if(e.key==='Enter'&&done){if(typeof sendPrompt==='function')sendPrompt('Continue');else alert('Beat 5 complete');}});
function replay(){clr();run();}
function goBack(){clr();if(typeof sendPrompt==="function")sendPrompt("Back");else alert("Go back");}
run();
</script>
</body></html>

</beat>

Now render Beat 1 as an HTML artifact. No preamble, no explanation — just the artifact.
