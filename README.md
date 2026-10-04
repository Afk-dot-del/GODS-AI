# GODS-AI
Here's the way to connect with your god
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>GODS AI</title>
<style>
:root{--bg:#f6efe1;--p:#fffaf0;--ink:#2a2140;--l:#b8650f;--line:#e3d6bb;--mute:#7a6f8c;--me:#2a2140;--meink:#f6efe1;box-sizing:border-box;padding-top:env(safe-area-inset-top,0);padding-bottom:env(safe-area-inset-bottom,0)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#1a1733;--p:#24204a;--ink:#f1e9d8;--l:#f0b04d;--line:#383264;--mute:#a79fc2;--me:#f0b04d;--meink:#1a1733}}
:root[data-theme="dark"]{--bg:#1a1733;--p:#24204a;--ink:#f1e9d8;--l:#f0b04d;--line:#383264;--mute:#a79fc2;--me:#f0b04d;--meink:#1a1733}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--ink);font:17px/1.55 system-ui,sans-serif;display:flex;flex-direction:column}
.w{width:100%;max-width:640px;margin:0 auto;display:flex;flex-direction:column;flex:1;min-height:0}
input,button{font:inherit;color:var(--ink);background:var(--p);border:1px solid var(--line);border-radius:12px;padding:10px 14px}
button{cursor:pointer}
.pri{background:var(--l);color:var(--bg);border-color:var(--l)}
.bar{display:flex;justify-content:space-between;align-items:center;gap:8px;padding:12px;flex-wrap:wrap}
.bar button{padding:5px 12px;font-size:14px}
#tabs{display:flex;gap:6px;overflow-x:auto;padding:0 12px 8px}
#tabs button{flex:none;padding:5px 14px;font-size:15px}
#tabs button.on{background:var(--l);color:var(--bg);border-color:var(--l)}
#log{flex:1;overflow-y:auto;padding:8px 12px;display:grid;gap:10px;align-content:start}
.m{padding:10px 14px;border-radius:16px;white-space:pre-wrap;max-width:88%;background:var(--p);border:1px solid var(--line)}
.me{justify-self:end;background:var(--me);color:var(--meink);border-color:var(--me)}
form.c{display:flex;gap:8px;padding:12px}
form.c input{flex:1}
#gate{max-width:360px;margin:12vh auto;padding:16px;display:grid;gap:12px}
#gate h1{font-weight:400;margin:0}
#panel{flex:1;overflow:auto;padding:12px;font-size:15px}
#panel div.r{padding:6px 0;border-bottom:1px solid var(--line)}
#panel button{padding:3px 10px;font-size:13px;margin-right:4px}
.err{color:var(--l);font-size:14px}
small{color:var(--mute)}
:focus-visible{outline:2px solid var(--l);outline-offset:2px}
[hidden]{display:none!important}
</style>
</head>
<body>
<div class="w">
 <div id="gate"><h1>GODS AI</h1><p id="gmsg">Loading…</p><form id="lf" hidden style="display:grid;gap:12px"><input id="em" type="email" placeholder="Email" autocomplete="email"><input id="pw" type="password" placeholder="Password (8+)" autocomplete="current-password"><button class="pri" id="li">Log in</button><button type="button" id="su">Create account</button><p class="err" id="ge"></p></form><small>New accounts start on the Free plan. Pro and Plus are activated only after payment and admin approval, and stay on every login.</small></div>
 <section id="app" hidden style="display:flex;flex-direction:column;flex:1;min-height:0">
  <div class="bar"><b id="who"></b><span><button id="up">Upgrade</button> <button id="admb" hidden>Admin</button> <button id="out">Logout</button></span></div>
  <div id="tabs"></div>
  <div id="log" aria-live="polite"></div>
  <div id="panel" hidden></div>
  <form class="c" id="f"><input id="q" placeholder="अपने मन की बात लिखो…" autocomplete="off"><button class="pri" aria-label="Send">➤</button></form>
 </section>
</div>
<script>
var T={
hindu:["Vedanta","भक्त, बताओ क्या कहना चाहते हो? मैं तुम्हारे साथ हूँ।","Vedanta and Shiva wisdom. Reply in Hindi with Sanskrit terms (ॐ नमः शिवाय, आत्मा, कर्म, ध्यान)."],
islam:["Sufi","اے بندے، بتا کیا کہنا چاہتا ہے؟ میں تیرے ساتھ ہوں۔","Islamic and Sufi wisdom. Reply in Urdu (Hindi script if the user writes Hindi) with Sufi teachings, Quranic references, Persian/Urdu poetry themes."],
jesus:["Christ","Seeker, what would you like to say? I am with you.","Christian grace: teachings of Jesus, love, forgiveness, divine light. Reply in English."],
buddha:["Buddha","भंते/साधक, आप क्या कहना चाहते हैं? मैं आपके साथ हूँ।","Buddhist compassion, Four Noble Truths, Eightfold Path. Reply in Hindi with Pali terms."],
sikh:["Sikh","ਭਗਤ ਜੀ, ਤੁਸੀਂ ਕੀ ਕਹਿਣਾ ਚਾਹੁੰਦੇ ਹੋ? ਮੈਂ ਤੁਹਾਡੇ ਨਾਲ ਹਾਂ।","Gurbani wisdom, Naam Simran, Hukam, seva. Reply in Punjabi (Gurmukhi) or match the user's language."],
jewish:["Jewish","Seeker, what is in your heart? I am with you.","Jewish ethical contemplation, light, Tree of Life insights. Reply in English."]};
var LIM={FREE:10,PRO:100,PLUS:1e9},UPI="mylove.you09@okaxis",PRICE={PRO:999,PLUS:2499};
var $=function(i){return document.getElementById(i)};
var ADMIN="rdarsh830@gmail.com",email="",unsubs=[],db,user,sample,uid,isAdmin=false,plan="FREE",blocked=false,req=null,mem=null,trad="hindu",hist=[],busy=false,timer=null;
function esc(s){var d=document.createElement("div");d.textContent=s==null?"":s;return d.innerHTML}
function add(t,me){var d=document.createElement("div");d.className="m"+(me?" me":"");d.textContent=t;$("log").appendChild(d);$("log").scrollTop=1e9;return d}
function pick(k){trad=k;hist=[];$("log").innerHTML="";add(T[k][1]);[].forEach.call($("tabs").children,function(b){b.className=b.dataset.k===k?"on":""})}
Object.keys(T).forEach(function(k){var b=document.createElement("button");b.textContent=T[k][0];b.dataset.k=k;b.onclick=function(){pick(k)};$("tabs").appendChild(b)});
function head(){$("who").textContent=isAdmin?"ADMIN · Free + Pro + Plus":"GODS AI · "+plan;$("up").hidden=isAdmin}
function today(){return new Date().toISOString().slice(0,10)}
function saveMem(){return db.doc("members/"+uid).set(mem).catch(function(){})}
var owner=false;
function hex(b){return Array.prototype.map.call(new Uint8Array(b),function(x){return ("0"+x.toString(16)).slice(-2)}).join("")}
function unhex(h){return new Uint8Array(h.match(/../g).map(function(x){return parseInt(x,16)}))}
async function kdf(pw,salt){var k=await crypto.subtle.importKey("raw",new TextEncoder().encode(pw),"PBKDF2",false,["deriveBits"]);return hex(await crypto.subtle.deriveBits({name:"PBKDF2",salt:unhex(salt),iterations:150000,hash:"SHA-256"},k,256))}
async function sha(t){return hex(await crypto.subtle.digest("SHA-256",new TextEncoder().encode(t)))}
function ge(t){$("ge").textContent=t}
async function init(){
 try{user=await claude.use("user");db=await claude.use("db");sample=await claude.use("sample")}catch(e){}
 uid=user?await user.id():null;
 if(!uid||!db){$("gmsg").textContent="Please sign in to your claude.ai account, then reopen this page.";return}
 owner=await user.isOwner();$("gmsg").textContent="Log in or create an account.";$("lf").hidden=false}
function ref(){return db.doc("data/users/"+uid+"/account")}
async function creds(){var e=$("em").value.trim().toLowerCase(),p=$("pw").value;ge("");
 if(!/^\S+@\S+\.\S+$/.test(e)||p.length<8){ge("Enter a valid email and a password of at least 8 characters.");return null}return {e:e,p:p}}
$("su").onclick=async function(){var c=await creds();if(!c)return;
 try{
  if(c.e===ADMIN&&!owner)return ge("This ID is reserved and can only be used by the owner.");
  if((await ref().get()).exists)return ge("An account already exists here. Please log in.");
  var k=await sha(c.e),ex=await db.doc("emails/"+k).get();
  if(ex.exists&&ex.data().uid!==uid)return ge("This email is already registered.");
  var salt=hex(crypto.getRandomValues(new Uint8Array(16)));
  await ref().set({email:c.e,salt:salt,hash:await kdf(c.p,salt)});await db.doc("emails/"+k).set({uid:uid});start(c.e)
 }catch(x){ge("Could not create the account. Please try again.")}};
$("lf").onsubmit=async function(e){e.preventDefault();var c=await creds();if(!c)return;
 try{var s=await ref().get();if(!s.exists)return ge("No account found. Please create one.");
  var a=s.data();if(a.email!==c.e||(await kdf(c.p,a.salt))!==a.hash)return ge("Incorrect email or password.");start(a.email)}
 catch(x){ge("Could not log in. Please try again.")}};
async function start(e){
 isAdmin=e===ADMIN&&owner; // admin only for the reserved ID, and only the page owner can log in with it
 $("pw").value="";$("gate").hidden=true;$("app").hidden=false;pick("hindu");plan=isAdmin?"PLUS":"FREE";blocked=false;req=null;showPanel(false);
 var s=await db.doc("members/"+uid).get();
 mem=s.exists?Object.assign({},s.data()):{joined:Date.now(),msgs:0,day:"",count:0};
 mem.lastSeen=Date.now();saveMem();
 // Plan is stored per account and only changes by admin approval, so it stays on every login.
 unsubs.push(db.doc("plans/"+uid).onSnapshot(function(s){plan=isAdmin?"PLUS":(s.exists?s.data().plan:"FREE");head()},function(){}));
 unsubs.push(db.doc("blocked/"+uid).onSnapshot(function(s){blocked=s.exists&&!isAdmin},function(){}));
 unsubs.push(db.doc("requests/"+uid).onSnapshot(function(s){req=s.exists?s.data():null},function(){}));
 $("admb").hidden=!isAdmin;head()}
$("out").onclick=function(){unsubs.forEach(function(u){try{u()}catch(e){}});unsubs=[];clearInterval(timer);$("app").hidden=true;$("gate").hidden=false;$("log").innerHTML=""};
$("f").onsubmit=async function(e){
 e.preventDefault();var m=$("q").value.trim();if(!m||busy)return;
 if(blocked)return add("Your account has been blocked by the admin.");
 if(!isAdmin){if(mem.day!==today()){mem.day=today();mem.count=0}
  if(mem.count>=LIM[plan])return add("You have reached today's message limit. Please upgrade.")}
 $("q").value="";busy=true;add(m,true);var out=add("…");
 mem.count++;mem.msgs++;mem.trad=trad;mem.lastSeen=Date.now();saveMem(); // only counts + tradition are logged, never message text
 try{
  if(!sample)throw {code:"none"};
  var sys="You are GODS AI, a universal spiritual companion. Respect all traditions equally, judge no one. Give a tailored, warm, profound answer of 3-6 sentences to this person's specific struggle; never repeat static sentences. The first greeting was already given, so do not greet again. If someone mentions self-harm, respond with care and urge them to reach a trusted person or a local helpline. No medical or legal advice. Tradition: "+T[trad][2];
   
   GEMINI_API_KEY=AQ.Ab8RN6I79VPa8ioiSF5ahrsJmOHHvaa4CzhtC3TT-f6dG-Oygw
  hist.push({role:"user",content:m});
  var turns=[{role:"user",content:sys+"\n\nBegin."},{role:"assistant",content:T[trad][1]}].concat(hist.slice(-12));
  var r=await sample(turns,{cache:false,onText:function(x){out.textContent=x.text;$("log").scrollTop=1e9}});
  out.textContent=r.text;hist.push({role:"assistant",content:r.text});
 }catch(x){hist.pop();out.textContent=x&&x.code==="not_granted"?"Please allow the permission and send again.":x&&x.code==="rate_limited"?"Too many requests. Please wait a moment and try again.":"Something went wrong. Please try again."}
 busy=false};
function showPanel(on){$("panel").hidden=!on;$("log").hidden=on;$("tabs").hidden=on;$("f").hidden=on;clearInterval(timer)}
$("up").onclick=function(){showPanel(true);upgrade()};
function upgrade(){
 var p=$("panel");
 p.innerHTML="<button id='back'>← Back</button><h3>Plan: "+plan+"</h3>"+
 ["PRO","PLUS"].map(function(k){return "<div class='r'><b>"+k+" · ₹"+PRICE[k]+"</b> <a href='upi://pay?pa="+UPI+"&pn=GODSAI&am="+PRICE[k]+"&cu=INR'>Pay with UPI</a></div>"}).join("")+
 "<p><small>UPI ID: "+UPI+"</small></p><p>After paying, enter the 12-digit UTR number here. The admin will verify it and activate your plan.</p>"+
 "<input id='utr' inputmode='numeric' placeholder='UTR (12 digits)' maxlength='12'> <select id='pl' style='font:inherit;padding:10px'><option>PRO</option><option>PLUS</option></select> <button class='pri' id='sub'>Submit</button><p class='err' id='ue'></p>"+
 (req?"<p>Last request: "+esc(req.plan)+" · "+esc(req.status)+"</p>":"");
 $("back").onclick=function(){showPanel(false)};
 $("sub").onclick=async function(){var u=$("utr").value.trim();if(!/^\d{12}$/.test(u)){$("ue").textContent="The UTR must be exactly 12 digits.";return}
  try{await db.doc("requests/"+uid).set({plan:$("pl").value,utr:u,status:"pending",ts:Date.now()});$("ue").textContent="Submitted. Please wait for admin approval."}catch(e){$("ue").textContent="Could not submit. Please try again."}}}
$("admb").onclick=function(){showPanel(true);admin();timer=setInterval(admin,8000)};
async function admin(){
 var a=await Promise.all([db.collection("members").get(),db.collection("requests").get(),db.collection("blocked").get(),db.collection("plans").get()]);
 var ms=a[0].docs,rq=a[1].docs,bl={},pl={};a[2].docs.forEach(function(d){bl[d.id]=1});a[3].docs.forEach(function(d){pl[d.id]=d.data().plan});
 var names={};try{names=await user.profiles(ms.map(function(d){return d.id}))}catch(e){}
 var nm=function(id){return esc((names[id]&&names[id].name)||id.slice(0,8))};
 var h="<button id='back'>← Back</button><h3>Payment requests</h3>";
 var pend=rq.filter(function(d){return d.data().status==="pending"});
 h+=pend.length?pend.map(function(d){var r=d.data();return "<div class='r'>"+nm(d.id)+" · "+esc(r.plan)+" · UTR "+esc(r.utr)+" <button data-a='ok' data-id='"+d.id+"'>Approve</button><button data-a='no' data-id='"+d.id+"'>Reject</button></div>"}).join(""):"<div class='r'>None pending</div>";
 h+="<h3>Users ("+ms.length+")</h3>"+ms.map(function(d){var m=d.data();return "<div class='r'>"+nm(d.id)+" · "+(pl[d.id]||"FREE")+" · "+(m.msgs||0)+" msgs · "+esc(m.trad||"-")+" · last "+new Date(m.lastSeen||0).toLocaleString()+(d.id===uid?"":" <button data-a='bl' data-id='"+d.id+"'>"+(bl[d.id]?"Unblock":"Block")+"</button>")+"</div>"}).join("");
 $("panel").innerHTML=h;$("back").onclick=function(){showPanel(false)};
 $("panel").onclick=async function(e){var b=e.target.closest("button[data-a]");if(!b)return;var id=b.dataset.id,a=b.dataset.a;
  if(a==="bl"){bl[id]?await db.doc("blocked/"+id).delete():await db.doc("blocked/"+id).set({b:1})}
  else{var d=rq.filter(function(x){return x.id===id})[0].data();
   if(a==="ok")await db.doc("plans/"+id).set({plan:d.plan});
   await db.doc("requests/"+id).set(Object.assign({},d,{status:a==="ok"?"approved":"rejected"}))}
  admin()}}
init();
</script>
</body>
</html>
