<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>OMKAR SIR | PR GURUKUL</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#edf3fc;color:#17345f}
header{background:#153d78;color:white;text-align:center;padding:22px 12px}
header h1{margin:0;font-size:27px}
header p{margin:7px 0}
main{max-width:650px;margin:auto;padding:14px}
.card{background:white;border-radius:15px;padding:17px;margin:13px 0;box-shadow:0 3px 12px #0001}
h2{font-size:20px;margin-top:0}
input,select{width:100%;padding:12px;margin:6px 0 12px;border:1px solid #ccd7e8;border-radius:8px;font-size:16px}
button{background:#1765d1;color:white;border:0;border-radius:9px;padding:12px;font-size:15px;cursor:pointer}
.full{width:100%}
.secondary{background:#e5edff;color:#17345f}
.danger{background:#b42318}
.stats{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
.stat{background:#e9f1ff;padding:14px;border-radius:10px;overflow-wrap:anywhere}
.stat b{display:block;font-size:22px;margin-top:7px}
.item{border:1px solid #dce5f2;padding:12px;border-radius:10px;margin:10px 0;overflow-wrap:anywhere}
.item button{margin:5px 4px 0 0}
.hidden{display:none!important}
small{color:#64748b}
.pin{font-size:25px;letter-spacing:8px;text-align:center}
#receipt{background:white;color:#111;border:1px solid #d6dbe3;border-radius:10px;padding:22px;margin:14px 0}
#receipt h2,#receipt h3{text-align:center}
.receipt-line{display:flex;justify-content:space-between;gap:12px;padding:9px 0;border-bottom:1px solid #ddd}
.receipt-line span:last-child{text-align:right;overflow-wrap:anywhere}
.receipt-actions{display:flex;gap:8px;margin:10px 0}
.receipt-actions button{flex:1}
@media print{
body{background:white;color:#111}
body *{visibility:hidden!important}
#printArea,#printArea *{visibility:visible!important}
#printArea{display:block!important;position:absolute;left:0;top:0;width:100%;margin:0;padding:0}
#receipt{border:1px solid #888;border-radius:0;box-shadow:none;margin:0;padding:22px;break-inside:avoid}
.no-print{display:none!important}
}
</style>
</head>
<body>
<header class="no-print">
<h1>OMKAR SIR</h1>
<h2 style="margin:8px 0">PR GURUKUL</h2>
<p>Director: OMKAR SIR</p>
<p>Teachers: SANDEEP KUMAR<br>MITHILESH AGARWAL</p>
</header>
<main class="no-print">
<section id="auth" class="card">
<h2 id="authTitle">Owner Login</h2>
<p id="authInfo">Apna 6 digit PIN daalein.</p>
<form id="authForm">
<label id="pinLabel" for="pin">Owner PIN</label>
<input id="pin" class="pin" type="password" inputmode="numeric" pattern="[0-9]{6}" minlength="6" maxlength="6" autocomplete="off" required placeholder="6 digit PIN">
<div id="confirmWrap" class="hidden">
<label for="confirmPin">Confirm PIN</label>
<input id="confirmPin" class="pin" type="password" inputmode="numeric" pattern="[0-9]{6}" minlength="6" maxlength="6" autocomplete="off" placeholder="Confirm PIN">
</div>
<button id="authButton" class="full" type="submit">Login</button>
</form>
<p id="authMessage" role="status"></p>
<small>Yeh local PIN hai, server-based secure authentication nahi.</small>
</section>

<div id="app" class="hidden">
<section class="card">
<h2>Owner Dashboard</h2>
<div class="stats">
<div class="stat">Total Students<b id="total">0</b></div>
<div class="stat">Fees Collected<b id="collected">₹0</b></div>
<div class="stat">Fees Pending<b id="pending">₹0</b></div>
<div class="stat">Present Today<b id="present">0</b></div>
</div>
<button id="logout" class="full danger" style="margin-top:14px">Logout</button>
</section>

<section class="card">
<h2>Add Student</h2>
<form id="studentForm">
<label>Student Name</label><input id="name" required placeholder="Student ka naam">
<label>Mobile Number</label><input id="phone" inputmode="numeric" placeholder="Mobile number">
<label>Course / Batch</label><input id="course" required placeholder="Jaise BCA / Class 10">
<label>Total Fees (₹)</label><input id="fee" type="number" min="0" value="0" required>
<label>Already Paid (₹)</label><input id="paid" type="number" min="0" value="0" required>
<button class="full" type="submit">+ Save Student</button>
</form>
<small>Already Paid ki entry ke liye payment history automatic create hogi.</small>
</section>

<section class="card">
<h2>Manage Attendance</h2>
<p><small>Date: <span id="today"></span></small></p>
<div id="attendanceList"></div>
</section>

<section class="card">
<h2>Manage Students & Fees</h2>
<input id="search" placeholder="Naam, mobile ya course se search">
<div id="studentList"></div>
</section>

<section class="card">
<h2>Backup & Restore</h2>
<p><small>Backup mein student details aur payment history ho sakti hai. File private rakhein.</small></p>
<button id="backup" class="full">Download Backup (JSON)</button>
<label for="restoreFile">Restore Backup</label>
<input id="restoreFile" type="file" accept=".json,application/json">
<button id="restore" class="full secondary">Import Selected Backup</button>
<p id="backupMsg" role="status"></p>
</section>

<section class="card">
<h2>PIN Settings</h2>
<button id="changePin" class="full secondary">Change Owner PIN</button>
<small>PIN reset karne se pehle backup zaroor rakhein.</small>
</section>
</div>
</main>

<section id="printArea" class="hidden">
<div id="receipt"></div>
<div class="receipt-actions no-print">
<button id="printBtn">Print / Save as PDF</button>
<button id="closeReceipt" class="secondary">Back to Dashboard</button>
</div>
<p class="no-print" style="text-align:center;padding:0 15px">PDF ke liye print menu mein “Save as PDF” select karein.</p>
</section>

<script>
const KEY='omkar_pr_gurukul_students_v1';
const ATT='omkar_pr_gurukul_attendance_v1';
const PINKEY='omkar_pr_gurukul_pin_hash_v1';
const $=id=>document.getElementById(id);
let students=[],attendance={},mode='login',changeOldHash='',currentReceipt=null;

function readData(){
try{students=JSON.parse(localStorage.getItem(KEY)||'[]');if(!Array.isArray(students))students=[];}catch(e){students=[];}
try{attendance=JSON.parse(localStorage.getItem(ATT)||'{}');if(!attendance||typeof attendance!=='object')attendance={};}catch(e){attendance={};}
students.forEach(s=>{if(!Array.isArray(s.payments))s.payments=[];});
}
function day(){
const d=new Date();
return d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0')+'-'+String(d.getDate()).padStart(2,'0');
}
function dateTime(){
return new Date().toLocaleString('en-IN',{dateStyle:'medium',timeStyle:'short'});
}
function setMsg(t){$('authMessage').textContent=t;}
function validPin(p){return /^[0-9]{6}$/.test(p);}
async function hashPin(pin){
if(!window.crypto||!crypto.subtle)throw new Error('Secure Crypto available nahi. Code Studio ke browser preview ya HTTPS par kholein.');
const data=new TextEncoder().encode('OMKAR-SIR-LOCAL-PIN-v1:'+pin);
const digest=await crypto.subtle.digest('SHA-256',data);
return Array.from(new Uint8Array(digest)).map(b=>b.toString(16).padStart(2,'0')).join('');
}
function setupAuth(){
const saved=localStorage.getItem(PINKEY);
mode=saved?'login':'setup';
$('authTitle').textContent=saved?'Owner Login':'Set Your Owner PIN';
$('authInfo').textContent=saved?'Apna 6 digit PIN daalein.':'Pehli baar apna naya 6 digit PIN set karein.';
$('pinLabel').textContent=saved?'Owner PIN':'New 6 digit PIN';
$('confirmWrap').classList.toggle('hidden',!!saved);
$('authButton').textContent=saved?'Unlock Dashboard':'Set PIN & Continue';
$('pin').value='';$('confirmPin').value='';setMsg('');
$('auth').classList.remove('hidden');$('app').classList.add('hidden');
$('printArea').classList.add('hidden');
}
$('authForm').addEventListener('submit',async e=>{
e.preventDefault();
const pin=$('pin').value;
if(!validPin(pin)){setMsg('PIN 6 digits ka hona chahiye.');return;}
$('authButton').disabled=true;
try{
if(mode==='setup'){
if(pin!==$('confirmPin').value){setMsg('Dono PIN match nahi karte.');return;}
localStorage.setItem(PINKEY,await hashPin(pin));openApp();
}else if(mode==='login'){
if(await hashPin(pin)===localStorage.getItem(PINKEY))openApp();
else setMsg('PIN galat hai. Dobara koshish karein.');
}else if(mode==='change'){
if(await hashPin(pin)!==changeOldHash){setMsg('Current PIN galat hai.');return;}
mode='changeConfirm';$('authTitle').textContent='Set New PIN';
$('authInfo').textContent='Naya 6 digit PIN daalein aur confirm karein.';
$('pinLabel').textContent='New PIN';$('confirmWrap').classList.remove('hidden');
$('authButton').textContent='Save New PIN';$('pin').value='';$('confirmPin').value='';setMsg('');
}else if(mode==='changeConfirm'){
if(pin!==$('confirmPin').value){setMsg('Dono PIN match nahi karte.');return;}
localStorage.setItem(PINKEY,await hashPin(pin));alert('PIN update ho gaya.');openApp();
}
}catch(err){setMsg(err.message||'Error hua. Browser preview mein try karein.');}
finally{$('authButton').disabled=false;}
});
function openApp(){
readData();$('auth').classList.add('hidden');$('app').classList.remove('hidden');
$('printArea').classList.add('hidden');$('today').textContent=day();render();
}
$('logout').addEventListener('click',setupAuth);
$('changePin').addEventListener('click',()=>{
mode='change';changeOldHash=localStorage.getItem(PINKEY);
$('auth').classList.remove('hidden');$('app').classList.add('hidden');
$('authTitle').textContent='Verify Current PIN';
$('authInfo').textContent='PIN badalne se pehle current PIN daalein.';
$('pinLabel').textContent='Current PIN';$('confirmWrap').classList.add('hidden');
$('authButton').textContent='Continue';$('pin').value='';setMsg('');
});

$('studentForm').addEventListener('submit',e=>{
e.preventDefault();
const fee=Number($('fee').value),paid=Number($('paid').value);
if(fee<0||paid<0||paid>fee){alert('Paid fees total fees se zyada nahi ho sakti.');return;}
const s={id:Date.now().toString()+Math.random().toString(36).slice(2,7),name:$('name').value.trim(),phone:$('phone').value.trim(),course:$('course').value.trim(),fee,paid,payments:[]};
if(paid>0)s.payments.push({id:receiptId(),amount:paid,date:dateTime(),note:'Opening payment'});
students.push(s);save();e.target.reset();$('fee').value=0;$('paid').value=0;
alert('Student record save ho gaya.');
});

function el(tag,txt){const n=document.createElement(tag);n.textContent=txt;return n;}
function action(label,fn,cls=''){const b=el('button',label);b.type='button';b.className=cls;b.addEventListener('click',fn);return b;}
function save(){localStorage.setItem(KEY,JSON.stringify(students));localStorage.setItem(ATT,JSON.stringify(attendance));render();}
function receiptId(){return 'PRG-'+Date.now().toString().slice(-8)+'-'+Math.floor(Math.random()*90+10);}
function render(){
$('total').textContent=students.length;
$('collected').textContent='₹'+students.reduce((s,x)=>s+Number(x.paid||0),0);
$('pending').textContent='₹'+students.reduce((s,x)=>s+Math.max(0,Number(x.fee||0)-Number(x.paid||0)),0);
$('present').textContent=students.filter(x=>attendance[day()]&&attendance[day()][x.id]==='Present').length;
const q=$('search').value.toLowerCase(),list=$('studentList');list.replaceChildren();
const filtered=students.filter(x=>(x.name+' '+x.course+' '+x.phone).toLowerCase().includes(q));
if(!filtered.length)list.append(el('p',students.length?'Koi matching student nahi mila.':'Abhi koi student nahi hai.'));
filtered.forEach(x=>{
const d=document.createElement('div');d.className='item';
d.append(el('b',x.name),el('p','Course: '+x.course),el('p','Mobile: '+(x.phone||'Nahi diya')),el('p','Fees: ₹'+x.paid+' paid / ₹'+x.fee+' total'),el('p','Baaki: ₹'+Math.max(0,x.fee-x.paid)));
d.append(action('Collect Fees + Receipt',()=>collectFees(x.id)),action('Print Last Receipt',()=>lastReceipt(x.id),'secondary'),action('Update Total Paid',()=>updatePaid(x.id),'secondary'),action('Delete Student',()=>deleteStudent(x.id),'danger'));
list.append(d);
});
const al=$('attendanceList');al.replaceChildren();
if(!students.length){al.append(el('p','Pehle student add karein.'));return;}
students.forEach(x=>{
const d=document.createElement('div');d.className='item';d.append(el('b',x.name));
const s=document.createElement('select');
[['','Not marked'],['Present','Present'],['Absent','Absent']].forEach(([v,t])=>{const o=el('option',t);o.value=v;s.append(o);});
s.value=(attendance[day()]||{})[x.id]||'';
s.addEventListener('change',()=>{
if(!attendance[day()])attendance[day()]={};
if(s.value)attendance[day()][x.id]=s.value;else delete attendance[day()][x.id];
save();
});d.append(s);al.append(d);
});
$('present').textContent=students.filter(x=>attendance[day()]&&attendance[day()][x.id]==='Present').length;
}
function collectFees(id){
const x=students.find(s=>s.id===id);if(!x)return;
const remaining=Math.max(0,Number(x.fee)-Number(x.paid));
if(remaining<=0){alert('Is student ki total fees already paid hai. Total fees badhani ho to Update Total Paid se pehle fee record check karein.');return;}
const raw=prompt('Aaj kitni fees receive hui? (₹)\nRemaining: ₹'+remaining,'');
if(raw===null)return;
if(raw.trim()===''){alert('Amount daliye.');return;}
const amount=Number(raw);
if(!Number.isFinite(amount)||amount<=0||amount>remaining){alert('Amount 0 se zyada aur remaining fees se kam ya barabar honi chahiye.');return;}
const payment={id:receiptId(),amount,date:dateTime(),note:'Fees payment'};
if(!Array.isArray(x.payments))x.payments=[];
x.payments.push(payment);x.paid=Number(x.paid)+amount;save();
showReceipt(x,payment);
}
function updatePaid(id){
const x=students.find(s=>s.id===id);if(!x)return;
const raw=prompt('Total fees paid ab tak (₹):',String(x.paid));if(raw===null)return;
const n=Number(raw);
if(!Number.isFinite(n)||n<0||n>x.fee){alert('Sahi amount daliye.');return;}
if(n>x.paid){
const amount=n-x.paid;
if(!Array.isArray(x.payments))x.payments=[];
const payment={id:receiptId(),amount,date:dateTime(),note:'Paid amount adjustment'};
x.payments.push(payment);x.paid=n;save();showReceipt(x,payment);
}else if(n<x.paid){
if(!confirm('Paid total kam karne par payment history automatically delete nahi hogi. Continue?'))return;
x.paid=n;save();
}else alert('Paid total mein koi badlav nahi hua.');
}
function deleteStudent(id){
const x=students.find(s=>s.id===id);if(!x)return;
if(confirm(x.name+' ka record delete karein?')){
students=students.filter(s=>s.id!==id);
Object.values(attendance).forEach(a=>{if(a)delete a[id];});save();
}
}
function safeText(value){return String(value??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));}
function line(label,value){
return '<div class="receipt-line"><span>'+safeText(label)+'</span><strong>'+safeText(value)+'</strong></div>';
}
function showReceipt(student,payment){
currentReceipt={studentId:student.id,paymentId:payment.id};
const paid=Number(student.paid||0),fee=Number(student.fee||0);
$('receipt').innerHTML=
'<h2>OMKAR SIR</h2><h3>PR GURUKUL</h3>'+
'<p style="text-align:center">FEES PAYMENT RECEIPT</p>'+
'<hr><p><strong>Receipt No:</strong> '+safeText(payment.id)+'</p>'+
'<p><strong>Date:</strong> '+safeText(payment.date)+'</p><hr>'+
line('Student Name',student.name)+
line('Mobile',student.phone||'Not provided')+
line('Course / Batch',student.course)+
line('Total Fees','₹'+fee.toFixed(2))+
line('Previous Paid','₹'+(paid-Number(payment.amount)).toFixed(2))+
line('Amount Received','₹'+Number(payment.amount).toFixed(2))+
line('Total Paid Till Date','₹'+paid.toFixed(2))+
line('Remaining Fees','₹'+Math.max(0,fee-paid).toFixed(2))+
'<p style="margin-top:35px">Receiver Signature: __________________</p>'+
'<p style="text-align:center;font-size:12px;margin-top:24px">Thank you — PR GURUKUL</p>';
$('app').classList.add('hidden');$('auth').classList.add('hidden');
$('printArea').classList.remove('hidden');window.scrollTo(0,0);
}
function lastReceipt(id){
const x=students.find(s=>s.id===id);if(!x)return;
if(!Array.isArray(x.payments)||!x.payments.length){alert('Is student ki payment history mein receipt nahi hai.');return;}
showReceipt(x,x.payments[x.payments.length-1]);
}
$('printBtn').addEventListener('click',()=>window.print());
$('closeReceipt').addEventListener('click',()=>{$('printArea').classList.add('hidden');$('app').classList.remove('hidden');window.scrollTo(0,0);});
$('search').addEventListener('input',render);

$('backup').addEventListener('click',()=>{
const blob=new Blob([JSON.stringify({format:'omkar-gurukul-backup-v2',students,attendance},null,2)],{type:'application/json'});
const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download='omkar-gurukul-backup.json';a.click();setTimeout(()=>URL.revokeObjectURL(url),1500);
$('backupMsg').textContent='Backup download shuru hua. File safe rakhein.';
});
$('restore').addEventListener('click',async()=>{
const f=$('restoreFile').files[0];if(!f){$('backupMsg').textContent='Pehle JSON backup file select karein.';return;}
try{
const obj=JSON.parse(await f.text());
if(!obj||!Array.isArray(obj.students)||!obj.attendance||typeof obj.attendance!=='object')throw new Error('Backup format sahi nahi hai.');
obj.students.forEach(s=>{if(!s.id||typeof s.name!=='string'||!Number.isFinite(Number(s.fee))||!Number.isFinite(Number(s.paid)))throw new Error('Student record valid nahi hai.');if(!Array.isArray(s.payments))s.payments=[];});
if(!confirm('Current records replace honge. Pehle current backup download kiya hai?'))return;
students=obj.students;attendance=obj.attendance;save();$('backupMsg').textContent='Backup restore ho gaya.';
}catch(e){$('backupMsg').textContent='Backup restore nahi hua: '+e.message;}
});
setupAuth();
</script>
</body>
</html>
