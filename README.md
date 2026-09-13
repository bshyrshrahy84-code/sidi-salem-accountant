<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>المحاسب الذكي — Smart Accountant</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#16211D; --paper:#F5F2EA; --surface:#FFFFFF; --line:#E4DFD1;
    --deep:#1B4332; --deep-2:#123526; --gold:#AD8A34; --gold-soft:#F1E6C8;
    --income:#2F6F4E; --income-soft:#E4F0E7; --expense:#A6402F; --expense-soft:#F5E4E0;
    --owed-to-me:#2E5C74; --owed-to-me-soft:#E2ECF0; --owed-by-me:#8A5A22; --owed-by-me-soft:#F1E7D8;
    --muted:#7A7264; --radius-s:6px; --radius-m:10px;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{font-family:'Tajawal',sans-serif; background:var(--paper); color:var(--ink); display:flex; justify-content:center; -webkit-font-smoothing:antialiased;}
  #phone{width:100%; max-width:430px; min-height:100vh; background:var(--paper); display:flex; flex-direction:column; position:relative;}
  header.appbar{padding:20px 20px 14px; background:var(--deep); color:#fff; position:relative; overflow:hidden;}
  header.appbar::after{content:""; position:absolute; left:-40px; bottom:-60px; width:160px; height:160px; border-radius:50%; border:1px solid rgba(255,255,255,0.08);}
  .brand-row{display:flex; align-items:baseline; justify-content:space-between; position:relative; z-index:1;}
  .brand-title{font-size:19px; font-weight:800;}
  .brand-sub{font-size:10.5px; color:rgba(255,255,255,0.55); font-weight:500;}
  .brand-owner{margin-top:2px; font-size:10.5px; color:rgba(255,255,255,0.4); position:relative; z-index:1;}
  .view-title{font-size:14.5px; font-weight:700; color:rgba(255,255,255,0.85); margin-top:12px; position:relative; z-index:1; display:flex; align-items:center; gap:8px;}
  .back-chip{font-size:11px; font-weight:700; color:rgba(255,255,255,0.6); border:1px solid rgba(255,255,255,0.25); border-radius:20px; padding:3px 10px; cursor:pointer;}
  main{flex:1; padding:16px 16px 90px; overflow-y:auto;}
  .search-box{margin-bottom:12px;}
  .search-box input{width:100%; border:1px solid var(--line); border-radius:20px; padding:9px 14px; font-family:inherit; font-size:13px; background:var(--surface);}
  .hero{background:var(--deep-2); color:#fff; border-radius:var(--radius-m); padding:20px; margin-bottom:14px;}
  .hero-label{font-size:12px; color:rgba(255,255,255,0.6); font-weight:500;}
  .hero-amount{font-size:29px; font-weight:800; margin-top:6px; direction:ltr; text-align:right;}
  .hero-amount .cur{font-size:14px; font-weight:500; color:rgba(255,255,255,0.55); margin-right:6px;}
  .hero-sub{font-size:12px; color:rgba(255,255,255,0.65); margin-top:6px; direction:ltr; text-align:right;}
  .hero-rule{height:1px; background:rgba(255,255,255,0.12); margin:12px 0 10px;}
  .hero-foot{font-size:10px; color:rgba(255,255,255,0.5); line-height:1.6;}
  .stat-grid{display:grid; grid-template-columns:1fr 1fr; gap:9px; margin-bottom:14px;}
  .stat{background:var(--surface); border:1px solid var(--line); border-radius:var(--radius-s); padding:12px 13px; cursor:pointer;}
  .stat-label{font-size:10.5px; color:var(--muted); font-weight:500;}
  .stat-value{font-size:17px; font-weight:800; margin-top:4px; direction:ltr; text-align:right;}
  .stat.income .stat-value{color:var(--income);} .stat.expense .stat-value{color:var(--expense);}
  .stat.owed-to-me .stat-value{color:var(--owed-to-me);} .stat.owed-by-me .stat-value{color:var(--owed-by-me);}
  .stat.capital .stat-value{color:var(--deep);}
  .menu-grid{display:grid; grid-template-columns:1fr 1fr; gap:9px; margin-bottom:16px;}
  .menu-btn{background:var(--surface); border:1px solid var(--line); border-radius:var(--radius-s); padding:14px 12px; text-align:right; cursor:pointer; font-family:inherit;}
  .menu-btn .t{font-size:13px; font-weight:800;} .menu-btn .s{font-size:10.5px; color:var(--muted); margin-top:3px;}
  section.block{margin-bottom:18px;}
  .block-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:9px;}
  .block-head h2{font-size:14px; font-weight:700; margin:0;}
  .tx-row{display:flex; align-items:center; gap:10px; padding:10px 0; border-bottom:1px solid var(--line);}
  .tx-row:last-child{border-bottom:none;}
  .tx-icon{width:33px; height:33px; border-radius:8px; display:flex; align-items:center; justify-content:center; flex-shrink:0; font-size:14px; font-weight:800;}
  .tx-icon.income{background:var(--income-soft); color:var(--income);}
  .tx-icon.expense{background:var(--expense-soft); color:var(--expense);}
  .tx-icon.capital{background:var(--gold-soft); color:var(--gold);}
  .tx-icon.owed-to-me{background:var(--owed-to-me-soft); color:var(--owed-to-me);}
  .tx-icon.owed-by-me{background:var(--owed-by-me-soft); color:var(--owed-by-me);}
  .tx-body{flex:1; min-width:0;} .tx-title{font-size:13px; font-weight:700;} .tx-sub{font-size:10.5px; color:var(--muted); margin-top:1px;}
  .tx-amount{font-size:13.5px; font-weight:800; direction:ltr;} .tx-amount.pos{color:var(--income);} .tx-amount.neg{color:var(--expense);}
  .empty{text-align:center; padding:26px 10px; color:var(--muted); font-size:12.5px; background:var(--surface); border:1px dashed var(--line); border-radius:var(--radius-s);}
  .entry-card{background:var(--surface); border:1px solid var(--line); border-radius:var(--radius-s); padding:13px 14px; margin-bottom:9px;}
  .entry-top{display:flex; justify-content:space-between; align-items:flex-start;}
  .entry-amount{font-size:16px; font-weight:800; direction:ltr;}
  .entry-meta{font-size:10.5px; color:var(--muted); margin-top:5px; display:flex; gap:7px; flex-wrap:wrap;}
  .entry-note{font-size:12px; color:var(--ink); margin-top:6px; opacity:0.85;}
  .tag{display:inline-block; font-size:10px; font-weight:700; padding:2px 8px; border-radius:20px; background:var(--paper); color:var(--muted);}
  .badge{display:inline-block; font-size:10px; font-weight:800; padding:3px 9px; border-radius:20px;}
  .badge.full{background:var(--income-soft); color:var(--income);}
  .badge.partial{background:var(--owed-by-me-soft); color:var(--owed-by-me);}
  .badge.none{background:var(--expense-soft); color:var(--expense);}
  .debt-card{background:var(--surface); border:1px solid var(--line); border-radius:var(--radius-s); padding:14px; margin-bottom:10px;}
  .debt-top{display:flex; justify-content:space-between; align-items:center;}
  .debt-person{font-size:14px; font-weight:800;}
  .debt-remaining{font-size:16px; font-weight:800; direction:ltr;}
  .debt-card.to-me .debt-remaining{color:var(--owed-to-me);} .debt-card.by-me .debt-remaining{color:var(--owed-by-me);}
  .debt-bar-track{height:6px; background:var(--paper); border-radius:6px; margin-top:10px; overflow:hidden;}
  .debt-bar-fill{height:100%; border-radius:6px;}
  .debt-card.to-me .debt-bar-fill{background:var(--owed-to-me);} .debt-card.by-me .debt-bar-fill{background:var(--owed-by-me);}
  .debt-foot{display:flex; justify-content:space-between; margin-top:8px; font-size:10.5px; color:var(--muted);}
  .debt-actions{display:flex; gap:8px; margin-top:11px;}
  .btn-mini{flex:1; padding:8px; border-radius:var(--radius-s); border:1px solid var(--line); background:var(--paper); font-family:inherit; font-size:11.5px; font-weight:700; color:var(--ink); cursor:pointer;}
  .btn-mini.primary{background:var(--deep); color:#fff; border-color:var(--deep);}
  .btn-mini.gold{background:var(--gold); color:#fff; border-color:var(--gold);}
  .tabs{display:flex; gap:8px; margin-bottom:14px;}
  .tab-btn{flex:1; padding:9px 4px; text-align:center; border-radius:20px; border:1px solid var(--line); background:var(--surface); color:var(--muted); font-family:inherit; font-size:12px; font-weight:700; cursor:pointer;}
  .tab-btn.active.to-me{background:var(--owed-to-me-soft); color:var(--owed-to-me); border-color:var(--owed-to-me-soft);}
  .tab-btn.active.by-me{background:var(--owed-by-me-soft); color:var(--owed-by-me); border-color:var(--owed-by-me-soft);}
  .fab{position:absolute; left:16px; bottom:78px; width:52px; height:52px; border-radius:50%; background:var(--gold); color:#fff; border:none; font-size:26px; display:flex; align-items:center; justify-content:center; box-shadow:0 6px 14px rgba(173,138,52,0.4); cursor:pointer; z-index:20;}
  nav.tabbar{position:absolute; bottom:0; left:0; right:0; background:var(--surface); border-top:1px solid var(--line); display:flex; padding:6px 4px 10px; max-width:430px; margin:0 auto;}
  .navitem{flex:1; display:flex; flex-direction:column; align-items:center; gap:3px; padding:6px 2px; background:none; border:none; font-family:inherit; color:var(--muted); cursor:pointer;}
  .navitem.active{color:var(--deep);} .navitem svg{width:19px; height:19px;} .navitem span{font-size:9.5px; font-weight:700;}
  .sheet-backdrop{position:absolute; inset:0; background:rgba(20,25,20,0.45); z-index:40; display:flex; align-items:flex-end;}
  .sheet{width:100%; background:var(--surface); border-radius:16px 16px 0 0; padding:18px 18px 22px; max-height:88vh; overflow-y:auto;}
  .sheet-handle{width:36px; height:4px; background:var(--line); border-radius:4px; margin:0 auto 14px;}
  .sheet h3{margin:0 0 14px; font-size:15.5px; font-weight:800;}
  .field{margin-bottom:12px;}
  .field label{display:block; font-size:12px; font-weight:700; color:var(--muted); margin-bottom:5px;}
  .field input, .field textarea, .field select{width:100%; border:1px solid var(--line); border-radius:var(--radius-s); padding:10px 12px; font-family:inherit; font-size:14px; background:var(--paper); color:var(--ink);}
  .field input:focus, .field textarea:focus, .field select:focus{outline:2px solid var(--deep); outline-offset:0;}
  .field textarea{resize:none; min-height:56px;}
  .field-row{display:flex; gap:8px;} .field-row .field{flex:1;}
  .radio-row{display:flex; gap:8px;}
  .radio-opt{flex:1; text-align:center; padding:10px; border-radius:var(--radius-s); border:1px solid var(--line); font-size:12.5px; font-weight:700; color:var(--muted); cursor:pointer;}
  .radio-opt.selected{border-color:var(--deep); background:var(--deep); color:#fff;}
  .sheet-actions{display:flex; gap:10px; margin-top:6px;}
  .btn{flex:1; padding:12px; border-radius:var(--radius-s); border:none; font-family:inherit; font-size:14px; font-weight:800; cursor:pointer;}
  .btn.cancel{background:var(--paper); color:var(--muted); border:1px solid var(--line);} .btn.save{background:var(--deep); color:#fff;}
  .err{color:var(--expense); font-size:11.5px; margin-top:-6px; margin-bottom:10px; display:none;}
  .final-row{display:flex; justify-content:space-between; align-items:center; padding:10px 0; border-bottom:1px solid var(--line); font-size:12.5px;}
  .final-row:last-child{border-bottom:none;}
  .final-row .lbl{color:var(--muted); font-weight:500;} .final-row .val{font-weight:800; direction:ltr;}
  .final-row.plus .val{color:var(--income);} .final-row.minus .val{color:var(--expense);}
  .final-row.total{border-top:2px solid var(--ink); border-bottom:none; margin-top:6px; padding-top:14px;}
  .final-row.total .lbl{color:var(--ink); font-weight:800; font-size:13.5px;} .final-row.total .val{font-size:19px;}
  .section-title{font-size:12px; font-weight:800; color:var(--gold); margin:16px 0 4px;}
  #printInvoice{display:none;}
  @media print{
    body.printing > *:not(#printInvoice){display:none !important;}
    body.printing{display:block !important; background:#fff;}
    body.printing #printInvoice{display:block !important; width:100%; padding:20px; font-family:'Tajawal',sans-serif;}
  }
  .inv-head{display:flex; justify-content:space-between; align-items:flex-start; border-bottom:2px solid #1B4332; padding-bottom:14px; margin-bottom:18px;}
  .inv-head h1{font-size:20px; margin:0; color:#1B4332;} .inv-head p{font-size:11px; color:#666; margin:2px 0 0;}
  .inv-meta{font-size:12px; text-align:left; direction:ltr;}
  .inv-table{width:100%; border-collapse:collapse; margin-bottom:18px;}
  .inv-table th, .inv-table td{border:1px solid #ccc; padding:8px 10px; font-size:12.5px; text-align:right;}
  .inv-table th{background:#F5F2EA;}
  .inv-summary{width:65%; margin-right:auto; margin-left:0;}
  .inv-summary .r{display:flex; justify-content:space-between; padding:6px 0; font-size:13px;}
  .inv-summary .r.total{border-top:2px solid #1B4332; font-weight:800; font-size:15px; padding-top:10px;}
  .inv-note{margin-top:24px; font-size:11px; color:#777;}
  ::selection{background:var(--gold-soft);}
</style>
</head>
<body>
<div id="phone">

  <header class="appbar">
    <div class="brand-row">
      <div><div class="brand-title">المحاسب الذكي</div><div class="brand-sub">Smart Accountant</div></div>
    </div>
    <div class="brand-owner">صاحب المشروع: شراحي محمد البشير</div>
    <div class="view-title">
      <span id="viewTitle">الرئيسية</span>
      <span class="back-chip" id="backChip" style="display:none;" onclick="goHome()">رجوع للرئيسية</span>
    </div>
  </header>

  <main id="main"></main>
  <button class="fab" id="fabBtn" title="إضافة">+</button>

  <nav class="tabbar">
    <button class="navitem" data-tab="home"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M3 11l9-7 9 7"/><path d="M5 10v10h14V10"/></svg><span>الرئيسية</span></button>
    <button class="navitem" data-tab="capital"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="3" y="7" width="18" height="12" rx="2"/><path d="M3 10h18"/><circle cx="8" cy="14.5" r="1.4"/></svg><span>رأس المال</span></button>
    <button class="navitem" data-tab="purchases"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 7h16l-1.5 10.5a2 2 0 0 1-2 1.5H7.5a2 2 0 0 1-2-1.5L4 7Z"/><path d="M8 7V5a4 4 0 0 1 8 0v2"/></svg><span>المشتريات</span></button>
    <button class="navitem" data-tab="debts"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M8 12h8"/><circle cx="12" cy="12" r="9"/></svg><span>الديون</span></button>
    <button class="navitem" data-tab="more"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="5" cy="12" r="1.6"/><circle cx="12" cy="12" r="1.6"/><circle cx="19" cy="12" r="1.6"/></svg><span>المزيد</span></button>
  </nav>

  <div id="sheetHost"></div>
  <div id="printInvoice"></div>
</div>

<script>
/* ================= State ================= */
let STATE = { capital:[], income:[], expenses:[], purchases:[], debts:[], transactions:[] };
let currentTab = 'home';
let debtFilter = 'toMe';
let searchPurchases = '', searchDebts = '', searchPayments = '', searchUnpaid = '';

const STORAGE_KEY = 'smart-accountant-data-v3';
const SECONDARY_TABS = ['income','expenses','payments','unpaid','final'];
const PAY_METHODS = ['نقداً','تحويل بنكي','بريد الجزائر CCP','أخرى'];

function uid(){ return Date.now().toString(36) + Math.random().toString(36).slice(2,7); }
function todayStr(){ return new Date().toISOString().slice(0,10); }
function fmt(n){ n = Number(n)||0; return n.toLocaleString('en-US',{maximumFractionDigits:0}); }
function fmtDate(iso){
  if(!iso) return '—';
  const d = new Date(iso+'T00:00:00');
  const months = ['جانفي','فيفري','مارس','أفريل','ماي','جوان','جويلية','أوت','سبتمبر','أكتوبر','نوفمبر','ديسمبر'];
  return d.getDate() + ' ' + months[d.getMonth()] + ' ' + d.getFullYear();
}
function escapeHtml(s){ return String(s||'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }
function optionsHtml(list, selected){ return list.map(o=>`<option value="${o}" ${o===selected?'selected':''}>${o}</option>`).join(''); }

async function loadState(){
  try{
    const res = await window.storage.get(STORAGE_KEY, false);
    if(res && res.value){
      STATE = Object.assign({capital:[],income:[],expenses:[],purchases:[],debts:[],transactions:[]}, JSON.parse(res.value));
    }
  }catch(e){ /* first run */ }
}
async function saveState(){
  try{ await window.storage.set(STORAGE_KEY, JSON.stringify(STATE), false); }catch(e){ console.error(e); }
}

/* ================= Computations ================= */
function sum(arr,key){ return arr.reduce((a,b)=>a+(Number(b[key])||0),0); }
function paidOf(item){ return sum(item.payments||[],'amount'); }
function remainingOf(item){ return Math.max((Number(item.amount)||0) - paidOf(item), 0); }
function statusOf(item){
  const paid = paidOf(item), total = Number(item.amount)||0;
  if(paid<=0) return 'none';
  if(paid>=total) return 'full';
  return 'partial';
}
function statusLabel(s){ return s==='full' ? 'مدفوع بالكامل' : s==='partial' ? 'مدفوع جزئياً' : 'غير مدفوع'; }

function purchaseTotal(p){
  const gross = (Number(p.qty)||1) * (Number(p.unitPrice)||0);
  return Math.max(gross - (Number(p.discount)||0), 0);
}

function totals(){
  let capIn=0, capOut=0;
  STATE.capital.forEach(c=>{ if(c.kind==='initial'||c.kind==='addition') capIn += Number(c.amount)||0; else capOut += Number(c.amount)||0; });
  const totalCapital = capIn - capOut;
  const initialCapital = sum(STATE.capital.filter(c=>c.kind==='initial'),'amount');
  const additions = sum(STATE.capital.filter(c=>c.kind==='addition'),'amount');
  const withdrawals = sum(STATE.capital.filter(c=>c.kind==='withdrawal'),'amount');

  const totalIncome = sum(STATE.income,'amount');
  const totalExpenses = sum(STATE.expenses,'amount');

  let purchasesTotal=0, purchasesPaid=0, purchasesRemaining=0;
  STATE.purchases.forEach(p=>{
    const amt = purchaseTotal(p);
    purchasesTotal += amt; purchasesPaid += paidOf({amount:amt, payments:p.payments}); purchasesRemaining += remainingOf({amount:amt, payments:p.payments});
  });

  let owedToMe=0, owedByMe=0;
  STATE.debts.forEach(d=>{ if(d.direction==='toMe') owedToMe += remainingOf(d); else owedByMe += remainingOf(d); });

  const cashOnHand = totalCapital + totalIncome - totalExpenses - purchasesPaid;
  const unpaidByMe = purchasesRemaining + owedByMe;
  const net = cashOnHand + owedToMe - owedByMe - purchasesRemaining;

  return {totalCapital, initialCapital, additions, withdrawals, totalIncome, totalExpenses,
    purchasesTotal, purchasesPaid, purchasesRemaining, owedToMe, owedByMe, unpaidByMe, cashOnHand, net};
}

function logTx(type,label,amount,sign){ STATE.transactions.unshift({id:uid(), date:todayStr(), type, label, amount, sign}); }

/* ================= Render ================= */
const titles = {home:'الرئيسية', capital:'رأس المال', purchases:'المشتريات', debts:'أجندة الديون',
  more:'المزيد', income:'المداخيل', expenses:'المصاريف', payments:'المدفوعات', unpaid:'غير المسددين', final:'الحساب النهائي'};

const THEMES = {
  home:      {p:'#1B4332', d:'#123526'},
  capital:   {p:'#8A6D1B', d:'#5C4A12'},
  income:    {p:'#2F6F4E', d:'#1E4A34'},
  expenses:  {p:'#A6402F', d:'#6E2A1F'},
  purchases: {p:'#8A5A22', d:'#5C3B16'},
  debts:     {p:'#2E5C74', d:'#1E3E4E'},
  payments:  {p:'#5B4B8A', d:'#3B3159'},
  unpaid:    {p:'#7A2E2E', d:'#4E1E1E'},
  final:     {p:'#16211D', d:'#0B120F'},
  more:      {p:'#4A4438', d:'#302B22'}
};
function applyTheme(){
  const th = THEMES[currentTab] || THEMES.home;
  document.documentElement.style.setProperty('--deep', th.p);
  document.documentElement.style.setProperty('--deep-2', th.d);
}

function goHome(){ currentTab='home'; render(); }

function render(){
  applyTheme();
  document.getElementById('viewTitle').textContent = titles[currentTab];
  document.getElementById('backChip').style.display = SECONDARY_TABS.includes(currentTab) ? 'inline-block' : 'none';
  document.querySelectorAll('.navitem').forEach(b=>{
    const secMap = SECONDARY_TABS.includes(currentTab) && b.dataset.tab==='more';
    b.classList.toggle('active', b.dataset.tab===currentTab || secMap);
  });
  document.getElementById('fabBtn').style.display = ['payments','unpaid','final','more'].includes(currentTab) ? 'none' : 'flex';

  const main = document.getElementById('main');
  const renderers = {home:renderHome, capital:renderCapital, income:renderIncome, expenses:renderExpenses,
    purchases:renderPurchases, debts:renderDebts, payments:renderPayments, unpaid:renderUnpaid, final:renderFinal, more:renderMore};
  main.innerHTML = renderers[currentTab] ? renderers[currentTab]() : '';
  attachDynamicHandlers();
}

function txIconLabel(type){
  const map = {capital:{icon:'ر',cls:'capital'}, income:{icon:'+',cls:'income'}, expense:{icon:'−',cls:'expense'},
    purchaseNew:{icon:'ش',cls:'owed-by-me'}, purchasePay:{icon:'✓',cls:'owed-by-me'},
    debtNewToMe:{icon:'س',cls:'owed-to-me'}, debtNewByMe:{icon:'س',cls:'owed-by-me'},
    debtPayToMe:{icon:'✓',cls:'owed-to-me'}, debtPayByMe:{icon:'✓',cls:'owed-by-me'}};
  return map[type] || {icon:'•',cls:'capital'};
}

function renderHome(){
  const t = totals();
  const recent = STATE.transactions.slice(0,8);
  const recentHtml = recent.length===0 ? `<div class="empty">ماكانش أي عملية مسجلة بعد</div>` :
    recent.map(tx=>{
      const ic = txIconLabel(tx.type); const sign = tx.sign>0?'pos':'neg'; const sc = tx.sign>0?'+':'−';
      return `<div class="tx-row"><div class="tx-icon ${ic.cls}">${ic.icon}</div>
        <div class="tx-body"><div class="tx-title">${escapeHtml(tx.label)}</div><div class="tx-sub">${fmtDate(tx.date)}</div></div>
        <div class="tx-amount ${sign}">${sc} ${fmt(tx.amount)}</div></div>`;
    }).join('');
  return `
    <div class="hero">
      <div class="hero-label">الوضع المالي النهائي (صافي)</div>
      <div class="hero-amount">${fmt(t.net)} <span class="cur">دج</span></div>
      <div class="hero-sub">الرصيد النقدي فعلياً: ${fmt(t.cashOnHand)} دج</div>
      <div class="hero-rule"></div>
      <div class="hero-foot">الرصيد النقدي = رأس المال + المداخيل − المصاريف − مشتريات مدفوعة · الوضع النهائي = الرصيد النقدي + ديون لك − ديون عليك − مشتريات متبقية</div>
    </div>
    <div class="stat-grid">
      <div class="stat capital" data-nav="capital"><div class="stat-label">رأس المال الحالي</div><div class="stat-value">${fmt(t.totalCapital)}</div></div>
      <div class="stat income" data-nav="income"><div class="stat-label">مجموع المداخيل</div><div class="stat-value">${fmt(t.totalIncome)}</div></div>
      <div class="stat expense" data-nav="expenses"><div class="stat-label">مجموع المصاريف</div><div class="stat-value">${fmt(t.totalExpenses)}</div></div>
      <div class="stat owed-by-me" data-nav="purchases"><div class="stat-label">مجموع المشتريات</div><div class="stat-value">${fmt(t.purchasesTotal)}</div></div>
      <div class="stat owed-to-me" data-nav="debts"><div class="stat-label">ديون لك</div><div class="stat-value">${fmt(t.owedToMe)}</div></div>
      <div class="stat owed-by-me" data-nav="debts"><div class="stat-label">ديون عليك</div><div class="stat-value">${fmt(t.owedByMe)}</div></div>
      <div class="stat expense" data-nav="unpaid" style="grid-column:1/3;"><div class="stat-label">إجمالي المبالغ غير المدفوعة (عليك للغير)</div><div class="stat-value">${fmt(t.unpaidByMe)}</div></div>
    </div>
    <section class="block"><div class="block-head"><h2>آخر العمليات</h2></div>${recentHtml}</section>
  `;
}

function renderCapital(){
  const t = totals();
  const list = STATE.capital.length===0 ? `<div class="empty">ماكانش أي رأس مال مسجل. اضغط + باش تبدأ.</div>` :
    [...STATE.capital].reverse().map(c=>{
      const kindLabel = c.kind==='initial'?'رأس مال أولي':c.kind==='addition'?'إضافة':'سحب من رأس المال';
      const isOut = c.kind==='withdrawal';
      return `<div class="entry-card">
        <div class="entry-top"><span class="tag">${kindLabel}</span><div class="entry-amount" style="color:${isOut?'var(--expense)':'var(--ink)'};">${isOut?'−':''} ${fmt(c.amount)} دج</div></div>
        <div class="entry-meta"><span>${fmtDate(c.date)}</span>${c.reason?`<span>· ${escapeHtml(c.reason)}</span>`:''}</div>
        ${c.note?`<div class="entry-note">${escapeHtml(c.note)}</div>`:''}
      </div>`;
    }).join('');
  return `
    <div class="stat-grid" style="margin-bottom:8px;">
      <div class="stat"><div class="stat-label">رأس المال الأولي</div><div class="stat-value" style="font-size:16px;">${fmt(t.initialCapital)}</div></div>
      <div class="stat income"><div class="stat-label">إضافات</div><div class="stat-value" style="font-size:16px;">${fmt(t.additions)}</div></div>
      <div class="stat expense"><div class="stat-label">سحوبات</div><div class="stat-value" style="font-size:16px;">${fmt(t.withdrawals)}</div></div>
      <div class="stat capital"><div class="stat-label">رأس المال الحالي</div><div class="stat-value" style="font-size:16px;">${fmt(t.totalCapital)}</div></div>
    </div>
    <section class="block" style="margin-top:16px;"><div class="block-head"><h2>سجل رأس المال</h2></div>${list}</section>
  `;
}

function renderIncome(){
  const t = totals();
  const list = STATE.income.length===0 ? `<div class="empty">ماكانش أي دخل مسجل. اضغط + باش تسجل أول دخل.</div>` :
    [...STATE.income].reverse().map(i=>`
      <div class="entry-card">
        <div class="entry-top"><span class="tag">${escapeHtml(i.source||'دخل')}</span><div class="entry-amount" style="color:var(--income);">+ ${fmt(i.amount)} دج</div></div>
        <div class="entry-meta"><span>${fmtDate(i.date)}</span>${i.method?`<span>· ${i.method}</span>`:''}${i.party?`<span>· ${escapeHtml(i.party)}</span>`:''}</div>
        ${i.note?`<div class="entry-note">${escapeHtml(i.note)}</div>`:''}
      </div>`).join('');
  return `
    <div class="stat income" style="margin-bottom:16px;"><div class="stat-label">إجمالي المداخيل</div><div class="stat-value" style="font-size:22px;">${fmt(t.totalIncome)} دج</div></div>
    <section class="block"><div class="block-head"><h2>سجل المداخيل</h2></div>${list}</section>
  `;
}

function renderExpenses(){
  const t = totals();
  const list = STATE.expenses.length===0 ? `<div class="empty">ماكانش أي مصروف مسجل. اضغط + باش تسجل أول مصروف.</div>` :
    [...STATE.expenses].reverse().map(x=>`
      <div class="entry-card">
        <div class="entry-top"><span class="tag">${escapeHtml(x.category||'مصروف')}</span><div class="entry-amount" style="color:var(--expense);">− ${fmt(x.amount)} دج</div></div>
        <div class="entry-meta"><span>${fmtDate(x.date)}</span>${x.method?`<span>· ${x.method}</span>`:''}${x.party?`<span>· ${escapeHtml(x.party)}</span>`:''}</div>
        ${x.note?`<div class="entry-note">${escapeHtml(x.note)}</div>`:''}
      </div>`).join('');
  return `
    <div class="stat expense" style="margin-bottom:16px;"><div class="stat-label">إجمالي المصاريف</div><div class="stat-value" style="font-size:22px;">${fmt(t.totalExpenses)} دج</div></div>
    <section class="block"><div class="block-head"><h2>سجل المصاريف</h2></div>${list}</section>
  `;
}

function renderPurchases(){
  const t = totals();
  let items = STATE.purchases;
  if(searchPurchases.trim()) items = items.filter(p=> p.supplier.toLowerCase().includes(searchPurchases.trim().toLowerCase()));
  const list = items.length===0 ? `<div class="empty">ماكانش أي مشترى مطابق. اضغط + باش تسجل مشترى.</div>` :
    [...items].reverse().map(p=>{
      const amt = purchaseTotal(p);
      const remaining = remainingOf({amount:amt, payments:p.payments});
      const paid = paidOf({payments:p.payments});
      const pct = amt>0 ? Math.min(100, Math.round((paid/amt)*100)) : 0;
      const st = statusOf({amount:amt, payments:p.payments});
      return `
      <div class="debt-card by-me">
        <div class="debt-top"><div class="debt-person">${escapeHtml(p.supplier)}</div><div class="debt-remaining">${fmt(remaining)} دج</div></div>
        <div class="entry-meta" style="margin-top:4px;">
          ${p.item?`<span>${escapeHtml(p.item)}</span>`:''}
          <span>الكمية: ${p.qty||1} × ${fmt(p.unitPrice)}</span>
          ${p.discount?`<span>خصم: ${fmt(p.discount)}</span>`:''}
          ${p.method?`<span>${p.method}</span>`:''}
          ${p.dueDate?`<span>استحقاق: ${fmtDate(p.dueDate)}</span>`:''}
        </div>
        <span class="badge ${st}" style="margin-top:8px; display:inline-block;">${statusLabel(st)}</span>
        <div class="debt-bar-track"><div class="debt-bar-fill" style="width:${pct}%"></div></div>
        <div class="debt-foot"><span>الإجمالي: ${fmt(amt)} دج</span><span>مدفوع: ${fmt(paid)} دج</span></div>
        <div class="debt-actions">
          ${remaining>0?`<button class="btn-mini primary" data-action="pay-purchase" data-id="${p.id}">تسجيل دفعة</button>`:''}
          <button class="btn-mini gold" data-action="print-invoice" data-id="${p.id}">فاتورة PDF</button>
        </div>
      </div>`;
    }).join('');
  return `
    <div class="search-box"><input type="text" id="searchPurchasesInput" placeholder="ابحث باسم المورد..." value="${escapeHtml(searchPurchases)}"></div>
    <div class="stat-grid" style="margin-bottom:16px;">
      <div class="stat owed-by-me"><div class="stat-label">إجمالي المشتريات</div><div class="stat-value">${fmt(t.purchasesTotal)}</div></div>
      <div class="stat expense"><div class="stat-label">متبقي للموردين</div><div class="stat-value">${fmt(t.purchasesRemaining)}</div></div>
    </div>
    <section class="block"><div class="block-head"><h2>سجل المشتريات</h2></div>${list}</section>
  `;
}

function renderDebts(){
  const t = totals();
  let list = STATE.debts.filter(d=> debtFilter==='toMe' ? d.direction==='toMe' : d.direction==='byMe');
  if(searchDebts.trim()) list = list.filter(d=> d.person.toLowerCase().includes(searchDebts.trim().toLowerCase()));
  const cardsHtml = list.length===0 ? `<div class="empty">${debtFilter==='toMe'?'ماكانش حد سلفتو فلوس':'ماكانش حد سلفك فلوس'}</div>` :
    [...list].reverse().map(d=>{
      const remaining = remainingOf(d); const paid = paidOf(d);
      const pct = d.amount>0 ? Math.min(100, Math.round((paid/d.amount)*100)) : 0;
      const st = statusOf(d);
      return `
      <div class="debt-card ${d.direction==='toMe'?'to-me':'by-me'}">
        <div class="debt-top"><div class="debt-person">${escapeHtml(d.person)}</div><div class="debt-remaining">${fmt(remaining)} دج</div></div>
        <div class="entry-meta" style="margin-top:4px;">
          <span>الدين: ${fmtDate(d.date)}</span>${d.dueDate?`<span>· استحقاق: ${fmtDate(d.dueDate)}</span>`:''}
        </div>
        <span class="badge ${st}" style="margin-top:8px; display:inline-block;">${statusLabel(st)}</span>
        <div class="debt-bar-track"><div class="debt-bar-fill" style="width:${pct}%"></div></div>
        <div class="debt-foot"><span>الأصل: ${fmt(d.amount)} دج</span><span>مدفوع: ${fmt(paid)} دج</span></div>
        ${d.note?`<div class="entry-note">${escapeHtml(d.note)}</div>`:''}
        ${remaining>0?`<div class="debt-actions"><button class="btn-mini primary" data-action="pay-debt" data-id="${d.id}">تسجيل تسديد</button></div>`:''}
      </div>`;
    }).join('');
  return `
    <div class="stat-grid" style="margin-bottom:12px;">
      <div class="stat owed-to-me"><div class="stat-label">مجموع الديون لك</div><div class="stat-value">${fmt(t.owedToMe)}</div></div>
      <div class="stat owed-by-me"><div class="stat-label">مجموع الديون عليك</div><div class="stat-value">${fmt(t.owedByMe)}</div></div>
    </div>
    <div class="search-box"><input type="text" id="searchDebtsInput" placeholder="ابحث باسم الشخص..." value="${escapeHtml(searchDebts)}"></div>
    <div class="tabs">
      <button class="tab-btn ${debtFilter==='toMe'?'active to-me':''}" data-debtfilter="toMe">الناس اللي يسالفوك</button>
      <button class="tab-btn ${debtFilter==='byMe'?'active by-me':''}" data-debtfilter="byMe">الناس اللي تسالفهم</button>
    </div>
    ${cardsHtml}
  `;
}

function renderPayments(){
  let rows = [];
  STATE.purchases.forEach(p=>{
    const amt = purchaseTotal(p);
    rows.push({who:p.supplier, kind:'مورد (مشترى)', total:amt, paid:paidOf({payments:p.payments}), remaining:remainingOf({amount:amt,payments:p.payments}), status:statusOf({amount:amt,payments:p.payments}), method:p.method});
  });
  STATE.debts.forEach(d=>{
    rows.push({who:d.person, kind: d.direction==='toMe'?'يديك (سلفة)':'تديه (سلفة)', total:d.amount, paid:paidOf(d), remaining:remainingOf(d), status:statusOf(d), method:''});
  });
  if(searchPayments.trim()) rows = rows.filter(r=> r.who.toLowerCase().includes(searchPayments.trim().toLowerCase()));
  if(rows.length===0) return `<div class="search-box"><input type="text" id="searchPaymentsInput" placeholder="ابحث بالاسم..." value="${escapeHtml(searchPayments)}"></div><div class="empty">ما فماش نتائج.</div>`;
  return `
    <div class="search-box"><input type="text" id="searchPaymentsInput" placeholder="ابحث بالاسم..." value="${escapeHtml(searchPayments)}"></div>
    <section class="block"><div class="block-head"><h2>من دفع وشحال باقي</h2></div>
      ${rows.map(r=>`
        <div class="entry-card">
          <div class="entry-top">
            <div><div style="font-weight:800; font-size:13.5px;">${escapeHtml(r.who)}</div><span class="tag" style="margin-top:5px; display:inline-block;">${r.kind}</span></div>
            <span class="badge ${r.status}">${statusLabel(r.status)}</span>
          </div>
          <div class="entry-meta" style="margin-top:8px;"><span>الإجمالي: ${fmt(r.total)} دج</span><span>مدفوع: ${fmt(r.paid)} دج</span><span>متبقي: ${fmt(r.remaining)} دج</span></div>
        </div>
      `).join('')}
    </section>
  `;
}

function renderUnpaid(){
  let rows = [];
  STATE.purchases.forEach(p=>{ const amt=purchaseTotal(p); const rem=remainingOf({amount:amt,payments:p.payments}); if(rem>0) rows.push({who:p.supplier, kind:'مورد', remaining:rem, due:p.dueDate}); });
  STATE.debts.forEach(d=>{ const rem=remainingOf(d); if(rem>0) rows.push({who:d.person, kind: d.direction==='toMe'?'يدين لك':'تدين له', remaining:rem, due:d.dueDate}); });
  if(searchUnpaid.trim()) rows = rows.filter(r=> r.who.toLowerCase().includes(searchUnpaid.trim().toLowerCase()));
  const box = `<div class="search-box"><input type="text" id="searchUnpaidInput" placeholder="ابحث بالاسم..." value="${escapeHtml(searchUnpaid)}"></div>`;
  if(rows.length===0) return box + `<div class="empty">ما فماش حد باقيلو مبلغ 🎉</div>`;
  return box + `
    <section class="block"><div class="block-head"><h2>الأشخاص اللي ما خلصوش بعد</h2></div>
      ${rows.map(r=>`
        <div class="entry-card">
          <div class="entry-top">
            <div><div style="font-weight:800; font-size:13.5px;">${escapeHtml(r.who)}</div><span class="tag" style="margin-top:5px; display:inline-block;">${r.kind}</span></div>
            <div class="entry-amount" style="color:var(--expense);">${fmt(r.remaining)} دج</div>
          </div>
          ${r.due?`<div class="entry-meta"><span>تاريخ الاستحقاق: ${fmtDate(r.due)}</span></div>`:''}
        </div>
      `).join('')}
    </section>
  `;
}

function renderFinal(){
  const t = totals();
  return `
    <section class="block">
      <div class="entry-card" style="padding:16px;">
        <div class="section-title">رأس المال</div>
        <div class="final-row"><span class="lbl">رأس المال الأولي</span><span class="val">${fmt(t.initialCapital)}</span></div>
        <div class="final-row plus"><span class="lbl">إضافات</span><span class="val">+ ${fmt(t.additions)}</span></div>
        <div class="final-row minus"><span class="lbl">سحوبات</span><span class="val">− ${fmt(t.withdrawals)}</span></div>
        <div class="final-row"><span class="lbl">رأس المال الحالي</span><span class="val">${fmt(t.totalCapital)}</span></div>

        <div class="section-title">الحركة المالية</div>
        <div class="final-row plus"><span class="lbl">إجمالي المداخيل</span><span class="val">+ ${fmt(t.totalIncome)}</span></div>
        <div class="final-row minus"><span class="lbl">إجمالي المصاريف</span><span class="val">− ${fmt(t.totalExpenses)}</span></div>
        <div class="final-row minus"><span class="lbl">مشتريات مدفوعة</span><span class="val">− ${fmt(t.purchasesPaid)}</span></div>
        <div class="final-row total"><span class="lbl">الرصيد النقدي الحالي (فعلياً)</span><span class="val">${fmt(t.cashOnHand)} دج</span></div>

        <div class="section-title" style="margin-top:18px;">الديون والمشتريات غير المسددة</div>
        <div class="final-row plus"><span class="lbl">ديون لك (عند الناس)</span><span class="val">+ ${fmt(t.owedToMe)}</span></div>
        <div class="final-row minus"><span class="lbl">ديون عليك (لغيرك)</span><span class="val">− ${fmt(t.owedByMe)}</span></div>
        <div class="final-row minus"><span class="lbl">مشتريات متبقية (للموردين)</span><span class="val">− ${fmt(t.purchasesRemaining)}</span></div>
        <div class="final-row"><span class="lbl">إجمالي غير المدفوع (عليك)</span><span class="val">${fmt(t.unpaidByMe)}</span></div>

        <div class="final-row total"><span class="lbl">الوضع المالي النهائي (صافي)</span><span class="val">${fmt(t.net)} دج</span></div>
      </div>
      <div class="empty" style="margin-top:12px; text-align:right; border-style:solid;">
        كل دين أو مشترى يُحسب بمبلغه <b>المتبقي فقط</b> (بعد خصم كل المدفوعات)، حتى ما يتحتسبش مرتين. الرصيد النقدي يعكس الفلوس الموجودة فعلياً، والوضع النهائي يزيد عليه الديون والالتزامات.
      </div>
    </section>
  `;
}

function renderMore(){
  return `
    <div class="menu-grid">
      <button class="menu-btn" data-nav="income"><div class="t">المداخيل</div><div class="s">سجل كل الدخل</div></button>
      <button class="menu-btn" data-nav="expenses"><div class="t">المصاريف</div><div class="s">سجل كل الخرج</div></button>
      <button class="menu-btn" data-nav="payments"><div class="t">المدفوعات</div><div class="s">شكون خلص وشحال باقي</div></button>
      <button class="menu-btn" data-nav="unpaid"><div class="t">غير المسددين</div><div class="s">اللي ما خلصوش بعد</div></button>
      <button class="menu-btn" data-nav="final" style="grid-column:1/3;"><div class="t">الحساب النهائي</div><div class="s">كل الحسابات مجمعة تلقائياً</div></button>
    </div>
  `;
}

/* ================= Sheets ================= */
function openSheet(html){
  document.getElementById('sheetHost').innerHTML = `<div class="sheet-backdrop" id="sheetBackdrop"><div class="sheet" onclick="event.stopPropagation()"><div class="sheet-handle"></div>${html}</div></div>`;
  document.getElementById('sheetBackdrop').addEventListener('click', closeSheet);
}
function closeSheet(){ document.getElementById('sheetHost').innerHTML=''; }

function sheetCapital(){
  openSheet(`
    <h3>عملية على رأس المال</h3>
    <div class="field"><label>النوع</label><div class="radio-row">
      <div class="radio-opt selected" data-kind="initial">رأس مال أولي</div>
      <div class="radio-opt" data-kind="addition">إضافة</div>
      <div class="radio-opt" data-kind="withdrawal">سحب</div>
    </div></div>
    <div class="field"><label>المبلغ (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
    <div class="field"><label>السبب / المصدر</label><input type="text" id="f-reason" placeholder="مثلاً: توفير شخصي، سحب للمصاريف الشخصية"></div>
    <div class="field"><label>ملاحظات</label><textarea id="f-note" placeholder="اختياري"></textarea></div>
    <div class="err" id="f-err">دخل مبلغ صحيح</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="saveCapital">حفظ</button></div>
  `);
  let kind='initial';
  document.querySelectorAll('.radio-opt').forEach(el=> el.addEventListener('click', ()=>{ document.querySelectorAll('.radio-opt').forEach(o=>o.classList.remove('selected')); el.classList.add('selected'); kind=el.dataset.kind; }));
  document.getElementById('saveCapital').addEventListener('click', async ()=>{
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!amount || amount<=0){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const reason = document.getElementById('f-reason').value.trim();
    const note = document.getElementById('f-note').value.trim();
    STATE.capital.push({id:uid(), kind, amount, date, reason, note});
    const label = kind==='initial'?'رأس مال أولي':kind==='addition'?('إضافة رأس مال'+(reason?' — '+reason:'')):('سحب من رأس المال'+(reason?' — '+reason:''));
    logTx('capital', label, amount, kind==='withdrawal'?-1:+1);
    await saveState(); closeSheet(); render();
  });
}

function sheetIncome(){
  openSheet(`
    <h3>تسجيل دخل</h3>
    <div class="field"><label>المبلغ (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>طريقة الدفع</label><select id="f-method">${optionsHtml(PAY_METHODS)}</select></div>
    </div>
    <div class="field"><label>مصدر الدخل</label><input type="text" id="f-source" placeholder="مثلاً: بيع، خدمة"></div>
    <div class="field"><label>الشخص / الجهة</label><input type="text" id="f-party" placeholder="اختياري"></div>
    <div class="field"><label>ملاحظات</label><textarea id="f-note" placeholder="اختياري"></textarea></div>
    <div class="err" id="f-err">دخل مبلغ صحيح</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="saveIncome">حفظ</button></div>
  `);
  document.getElementById('saveIncome').addEventListener('click', async ()=>{
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!amount || amount<=0){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const method = document.getElementById('f-method').value;
    const source = document.getElementById('f-source').value.trim();
    const party = document.getElementById('f-party').value.trim();
    const note = document.getElementById('f-note').value.trim();
    STATE.income.push({id:uid(), amount, date, method, source, party, note});
    logTx('income', source?('دخل — '+source):'دخل جديد', amount, +1);
    await saveState(); closeSheet(); render();
  });
}

function sheetExpense(){
  openSheet(`
    <h3>تسجيل مصروف</h3>
    <div class="field"><label>المبلغ (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>طريقة الدفع</label><select id="f-method">${optionsHtml(PAY_METHODS)}</select></div>
    </div>
    <div class="field"><label>نوع المصروف</label><input type="text" id="f-category" placeholder="مثلاً: كراء، نقل"></div>
    <div class="field"><label>الشخص / الجهة</label><input type="text" id="f-party" placeholder="اختياري"></div>
    <div class="field"><label>ملاحظات</label><textarea id="f-note" placeholder="اختياري"></textarea></div>
    <div class="err" id="f-err">دخل مبلغ صحيح</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="saveExpense">حفظ</button></div>
  `);
  document.getElementById('saveExpense').addEventListener('click', async ()=>{
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!amount || amount<=0){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const method = document.getElementById('f-method').value;
    const category = document.getElementById('f-category').value.trim();
    const party = document.getElementById('f-party').value.trim();
    const note = document.getElementById('f-note').value.trim();
    STATE.expenses.push({id:uid(), amount, date, method, category, party, note});
    logTx('expense', category?('مصروف — '+category):'مصروف جديد', amount, -1);
    await saveState(); closeSheet(); render();
  });
}

function sheetPurchase(){
  openSheet(`
    <h3>تسجيل مشترى جديد</h3>
    <div class="field"><label>اسم المورد / المحل</label><input type="text" id="f-supplier" placeholder="اسم المورد"></div>
    <div class="field"><label>وصف البضاعة</label><input type="text" id="f-item" placeholder="مثلاً: بضاعة، مواد أولية"></div>
    <div class="field-row">
      <div class="field"><label>الكمية</label><input type="number" id="f-qty" value="1" inputmode="decimal"></div>
      <div class="field"><label>سعر الوحدة (دج)</label><input type="number" id="f-unitprice" inputmode="decimal" placeholder="0"></div>
    </div>
    <div class="field"><label>التخفيض (دج، اختياري)</label><input type="number" id="f-discount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>تاريخ الاستحقاق</label><input type="date" id="f-duedate"></div>
    </div>
    <div class="field"><label>طريقة الدفع</label><select id="f-method">${optionsHtml(PAY_METHODS)}</select></div>
    <div class="field"><label>المبلغ المدفوع الآن (دج)</label><input type="number" id="f-paidnow" inputmode="decimal" placeholder="0 إذا ما دفعتش والو"></div>
    <div class="field"><label>ملاحظات</label><textarea id="f-note" placeholder="اختياري"></textarea></div>
    <div class="err" id="f-err">دخل اسم المورد وسعر الوحدة</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="savePurchase">حفظ</button></div>
  `);
  document.getElementById('savePurchase').addEventListener('click', async ()=>{
    const supplier = document.getElementById('f-supplier').value.trim();
    const item = document.getElementById('f-item').value.trim();
    const qty = parseFloat(document.getElementById('f-qty').value) || 1;
    const unitPrice = parseFloat(document.getElementById('f-unitprice').value);
    const discount = parseFloat(document.getElementById('f-discount').value) || 0;
    const method = document.getElementById('f-method').value;
    const date = document.getElementById('f-date').value || todayStr();
    const dueDate = document.getElementById('f-duedate').value || '';
    const note = document.getElementById('f-note').value.trim();
    let paidNow = parseFloat(document.getElementById('f-paidnow').value) || 0;
    if(!supplier || !unitPrice || unitPrice<=0){ document.getElementById('f-err').style.display='block'; return; }
    const total = Math.max(qty*unitPrice - discount, 0);
    if(paidNow > total) paidNow = total;
    const payments = paidNow>0 ? [{id:uid(), amount:paidNow, date, note:'دفعة عند الشراء'}] : [];
    STATE.purchases.push({id:uid(), supplier, item, qty, unitPrice, discount, method, date, dueDate, note, payments});
    logTx('purchaseNew', 'مشترى من '+supplier, total, -1);
    if(paidNow>0) logTx('purchasePay', 'دفعة لـ '+supplier, paidNow, -1);
    await saveState(); closeSheet(); render();
  });
}

function sheetPayPurchase(purchaseId){
  const p = STATE.purchases.find(x=>x.id===purchaseId); if(!p) return;
  const amt = purchaseTotal(p);
  const remaining = remainingOf({amount:amt, payments:p.payments});
  openSheet(`
    <h3>تسجيل دفعة — ${escapeHtml(p.supplier)}</h3>
    <div class="field"><label>المبلغ المتبقي حالياً</label><input type="text" value="${fmt(remaining)} دج" disabled></div>
    <div class="field"><label>مبلغ الدفعة (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>طريقة الدفع</label><select id="f-method">${optionsHtml(PAY_METHODS)}</select></div>
    </div>
    <div class="err" id="f-err">دخل مبلغ صحيح لا يتجاوز المتبقي</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="savePayP">حفظ</button></div>
  `);
  document.getElementById('savePayP').addEventListener('click', async ()=>{
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!amount || amount<=0 || amount>remaining+0.0001){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const method = document.getElementById('f-method').value;
    p.payments.push({id:uid(), amount, date, method});
    logTx('purchasePay', 'دفعة لـ '+p.supplier, amount, -1);
    await saveState(); closeSheet(); render();
  });
}

function sheetDebt(){
  openSheet(`
    <h3>تسجيل سلفة / دين جديد</h3>
    <div class="field"><label>النوع</label><div class="radio-row">
      <div class="radio-opt selected" data-dir="toMe">سلفته لحد (هو يديني)</div>
      <div class="radio-opt" data-dir="byMe">سلفوني (أنا نديه)</div>
    </div></div>
    <div class="field"><label>اسم الشخص</label><input type="text" id="f-person" placeholder="اسم الشخص"></div>
    <div class="field"><label>المبلغ (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>تاريخ الدين</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>تاريخ الاستحقاق</label><input type="date" id="f-duedate"></div>
    </div>
    <div class="field"><label>ملاحظات</label><textarea id="f-note" placeholder="اختياري"></textarea></div>
    <div class="err" id="f-err">دخل الاسم والمبلغ</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="saveDebt">حفظ</button></div>
  `);
  let direction='toMe';
  document.querySelectorAll('.radio-opt').forEach(el=> el.addEventListener('click', ()=>{ document.querySelectorAll('.radio-opt').forEach(o=>o.classList.remove('selected')); el.classList.add('selected'); direction=el.dataset.dir; }));
  document.getElementById('saveDebt').addEventListener('click', async ()=>{
    const person = document.getElementById('f-person').value.trim();
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!person || !amount || amount<=0){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const dueDate = document.getElementById('f-duedate').value || '';
    const note = document.getElementById('f-note').value.trim();
    STATE.debts.push({id:uid(), direction, person, amount, date, dueDate, note, payments:[]});
    logTx(direction==='toMe'?'debtNewToMe':'debtNewByMe', 'سلفة '+(direction==='toMe'?'لـ ':'من ')+person, amount, direction==='toMe'?+1:-1);
    await saveState(); closeSheet(); currentTab='debts'; debtFilter=direction; render();
  });
}

function sheetPayDebt(debtId){
  const debt = STATE.debts.find(d=>d.id===debtId); if(!debt) return;
  const remaining = remainingOf(debt);
  openSheet(`
    <h3>تسجيل تسديد — ${escapeHtml(debt.person)}</h3>
    <div class="field"><label>المبلغ المتبقي حالياً</label><input type="text" value="${fmt(remaining)} دج" disabled></div>
    <div class="field"><label>مبلغ التسديد (دج)</label><input type="number" id="f-amount" inputmode="decimal" placeholder="0"></div>
    <div class="field-row">
      <div class="field"><label>التاريخ</label><input type="date" id="f-date" value="${todayStr()}"></div>
      <div class="field"><label>طريقة الدفع</label><select id="f-method">${optionsHtml(PAY_METHODS)}</select></div>
    </div>
    <div class="err" id="f-err">دخل مبلغ صحيح لا يتجاوز المتبقي</div>
    <div class="sheet-actions"><button class="btn cancel" data-close>إلغاء</button><button class="btn save" id="savePayment">حفظ</button></div>
  `);
  document.getElementById('savePayment').addEventListener('click', async ()=>{
    const amount = parseFloat(document.getElementById('f-amount').value);
    if(!amount || amount<=0 || amount>remaining+0.0001){ document.getElementById('f-err').style.display='block'; return; }
    const date = document.getElementById('f-date').value || todayStr();
    const method = document.getElementById('f-method').value;
    debt.payments.push({id:uid(), amount, date, method});
    logTx(debt.direction==='toMe'?'debtPayToMe':'debtPayByMe', 'تسديد من/لـ '+debt.person, amount, debt.direction==='toMe'?+1:-1);
    await saveState(); closeSheet(); render();
  });
}

/* ================= Invoice PDF (print) ================= */
function printInvoice(purchaseId){
  const p = STATE.purchases.find(x=>x.id===purchaseId); if(!p) return;
  const amt = purchaseTotal(p);
  const remaining = remainingOf({amount:amt, payments:p.payments});
  const paid = paidOf({payments:p.payments});
  document.getElementById('printInvoice').innerHTML = `
    <div class="inv-head">
      <div><h1>فاتورة مشترى</h1><p>المحاسب الذكي — Smart Accountant</p><p>صاحب المشروع: شراحي محمد البشير</p></div>
      <div class="inv-meta"><div>رقم الفاتورة: ${p.id}</div><div>${fmtDate(p.date)}</div></div>
    </div>
    <table class="inv-table">
      <tr><th>المورد</th><td>${escapeHtml(p.supplier)}</td></tr>
      <tr><th>البضاعة</th><td>${escapeHtml(p.item||'—')}</td></tr>
      <tr><th>الكمية</th><td>${p.qty||1}</td></tr>
      <tr><th>سعر الوحدة</th><td>${fmt(p.unitPrice)} دج</td></tr>
      ${p.discount?`<tr><th>التخفيض</th><td>${fmt(p.discount)} دج</td></tr>`:''}
      ${p.method?`<tr><th>طريقة الدفع</th><td>${p.method}</td></tr>`:''}
      ${p.note?`<tr><th>ملاحظات</th><td>${escapeHtml(p.note)}</td></tr>`:''}
    </table>
    <div class="inv-summary">
      <div class="r"><span>المبلغ الإجمالي</span><span>${fmt(amt)} دج</span></div>
      <div class="r"><span>المبلغ المدفوع</span><span>${fmt(paid)} دج</span></div>
      <div class="r total"><span>المبلغ المتبقي</span><span>${fmt(remaining)} دج</span></div>
    </div>
    <div class="inv-note">تم إنشاء هذه الفاتورة تلقائياً عبر تطبيق المحاسب الذكي. لحفظها كـ PDF اختر "حفظ كـ PDF" من نافذة الطباعة.</div>
  `;
  document.body.classList.add('printing');
  window.print();
}
window.addEventListener('afterprint', ()=> document.body.classList.remove('printing'));

/* ================= Wiring ================= */
function attachDynamicHandlers(){
  document.querySelectorAll('[data-debtfilter]').forEach(b=> b.addEventListener('click', ()=>{ debtFilter=b.dataset.debtfilter; render(); }));
  document.querySelectorAll('[data-action="pay-debt"]').forEach(b=> b.addEventListener('click', ()=> sheetPayDebt(b.dataset.id)));
  document.querySelectorAll('[data-action="pay-purchase"]').forEach(b=> b.addEventListener('click', ()=> sheetPayPurchase(b.dataset.id)));
  document.querySelectorAll('[data-action="print-invoice"]').forEach(b=> b.addEventListener('click', ()=> printInvoice(b.dataset.id)));
  document.querySelectorAll('[data-nav]').forEach(b=> b.addEventListener('click', ()=>{ currentTab=b.dataset.nav; render(); }));
  const sp=document.getElementById('searchPurchasesInput'); if(sp) sp.addEventListener('input', e=>{ searchPurchases=e.target.value; render(); setTimeout(()=>{const el=document.getElementById('searchPurchasesInput'); if(el){el.focus(); el.selectionStart=el.selectionEnd=el.value.length;}},0); });
  const sd=document.getElementById('searchDebtsInput'); if(sd) sd.addEventListener('input', e=>{ searchDebts=e.target.value; render(); setTimeout(()=>{const el=document.getElementById('searchDebtsInput'); if(el){el.focus(); el.selectionStart=el.selectionEnd=el.value.length;}},0); });
  const sy=document.getElementById('searchPaymentsInput'); if(sy) sy.addEventListener('input', e=>{ searchPayments=e.target.value; render(); setTimeout(()=>{const el=document.getElementById('searchPaymentsInput'); if(el){el.focus(); el.selectionStart=el.selectionEnd=el.value.length;}},0); });
  const su=document.getElementById('searchUnpaidInput'); if(su) su.addEventListener('input', e=>{ searchUnpaid=e.target.value; render(); setTimeout(()=>{const el=document.getElementById('searchUnpaidInput'); if(el){el.focus(); el.selectionStart=el.selectionEnd=el.value.length;}},0); });
}

document.getElementById('sheetHost').addEventListener('click', e=>{ if(e.target && e.target.hasAttribute('data-close')) closeSheet(); });
document.querySelectorAll('.navitem').forEach(b=> b.addEventListener('click', ()=>{ currentTab=b.dataset.tab; render(); }));
document.getElementById('fabBtn').addEventListener('click', ()=>{
  if(currentTab==='capital') sheetCapital();
  else if(currentTab==='income') sheetIncome();
  else if(currentTab==='expenses') sheetExpense();
  else if(currentTab==='purchases') sheetPurchase();
  else if(currentTab==='debts') sheetDebt();
  else sheetCapital();
});

(async function init(){ await loadState(); render(); })();
</script>
</body>
</html>
