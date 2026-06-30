
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PMS 위험도</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,sans-serif}
body{display:flex;justify-content:center;align-items:center;height:100vh;background:#fff}
.container{text-align:center;width:100%;max-width:420px;padding:24px}
#today{font-size:32px;font-weight:700;margin-bottom:70px}
.label{font-size:28px;margin-bottom:15px}
#danger{font-size:90px;font-weight:800;color:#ff3b30;margin-bottom:90px}
button{width:100%;padding:18px;font-size:22px;border:none;border-radius:16px;background:#007AFF;color:#fff;cursor:pointer}
.modal{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);justify-content:center;align-items:center}
.card{background:#fff;padding:24px;border-radius:18px;width:320px}
.card input{width:100%;padding:14px;font-size:18px;margin:10px 0}
</style>
</head>
<body>
<div class="container">
<div id="today"></div>
<div class="label">위험도</div>
<div id="danger">0%</div>
<button onclick="modal.style.display='flex'">추가 기록하기</button>
</div>

<div class="modal" id="modal">
<div class="card">
<input type="date" id="start">
<input type="date" id="end">
<button onclick="save()">저장</button>
</div>
</div>

<script>
const defaults=[
{start:"2025-01-24",end:"2025-01-30"},
{start:"2025-03-25",end:"2025-03-31"},
{start:"2025-05-15",end:"2025-05-20"},
{start:"2025-08-16",end:"2025-08-20"},
{start:"2025-09-28",end:"2025-10-03"},
{start:"2025-11-15",end:"2025-11-21"},
{start:"2026-02-06",end:"2026-02-11"},
{start:"2026-03-19",end:"2026-03-24"},
{start:"2026-05-26",end:"2026-05-31"},
{start:"2026-06-21",end:"2026-06-27"}];
if(!localStorage.records)localStorage.records=JSON.stringify(defaults);
const rec=()=>JSON.parse(localStorage.records);
function avg(){let r=rec(),a=[];for(let i=1;i<r.length;i++)a.push((new Date(r[i].start)-new Date(r[i-1].start))/86400000);return Math.round(a.reduce((x,y)=>x+y)/a.length)}
function risk(){
 let r=rec(),last=new Date(r[r.length-1].start),next=new Date(last);next.setDate(next.getDate()+avg());
 let t=new Date();t.setHours(0,0,0,0);let d=Math.floor((t-next)/86400000),v=5;
 if(d>=-7&&d<0){let x=Math.abs(d);v=30+(7-x)*10;}
 else if(d==0)v=95; else if(d==1)v=90; else if(d==2)v=40; else if(d==3)v=20; else if(d==4)v=10;
 danger.textContent=v+"%";
}
function save(){
 if(!start.value||!end.value)return alert("날짜를 입력하세요.");
 let r=rec();r.push({start:start.value,end:end.value});
 r.sort((a,b)=>new Date(a.start)-new Date(b.start));
 localStorage.records=JSON.stringify(r);
 modal.style.display='none';risk();
}
today.textContent=new Date().toLocaleDateString('ko-KR',{year:'numeric',month:'long',day:'numeric',weekday:'long'});
window.onclick=e=>{if(e.target===modal)modal.style.display='none';}
risk();
</script>
</body>
</html>
