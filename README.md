[index.html](https://github.com/user-attachments/files/32451320/svidanie.html)
# date-invitation<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Прошение об аудиенции</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;0,9..144,600;1,9..144,500&family=Inter:wght@400;500&display=swap" rel="stylesheet">
<style>
:root{--bg:#faf9f6;--bg-card:#fff;--ink:#201e1b;--ink-soft:#6b6660;--line:#e3e0d9;--accent:#9c3b4a;--focus:#9c3b4a}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#1c1a18;--bg-card:#262320;--ink:#f2efe9;--ink-soft:#b6b0a7;--line:#3a3632;--accent:#e0919d;--focus:#e0919d}}
*{box-sizing:border-box}
html,body{margin:0;padding:0;min-height:100%;background:var(--bg)}
body{font-family:"Inter",-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;color:var(--ink);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px;overflow-x:hidden;position:relative}
.scene{position:relative;width:100%;max-width:460px}
.card{background:var(--bg-card);border:1px solid var(--line);border-radius:4px;padding:52px 40px;text-align:center;position:relative;z-index:1;display:none}
.card.show{display:block;animation:rise .45s ease both}
.kicker{font-size:13px;color:var(--ink-soft);margin:0 0 18px}
h1{font-family:"Fraunces",Georgia,"Times New Roman",serif;font-weight:500;font-size:clamp(27px,5.6vw,38px);line-height:1.22;margin:0 0 20px;letter-spacing:-.01em}
.sub{font-size:16px;line-height:1.65;color:var(--ink-soft);max-width:340px;margin:0 auto 38px}
.actions{display:flex;justify-content:center;gap:14px;position:relative;height:52px}
button{font-family:"Inter",sans-serif;font-size:15px;font-weight:500;border-radius:3px;padding:13px 26px;cursor:pointer;border:1px solid transparent;transition:transform .15s ease,background .15s ease;white-space:nowrap}
button:focus-visible{outline:2px solid var(--focus);outline-offset:3px}
#yes,#submit-time{background:var(--accent);color:#fff;border-color:var(--accent)}
#yes:hover,#submit-time:hover{transform:translateY(-1px)}
#no{background:transparent;color:var(--ink-soft);border-color:var(--line)}
#no.armed{position:fixed;transition:left .22s ease,top .22s ease}
.no-anchor{visibility:hidden;padding:13px 26px;font-size:15px;border:1px solid transparent}
.tally{margin-top:26px;font-size:13px;color:var(--ink-soft);min-height:16px}
.field-row{display:flex;gap:12px;margin-bottom:28px;text-align:left}
.field{flex:1;display:flex;flex-direction:column;gap:6px}
.field label{font-size:13px;color:var(--ink-soft)}
.field input{font-family:"Inter",sans-serif;font-size:15px;color:var(--ink);background:var(--bg);border:1px solid var(--line);border-radius:3px;padding:11px 12px;width:100%}
.field input:focus-visible{outline:2px solid var(--focus);outline-offset:2px}
.confirm-detail{font-family:"Fraunces",Georgia,serif;font-size:19px;color:var(--accent);margin:0 0 6px}
#notify-btn{background:transparent;color:var(--accent);border-color:var(--accent)}
#notify-btn.sent{color:var(--ink-soft);border-color:var(--line)}
@keyframes rise{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@media(prefers-reduced-motion:reduce){*{transition:none!important;animation:none!important}}
@media(max-width:420px){.card{padding:40px 22px}.field-row{flex-direction:column;gap:16px}}
</style>
</head>
<body>
<div class="scene">
<div class="card show" id="ask-card">
<p class="kicker">Прошение об аудиенции</p>
<h1>О, лучезарная, окажите скромному просителю великую милость!</h1>
<p class="sub">Не соизволите ли вы уделить капельку вашего драгоценного внимания и составить мне компанию на свидании?</p>
<div class="actions">
<button id="yes">Соизволяю</button>
<span class="no-anchor" aria-hidden="true">Увы, не в этот раз</span>
<button id="no">Увы, не в этот раз</button>
</div>
<p class="tally" id="tally"></p>
</div>

<div class="card" id="schedule-card">
<p class="kicker">Высочайшее согласие получено</p>
<h1>Назначьте день и час, когда мне выпадет честь вас видеть.</h1>
<p class="sub">Выбор всецело за вами — подстроюсь под любое время.</p>
<div class="field-row">
<div class="field"><label for="date-input">Дата</label><input type="date" id="date-input" required></div>
<div class="field"><label for="time-input">Время</label><input type="time" id="time-input" required></div>
</div>
<div class="actions" style="height:auto"><button id="submit-time">Назначить час</button></div>
</div>

<div class="card" id="result-card">
<p class="kicker">Аудиенция назначена</p>
<h1>Свершилось.</h1>
<p class="confirm-detail" id="confirm-detail"></p>
<p class="sub">Буду считать часы до встречи. Остальное — платье, настроение, маршрут — доверяю целиком вам.</p>
<div class="actions" style="height:auto"><button id="notify-btn">Уведомить его</button></div>
</div>
</div>

<script>
const yes=document.getElementById('yes'),no=document.getElementById('no'),tally=document.getElementById('tally');
const askCard=document.getElementById('ask-card'),scheduleCard=document.getElementById('schedule-card'),resultCard=document.getElementById('result-card');
const dateInput=document.getElementById('date-input'),timeInput=document.getElementById('time-input'),submitTime=document.getElementById('submit-time'),confirmDetail=document.getElementById('confirm-detail'),notifyBtn=document.getElementById('notify-btn');
const NOTIFY_PHONE='996774289256';
dateInput.min=new Date().toISOString().split('T')[0];
let dodges=0,armed=false;

function placeNoRandomly(){
 const margin=16,rect=no.getBoundingClientRect();
 const maxLeft=Math.max(margin,window.innerWidth-rect.width-margin);
 const maxTop=Math.max(margin,window.innerHeight-rect.height-margin);
 no.style.left=(margin+Math.random()*(maxLeft-margin))+'px';
 no.style.top=(margin+Math.random()*(maxTop-margin))+'px';
}
function dodge(){
 if(!armed){
  const r=no.getBoundingClientRect();
  no.style.left=r.left+'px';no.style.top=r.top+'px';no.classList.add('armed');armed=true;void no.offsetWidth;
 }
 placeNoRandomly();dodges++;
 if(dodges===1)tally.textContent='кнопка смущена и отступает.';
 else if(dodges<4)tally.textContent='попытка №'+dodges+' — снова мимо.';
 else if(dodges<8)tally.textContent='упорство похвально, но ответ уже очевиден.';
 else tally.textContent='право слово, просто нажмите «Соизволяю».';
}
no.addEventListener('mouseenter',dodge);
no.addEventListener('click',e=>{e.preventDefault();dodge()});
no.addEventListener('touchstart',e=>{e.preventDefault();dodge()},{passive:false});
window.addEventListener('resize',()=>{if(armed)placeNoRandomly()});

yes.addEventListener('click',()=>{
 askCard.classList.remove('show');scheduleCard.classList.add('show');
});
function formatDate(value){
 return new Date(value+'T00:00:00').toLocaleDateString('ru-RU',{day:'numeric',month:'long',year:'numeric'});
}
submitTime.addEventListener('click',()=>{
 if(!dateInput.value||!timeInput.value){dateInput.reportValidity();timeInput.reportValidity();return}
 confirmDetail.textContent=formatDate(dateInput.value)+', в '+timeInput.value;
 scheduleCard.classList.remove('show');resultCard.classList.add('show');
 notifyBtn.dataset.date=formatDate(dateInput.value);notifyBtn.dataset.time=timeInput.value;
});
notifyBtn.addEventListener('click',()=>{
 const dodgeLine=dodges===0?'на «нет» даже не покушалась — согласилась сразу.':'на «нет» покушалась '+dodges+(dodges===1?' раз':' раз(а)')+', но кнопка не далась.';
 const text='Отчёт об аудиенции: согласие получено. Дата — '+notifyBtn.dataset.date+', время — '+notifyBtn.dataset.time+'. '+dodgeLine;
 window.open('https://wa.me/'+NOTIFY_PHONE+'?text='+encodeURIComponent(text),'_blank','noopener');
 notifyBtn.textContent='Отправлено ✓';notifyBtn.classList.add('sent');
});
</script>
</body>
</html>
