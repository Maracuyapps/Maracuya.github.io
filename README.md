<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MiNegocio POS</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0f1117;--bg2:#181c28;--bg3:#1e2335;--bg4:#252a3d;
  --card:#1e2335;--border:#2e3452;--border2:#3a4166;
  --text:#ffffff;--text2:#8b91b0;--text3:#5c6285;
  --accent:#6c63ff;--accent2:#8b83ff;--accentbg:rgba(108,99,255,0.15);
  --green:#00c48c;--greenbg:rgba(0,196,140,0.13);
  --red:#ff5c7a;--redbg:rgba(255,92,122,0.13);
  --yellow:#ffb547;--yellowbg:rgba(255,181,71,0.13);
  --blue:#4da6ff;--bluebg:rgba(77,166,255,0.13);
  --orange:#ff7a3d;--orangebg:rgba(255,122,61,0.13);
  --r:14px;--r2:10px;--r3:8px;
}
html,body{height:100%;font-family:'Plus Jakarta Sans',sans-serif;background:var(--bg);color:var(--text);font-size:14px}
#root{min-height:100vh}

/* LOGIN */
.login-wrap{display:flex;align-items:center;justify-content:center;min-height:100vh;padding:2rem}
.login-box{background:var(--bg2);border:1px solid var(--border);border-radius:20px;padding:2.5rem;width:100%;max-width:440px}
.login-logo{display:flex;align-items:center;gap:10px;margin-bottom:2rem}
.logo-icon{width:44px;height:44px;background:var(--accent);border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:22px}
.logo-text{font-size:24px;font-weight:700;letter-spacing:-.5px}
.login-title{font-size:18px;font-weight:600;margin-bottom:6px}
.login-sub{font-size:13px;color:var(--text2);margin-bottom:2rem}
.user-list{display:flex;flex-direction:column;gap:10px;margin-bottom:1.5rem}
.user-btn{background:var(--bg3);border:1px solid var(--border);border-radius:var(--r2);padding:14px 16px;cursor:pointer;display:flex;align-items:center;gap:12px;transition:all .15s;text-align:left;width:100%}
.user-btn:hover,.user-btn.selected{border-color:var(--accent);background:var(--accentbg)}
.user-avatar{width:40px;height:40px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;flex-shrink:0}
.user-name{font-size:14px;font-weight:600;color:var(--text)}
.user-role{font-size:12px;color:var(--text2);margin-top:2px}
.pin-area{margin-bottom:1.5rem}
.pin-label{font-size:12px;color:var(--text2);margin-bottom:8px}
.pin-input{background:var(--bg3);border:1px solid var(--border);border-radius:var(--r3);padding:12px 16px;font-size:20px;color:var(--text);width:100%;outline:none;font-family:monospace;letter-spacing:8px;text-align:center}
.pin-input:focus{border-color:var(--accent)}
.btn-login{width:100%;padding:14px;background:var(--accent);color:#fff;border:none;border-radius:var(--r3);font-size:15px;font-weight:600;cursor:pointer;font-family:inherit;transition:all .15s}
.btn-login:hover{background:var(--accent2)}
.login-err{color:var(--red);font-size:12px;margin-top:8px;text-align:center}

/* LAYOUT */
.app-layout{display:flex;height:100vh;overflow:hidden}
.sidebar{width:225px;background:var(--bg2);border-right:1px solid var(--border);display:flex;flex-direction:column;padding:1.25rem 0;flex-shrink:0;overflow-y:auto}
.sb-header{padding:0 1.25rem 1.25rem;border-bottom:1px solid var(--border);margin-bottom:.75rem}
.sb-logo{display:flex;align-items:center;gap:8px}
.sb-logo-icon{width:34px;height:34px;background:var(--accent);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:16px}
.sb-brand{font-size:17px;font-weight:700;letter-spacing:-.3px}
.sb-user{margin-top:10px;display:flex;align-items:center;gap:8px}
.sb-avatar{width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700}
.sb-uname{font-size:12px;font-weight:500;color:var(--text2)}
.nav-section{padding:0 .75rem}
.nav-section-label{font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;padding:0 .5rem;margin-bottom:4px;margin-top:14px}
.nav-item{display:flex;align-items:center;gap:9px;padding:10px 12px;border-radius:var(--r3);cursor:pointer;font-size:13px;font-weight:500;color:var(--text2);transition:all .12s;margin-bottom:2px}
.nav-item:hover{background:var(--bg3);color:var(--text)}
.nav-item.active{background:var(--accentbg);color:var(--accent)}
.nav-icon{font-size:13px;width:20px;text-align:center;font-family:monospace}
.sb-footer{margin-top:auto;padding:0 .75rem;padding-top:1rem}

/* CONTENT */
.main-content{flex:1;overflow-y:auto;padding:1.75rem 2rem;background:var(--bg);position:relative}
.page-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:1.5rem}
.page-title{font-size:22px;font-weight:700;letter-spacing:-.3px}
.page-sub{font-size:13px;color:var(--text2);margin-top:3px}
.header-actions{display:flex;gap:8px;align-items:center}

/* BUTTONS */
.btn{padding:9px 18px;border-radius:var(--r3);border:1px solid var(--border2);background:var(--bg3);cursor:pointer;font-size:13px;font-weight:500;color:var(--text);transition:all .12s;font-family:inherit}
.btn:hover{background:var(--bg4);border-color:var(--accent)}
.btn-accent{background:var(--accent);color:#fff;border-color:var(--accent)}
.btn-accent:hover{background:var(--accent2);border-color:var(--accent2)}
.btn-green{background:var(--greenbg);color:var(--green);border-color:transparent}
.btn-red{background:var(--redbg);color:var(--red);border-color:transparent}
.btn-yellow{background:var(--yellowbg);color:var(--yellow);border-color:transparent}
.btn-sm{padding:6px 12px;font-size:12px}
.btn-xs{padding:4px 9px;font-size:11px}

/* METRICS */
.metrics{display:grid;grid-template-columns:repeat(auto-fit,minmax(155px,1fr));gap:12px;margin-bottom:1.5rem}
.metric-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem;transition:border-color .15s}
.metric-card:hover{border-color:var(--border2)}
.metric-icon{width:38px;height:38px;border-radius:11px;display:flex;align-items:center;justify-content:center;font-size:17px;margin-bottom:10px}
.metric-val{font-size:21px;font-weight:700;letter-spacing:-.5px;line-height:1}
.metric-lbl{font-size:12px;color:var(--text2);margin-top:5px}
.metric-trend{font-size:11px;margin-top:4px;font-weight:500}

/* CARD */
.card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem;margin-bottom:1rem}
.card-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:1rem}
.card-title{font-size:14px;font-weight:600}

/* TABLE */
.tbl-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
th{text-align:left;padding:10px 12px;font-size:11px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.06em;border-bottom:1px solid var(--border)}
td{padding:12px 12px;border-bottom:1px solid var(--border);color:var(--text)}
tr:last-child td{border-bottom:none}
tr:hover td{background:rgba(255,255,255,.02)}

/* BADGES */
.badge{display:inline-flex;align-items:center;padding:3px 10px;border-radius:99px;font-size:11px;font-weight:600}
.badge-green{background:var(--greenbg);color:var(--green)}
.badge-red{background:var(--redbg);color:var(--red)}
.badge-yellow{background:var(--yellowbg);color:var(--yellow)}
.badge-blue{background:var(--bluebg);color:var(--blue)}
.badge-purple{background:var(--accentbg);color:var(--accent)}
.badge-orange{background:var(--orangebg);color:var(--orange)}

/* TABS */
.tabs{display:flex;gap:4px;background:var(--bg2);border-radius:var(--r3);padding:4px;margin-bottom:1.25rem;width:fit-content}
.tab{padding:7px 18px;border-radius:6px;font-size:13px;font-weight:500;cursor:pointer;color:var(--text2);transition:all .12s}
.tab.active{background:var(--accent);color:#fff}

/* FORMS */
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}
.form-grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:12px}
.form-group{display:flex;flex-direction:column;gap:5px}
.form-label{font-size:12px;font-weight:500;color:var(--text2)}
.form-input{background:var(--bg3);border:1px solid var(--border);border-radius:var(--r3);padding:10px 13px;font-size:13px;color:var(--text);outline:none;font-family:inherit;transition:border .12s;width:100%}
.form-input:focus{border-color:var(--accent)}
.form-actions{display:flex;gap:8px;margin-top:8px}

/* CHART */
.chart-wrap{height:165px;display:flex;align-items:flex-end;gap:6px;margin-top:1rem}
.bar-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:5px;height:100%;justify-content:flex-end}
.bar-rect{width:100%;border-radius:5px 5px 0 0;min-height:4px;transition:height .4s}
.bar-lbl{font-size:10px;color:var(--text3)}
.bar-num{font-size:10px;font-weight:600;color:var(--text2)}

/* ORDER */
.menu-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(120px,1fr));gap:8px}
.menu-item{background:var(--bg3);border:1px solid var(--border);border-radius:var(--r2);padding:12px 8px;cursor:pointer;text-align:center;transition:all .12s}
.menu-item:hover{border-color:var(--accent);background:var(--accentbg);transform:translateY(-1px)}
.menu-item-name{font-size:12px;font-weight:600;margin-bottom:3px}
.menu-item-price{font-size:11px;color:var(--green)}
.order-item{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border)}
.qty-ctrl{display:flex;align-items:center;gap:8px}
.qty-btn{width:28px;height:28px;border-radius:50%;background:var(--bg4);border:1px solid var(--border);color:var(--text);cursor:pointer;font-size:17px;display:flex;align-items:center;justify-content:center;line-height:1}
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:1rem}

/* ALERTS */
.alert{padding:11px 15px;border-radius:var(--r3);font-size:13px;margin-bottom:1rem;display:flex;align-items:center;gap:8px}
.alert-warn{background:var(--yellowbg);color:var(--yellow);border:1px solid rgba(255,181,71,.25)}
.alert-success{background:var(--greenbg);color:var(--green);border:1px solid rgba(0,196,140,.25)}

/* TOAST */
.toast-container{position:fixed;top:20px;right:20px;z-index:9999;display:flex;flex-direction:column;gap:8px;pointer-events:none}
.toast{background:var(--bg2);border:1px solid var(--border2);border-radius:13px;padding:13px 16px;min-width:270px;max-width:340px;pointer-events:all;display:flex;gap:10px;align-items:flex-start;animation:slideIn .25s ease;box-shadow:0 4px 20px rgba(0,0,0,.4)}
.toast-warn{border-left:3px solid var(--yellow)}
.toast-success{border-left:3px solid var(--green)}
.toast-info{border-left:3px solid var(--accent)}
.toast-icon{font-size:18px;flex-shrink:0}
.toast-body{flex:1}
.toast-title{font-size:13px;font-weight:600;margin-bottom:2px}
.toast-msg{font-size:12px;color:var(--text2)}
.toast-close{font-size:16px;cursor:pointer;background:none;border:none;color:var(--text2);font-family:inherit;padding:0}
@keyframes slideIn{from{transform:translateX(110%);opacity:0}to{transform:none;opacity:1}}

/* MODAL */
.modal-backdrop{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.7);z-index:1000;display:flex;align-items:center;justify-content:center;padding:1rem}
.modal{background:var(--bg2);border:1px solid var(--border2);border-radius:20px;padding:2rem;width:100%;max-width:480px;max-height:90vh;overflow-y:auto;box-shadow:0 8px 40px rgba(0,0,0,.5)}
.modal-title{font-size:17px;font-weight:700;margin-bottom:1.25rem}

/* PRINT */
@media print{
  body *{visibility:hidden!important}
  #print-area,#print-area *{visibility:visible!important}
  #print-area{position:fixed;left:0;top:0;width:80mm;font-family:'Courier New',monospace;font-size:10pt;color:#000;background:#fff}
  @page{size:80mm auto;margin:2mm 3mm}
}
::-webkit-scrollbar{width:5px;height:5px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--border2);border-radius:99px}
</style>
</head>
<body>
<div id="root"></div>
<div id="toast-container" class="toast-container"></div>
<div id="print-area" style="display:none"></div>

<script>
// ===================== DATOS =====================
let USERS=[
  {id:1,name:"Carlos Ruiz",role:"admin",pin:"1234",color:"#6c63ff",initials:"CR",perms:["dashboard","nueva_orden","ordenes","ventas","gastos","inventario","productos","balance","usuarios","config"]},
  {id:2,name:"María López",role:"mesero",pin:"2222",color:"#00c48c",initials:"ML",perms:["nueva_orden","ordenes"]},
  {id:3,name:"Juan Pérez",role:"mesero",pin:"3333",color:"#4da6ff",initials:"JP",perms:["nueva_orden","ordenes"]},
  {id:4,name:"Ana Torres",role:"caja",pin:"4444",color:"#ffb547",initials:"AT",perms:["ordenes","ventas","balance"]},
];
const DB={
  negocio:{nombre:"Restaurante El Sabor",nit:"900.123.456-7",dir:"Cra 5 #10-23, Santa Marta",tel:"300 123 4567",ciudad:"Santa Marta, Magdalena"},
  products:[
    {id:1,name:"Bandeja paisa",cat:"Platos",price:28000,cost:12000,stock:50,minStock:5,emoji:"🍱"},
    {id:2,name:"Ajiaco santafereño",cat:"Sopas",price:22000,cost:9000,stock:30,minStock:5,emoji:"🍲"},
    {id:3,name:"Arepa con queso",cat:"Desayunos",price:8000,cost:2500,stock:4,minStock:5,emoji:"🫓"},
    {id:4,name:"Jugo de lulo",cat:"Bebidas",price:7000,cost:2000,stock:60,minStock:10,emoji:"🧃"},
    {id:5,name:"Café tinto",cat:"Bebidas",price:3000,cost:800,stock:3,minStock:20,emoji:"☕"},
    {id:6,name:"Sancocho de gallina",cat:"Sopas",price:25000,cost:11000,stock:20,minStock:5,emoji:"🍗"},
  ],
  sales:[
    {id:1,date:hoy(),items:[{id:1,name:"Bandeja paisa",qty:2,price:28000},{id:4,name:"Jugo de lulo",qty:2,price:7000}],total:70000,waiter:"María López",table:"Mesa 3",status:"pagado"},
    {id:2,date:hoy(),items:[{id:3,name:"Arepa con queso",qty:3,price:8000},{id:5,name:"Café tinto",qty:3,price:3000}],total:33000,waiter:"Juan Pérez",table:"Mesa 1",status:"pagado"},
  ],
  expenses:[
    {id:1,date:hoy(),concept:"Ingredientes carne",amount:85000,cat:"Insumos",by:"Carlos Ruiz"},
    {id:2,date:hoy(),concept:"Gas natural",amount:45000,cat:"Servicios",by:"Carlos Ruiz"},
  ],
  orders:[
    {id:101,table:"Mesa 2",waiter:"Juan Pérez",items:[{id:1,name:"Bandeja paisa",qty:1,price:28000}],total:28000,status:"en_cocina",time:"12:34"},
    {id:102,table:"Mesa 4",waiter:"María López",items:[{id:5,name:"Café tinto",qty:2,price:3000},{id:3,name:"Arepa con queso",qty:2,price:8000}],total:22000,status:"listo",time:"12:41"},
  ],
  nxt:{prod:7,sale:3,exp:3,order:103}
};

function hoy(){const d=new Date();return d.toISOString().split("T")[0];}
const fmt=n=>new Intl.NumberFormat("es-CO",{style:"currency",currency:"COP",minimumFractionDigits:0}).format(n);
const fmtk=n=>n>=1000000?`${(n/1000000).toFixed(1)}M`:n>=1000?`${Math.round(n/1000)}k`:String(n);

// ===================== STATE =====================
let S={user:null,page:"dashboard",period:"dia",order:{table:"",items:[]},modal:null,selUser:null,pinInput:"",pinErr:""};

// ===================== TOASTS =====================
let toastId=0;
function toast(title,msg,type="info",duration=5000){
  const id=++toastId;
  const icons={info:"🔔",warn:"⚠️",success:"✅"};
  const c=document.getElementById("toast-container");
  const el=document.createElement("div");
  el.className=`toast toast-${type}`;el.id=`toast-${id}`;
  el.innerHTML=`<div class="toast-icon">${icons[type]}</div><div class="toast-body"><div class="toast-title">${title}</div><div class="toast-msg">${msg}</div></div><button class="toast-close" onclick="closeToast(${id})">✕</button>`;
  c.appendChild(el);
  if(duration>0)setTimeout(()=>closeToast(id),duration);
}
function closeToast(id){const el=document.getElementById(`toast-${id}`);if(el){el.style.opacity="0";el.style.transform="translateX(110%)";el.style.transition="all .2s";setTimeout(()=>el.remove(),200);}}

// ===================== STOCK ALERTS =====================
let notifiedStock=new Set();
function checkStock(){
  DB.products.forEach(p=>{
    if(p.stock<=p.minStock&&!notifiedStock.has(p.id)){
      notifiedStock.add(p.id);
      toast(`⚠️ Stock bajo: ${p.name}`,`Quedan ${p.stock} unidades (mín. ${p.minStock})`,"warn",0);
    }
    if(p.stock>p.minStock)notifiedStock.delete(p.id);
  });
}

// ===================== BALANCE =====================
function getSales(p){
  const d=new Date(hoy());
  return DB.sales.filter(s=>{
    const sd=new Date(s.date);
    if(p==="dia")return s.date===hoy();
    if(p==="semana"){const w=new Date(d);w.setDate(d.getDate()-6);return sd>=w;}
    if(p==="mes")return s.date.substring(0,7)===hoy().substring(0,7);
    return s.date.substring(0,4)===hoy().substring(0,4);
  });
}
function getExp(p){
  const d=new Date(hoy());
  return DB.expenses.filter(e=>{
    const ed=new Date(e.date);
    if(p==="dia")return e.date===hoy();
    if(p==="semana"){const w=new Date(d);w.setDate(d.getDate()-6);return ed>=w;}
    if(p==="mes")return e.date.substring(0,7)===hoy().substring(0,7);
    return e.date.substring(0,4)===hoy().substring(0,4);
  });
}
function calcBal(p){
  const sales=getSales(p),exps=getExp(p);
  const ventas=sales.reduce((a,s)=>a+s.total,0);
  const gastos=exps.reduce((a,e)=>a+e.amount,0);
  const costo=sales.reduce((a,s)=>a+s.items.reduce((b,it)=>{const pr=DB.products.find(x=>x.id===it.id);return b+(pr?pr.cost*it.qty:0);},0),0);
  return{ventas,gastos,costo,gan:ventas-gastos-costo,txn:sales.length};
}

// ===================== RENDER =====================
function render(){
  const r=document.getElementById("root");
  if(!S.user){r.innerHTML=renderLogin();return;}
  r.innerHTML=renderApp();
  checkStock();
}

function renderLogin(){
  return`<div class="login-wrap"><div class="login-box">
    <div class="login-logo"><div class="logo-icon">🍴</div><div class="logo-text">MiNegocio</div></div>
    <div class="login-title">Bienvenido de vuelta</div>
    <div class="login-sub">Selecciona tu perfil e ingresa tu PIN para continuar</div>
    <div class="user-list">${USERS.map(u=>`
      <button class="user-btn${S.selUser===u.id?" selected":""}" onclick="selUser(${u.id})">
        <div class="user-avatar" style="background:${u.color}22;color:${u.color}">${u.initials}</div>
        <div class="user-info"><div class="user-name">${u.name}</div><div class="user-role">${u.role==="admin"?"👑 Administrador":u.role==="mesero"?"🍽️ Mesero":"💰 Cajero"}</div></div>
        ${S.selUser===u.id?'<div style="color:var(--accent);font-size:18px">›</div>':''}
      </button>`).join("")}</div>
    ${S.selUser?`<div class="pin-area">
      <div class="pin-label">Ingresa tu PIN de 4 dígitos</div>
      <input class="pin-input" type="password" maxlength="4" id="pin-inp" placeholder="• • • •" value="${S.pinInput}" oninput="S.pinInput=this.value" onkeydown="if(event.key==='Enter')doLogin()" autofocus>
      ${S.pinErr?`<div class="login-err">❌ ${S.pinErr}</div>`:""}
    </div>
    <button class="btn-login" onclick="doLogin()">Ingresar al sistema →</button>`:""}
    <div style="margin-top:1.5rem;padding-top:1rem;border-top:1px solid var(--border);font-size:11px;color:var(--text3);text-align:center">
      PIN por defecto — Admin: 1234 · Mesero: 2222/3333 · Caja: 4444
    </div>
  </div></div>`;
}
function selUser(id){S.selUser=id;S.pinInput="";S.pinErr="";render();setTimeout(()=>{const el=document.getElementById("pin-inp");if(el)el.focus();},100);}
function doLogin(){
  const u=USERS.find(x=>x.id===S.selUser);if(!u)return;
  if(S.pinInput===u.pin){S.user=u;S.page=u.perms[0];S.pinInput="";S.pinErr="";render();toast(`¡Hola, ${u.name.split(" ")[0]}!`,"Sesión iniciada correctamente","success");}
  else{S.pinErr="PIN incorrecto. Intenta de nuevo.";render();setTimeout(()=>{const el=document.getElementById("pin-inp");if(el){el.value="";el.focus();}},100);}
}

function getNav(){
  const all=[
    {id:"dashboard",icon:"◈",label:"Inicio",sec:"Principal"},
    {id:"nueva_orden",icon:"＋",label:"Nueva orden",sec:"Operaciones"},
    {id:"ordenes",icon:"≡",label:"Órdenes",sec:"Operaciones"},
    {id:"ventas",icon:"$",label:"Ventas",sec:"Finanzas"},
    {id:"gastos",icon:"↓",label:"Gastos",sec:"Finanzas"},
    {id:"balance",icon:"⬡",label:"Balance",sec:"Finanzas"},
    {id:"inventario",icon:"◻",label:"Inventario",sec:"Gestión"},
    {id:"productos",icon:"✦",label:"Productos",sec:"Gestión"},
    {id:"usuarios",icon:"◎",label:"Usuarios",sec:"Gestión"},
    {id:"config",icon:"⚙",label:"Configuración",sec:"Gestión"},
  ];
  return all.filter(n=>S.user.perms.includes(n.id));
}

function renderApp(){
  const nav=getNav();
  const secs=[...new Set(nav.map(n=>n.sec))];
  const pending=DB.orders.filter(o=>o.status!=="entregado").length;
  const lowStock=DB.products.filter(p=>p.stock<=p.minStock).length;
  let content="";
  const pg=S.page;
  if(pg==="dashboard")content=renderDash();
  else if(pg==="nueva_orden")content=renderNuevaOrden();
  else if(pg==="ordenes")content=renderOrdenes();
  else if(pg==="ventas")content=renderVentas();
  else if(pg==="gastos")content=renderGastos();
  else if(pg==="balance")content=renderBalance();
  else if(pg==="inventario")content=renderInventario();
  else if(pg==="productos")content=renderProductos();
  else if(pg==="usuarios")content=renderUsuarios();
  else if(pg==="config")content=renderConfig();
  return`<div class="app-layout">
    <div class="sidebar">
      <div class="sb-header">
        <div class="sb-logo"><div class="sb-logo-icon">🍴</div><div class="sb-brand">MiNegocio</div></div>
        <div class="sb-user">
          <div class="sb-avatar" style="background:${S.user.color}22;color:${S.user.color}">${S.user.initials}</div>
          <div>
            <div class="sb-uname">${S.user.name}</div>
            <div style="font-size:10px;color:var(--text3)">${S.user.role==="admin"?"Administrador":S.user.role==="mesero"?"Mesero":"Cajero"}</div>
          </div>
        </div>
      </div>
      <div class="nav-section">
        ${secs.map(sec=>`
          <div class="nav-section-label">${sec}</div>
          ${nav.filter(n=>n.sec===sec).map(n=>`
            <div class="nav-item${S.page===n.id?" active":""}" onclick="goPage('${n.id}')">
              <span class="nav-icon">${n.icon}</span>${n.label}
              ${n.id==="ordenes"&&pending>0?`<span style="margin-left:auto;background:var(--accent);color:#fff;border-radius:99px;font-size:10px;padding:1px 7px;font-weight:700">${pending}</span>`:""}
              ${n.id==="inventario"&&lowStock>0?`<span style="margin-left:auto;background:var(--yellowbg);color:var(--yellow);border-radius:99px;font-size:10px;padding:1px 7px;font-weight:700">${lowStock}</span>`:""}
            </div>`).join("")}`).join("")}
      </div>
      <div class="sb-footer">
        <div id="save-indicator" style="font-size:11px;padding:6px 12px 8px;text-align:center;color:var(--text3)">—</div>
        <button class="btn btn-sm" style="width:calc(100% - 24px);margin:0 12px 8px;font-size:12px;color:var(--green);border-color:var(--greenbg);background:var(--greenbg)" onclick="saveData();toast('💾 Guardado','Datos guardados correctamente','success',2500)">💾 Guardar ahora</button>
        <div class="nav-item" style="color:var(--red)" onclick="logout()"><span class="nav-icon">⤺</span>Cerrar sesión</div>
      </div>
    </div>
    <div class="main-content">${content}</div>
    ${S.modal?renderModal():""}
  </div>`;
}

// ---- PAGES ----
function renderDash(){
  const b=calcBal("dia"),bm=calcBal("mes");
  const low=DB.products.filter(p=>p.stock<=p.minStock);
  const pend=DB.orders.filter(o=>o.status!=="entregado");
  const today=hoy();
  const bars=[{l:"D-6",v:51000},{l:"D-5",v:84000},{l:"D-4",v:116000},{l:"D-3",v:62000},{l:"D-2",v:40000},{l:"Ayer",v:33000},{l:"Hoy",v:b.ventas}];
  const maxV=Math.max(...bars.map(x=>x.v),1);
  const fecha=new Date().toLocaleDateString("es-CO",{weekday:"long",year:"numeric",month:"long",day:"numeric"});
  return`<div class="page-header"><div><div class="page-title">Inicio</div><div class="page-sub">📅 ${fecha}</div></div></div>
  ${low.length>0?`<div class="alert alert-warn">⚠️ Stock bajo en: ${low.map(p=>`<b>${p.name}</b>`).join(", ")} — <span style="cursor:pointer;text-decoration:underline" onclick="goPage('inventario')">Ver inventario →</span></div>`:""}
  <div class="metrics">
    <div class="metric-card"><div class="metric-icon" style="background:var(--accentbg)">💵</div><div class="metric-val" style="color:var(--accent)">${fmtk(b.ventas)}</div><div class="metric-lbl">Ventas hoy</div><div class="metric-trend" style="color:var(--green)">▲ ${b.txn} transacciones</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--greenbg)">📈</div><div class="metric-val" style="color:var(--green)">${fmtk(b.gan)}</div><div class="metric-lbl">Ganancia hoy</div><div class="metric-trend" style="color:var(--text2)">Margen ${b.ventas?Math.round(b.gan/b.ventas*100):0}%</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--redbg)">💸</div><div class="metric-val" style="color:var(--red)">${fmtk(b.gastos)}</div><div class="metric-lbl">Gastos hoy</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--bluebg)">📦</div><div class="metric-val" style="color:var(--blue)">${fmtk(bm.ventas)}</div><div class="metric-lbl">Ventas del mes</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--yellowbg)">🕐</div><div class="metric-val" style="color:var(--yellow)">${pend.length}</div><div class="metric-lbl">Órdenes activas</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--orangebg)">⚠</div><div class="metric-val" style="color:var(--orange)">${low.length}</div><div class="metric-lbl">Alertas stock</div></div>
  </div>
  <div class="two-col">
    <div class="card"><div class="card-header"><div class="card-title">Ventas últimos 7 días</div></div>
      <div class="chart-wrap">${bars.map((b2,i)=>`<div class="bar-col"><div class="bar-num">${fmtk(b2.v)}</div><div class="bar-rect" style="height:${Math.round((b2.v/maxV)*140)+4}px;background:${i===6?"var(--accent)":"var(--bg4)"}"></div><div class="bar-lbl">${b2.l}</div></div>`).join("")}</div>
    </div>
    <div class="card"><div class="card-header"><div class="card-title">Órdenes activas</div><button class="btn btn-sm" onclick="goPage('ordenes')">Ver todas</button></div>
      ${pend.length===0?`<div style="font-size:13px;color:var(--text2);text-align:center;padding:2rem 0">✓ Sin órdenes pendientes</div>`:
      `<table><thead><tr><th>Mesa</th><th>Mesero</th><th>Total</th><th>Estado</th></tr></thead><tbody>
      ${pend.map(o=>`<tr><td style="font-weight:600">${o.table}</td><td style="font-size:12px;color:var(--text2)">${o.waiter.split(" ")[0]}</td><td style="color:var(--green);font-weight:600">${fmt(o.total)}</td><td><span class="badge ${o.status==="listo"?"badge-green":"badge-yellow"}">${o.status==="listo"?"Listo":"En cocina"}</span></td></tr>`).join("")}
      </tbody></table>`}
    </div>
  </div>`;
}

function renderNuevaOrden(){
  const o=S.order;
  const total=o.items.reduce((a,i)=>a+i.price*i.qty,0);
  const cats=[...new Set(DB.products.map(p=>p.cat))];
  return`<div class="page-header"><div><div class="page-title">Nueva orden</div><div class="page-sub">Selecciona mesa y agrega productos del menú</div></div></div>
  <div class="two-col">
    <div>
      <div class="card" style="margin-bottom:1rem">
        <div class="card-title" style="margin-bottom:12px">Mesa</div>
        <select class="form-input" onchange="S.order.table=this.value;render()">
          <option value="">Seleccionar mesa...</option>
          ${["Mesa 1","Mesa 2","Mesa 3","Mesa 4","Mesa 5","Mesa 6","Mesa 7","Mesa 8","Mesa 9","Mesa 10"].map(m=>`<option${o.table===m?" selected":""}>${m}</option>`).join("")}
        </select>
      </div>
      <div class="card">
        <div class="card-title" style="margin-bottom:12px">Menú</div>
        ${cats.map(cat=>`
          <div style="font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;margin-bottom:8px;${cat!==cats[0]?"margin-top:14px":""}">${cat}</div>
          <div class="menu-grid" style="margin-bottom:4px">
            ${DB.products.filter(p=>p.cat===cat).map(p=>`
              <div class="menu-item" onclick="addItem(${p.id})" style="${p.stock===0?"opacity:.4;pointer-events:none":""}">
                <div style="font-size:22px;margin-bottom:5px">${p.emoji}</div>
                <div class="menu-item-name">${p.name}</div>
                <div class="menu-item-price">${fmt(p.price)}</div>
                ${p.stock<=p.minStock&&p.stock>0?`<div style="font-size:10px;color:var(--yellow);margin-top:2px">⚠ ${p.stock} disp.</div>`:""}
                ${p.stock===0?`<div style="font-size:10px;color:var(--red);margin-top:2px">Agotado</div>`:""}
              </div>`).join("")}
          </div>`).join("")}
      </div>
    </div>
    <div>
      <div class="card" style="position:sticky;top:0">
        <div class="card-header">
          <div class="card-title">Orden — ${o.table||"Sin mesa"}</div>
          ${o.items.length>0?`<button class="btn btn-xs btn-red" onclick="S.order.items=[];render()">Limpiar</button>`:""}
        </div>
        ${o.items.length===0?`<div style="text-align:center;padding:2.5rem 0;color:var(--text3);font-size:13px">👆 Toca un producto para agregarlo</div>`:`
        <div>${o.items.map((it,idx)=>`
          <div class="order-item">
            <div><div style="font-size:13px;font-weight:500">${it.name}</div><div style="font-size:11px;color:var(--text2)">${fmt(it.price)} c/u</div></div>
            <div class="qty-ctrl">
              <button class="qty-btn" onclick="chgQty(${idx},-1)">−</button>
              <span style="font-size:13px;font-weight:600;min-width:20px;text-align:center">${it.qty}</span>
              <button class="qty-btn" onclick="chgQty(${idx},1)">+</button>
              <span style="font-size:13px;font-weight:700;color:var(--green);min-width:75px;text-align:right">${fmt(it.price*it.qty)}</span>
            </div>
          </div>`).join("")}</div>
        <div style="display:flex;justify-content:space-between;align-items:center;padding:13px 0 0;border-top:1px solid var(--border);margin-top:8px;font-weight:700;font-size:16px">
          <span>Total</span><span style="color:var(--accent)">${fmt(total)}</span>
        </div>
        <button class="btn btn-accent" style="width:100%;margin-top:14px;padding:13px;font-size:14px" onclick="submitOrder()">🚀 Enviar a cocina</button>`}
      </div>
    </div>
  </div>`;
}

function renderOrdenes(){
  const sl={en_cocina:"badge-yellow",listo:"badge-green",entregado:"badge-blue"};
  const ll={en_cocina:"En cocina",listo:"✓ Listo",entregado:"Entregado"};
  return`<div class="page-header"><div><div class="page-title">Órdenes</div><div class="page-sub">Gestiona el flujo de cocina y cobros</div></div></div>
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>#</th><th>Mesa</th><th>Mesero</th><th>Hora</th><th>Productos</th><th>Total</th><th>Estado</th><th>Acciones</th></tr></thead><tbody>
  ${DB.orders.length===0?`<tr><td colspan="8" style="text-align:center;color:var(--text2);padding:2rem">Sin órdenes registradas</td></tr>`:""}
  ${DB.orders.map(o=>`<tr>
    <td style="font-weight:700;color:var(--text3)">#${o.id}</td>
    <td style="font-weight:600">${o.table}</td>
    <td style="font-size:12px;color:var(--text2)">${o.waiter.split(" ")[0]}</td>
    <td style="font-size:12px;color:var(--text3)">${o.time}</td>
    <td style="font-size:12px;color:var(--text2);max-width:150px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${o.items.map(i=>`${i.qty}× ${i.name}`).join(", ")}</td>
    <td style="font-weight:700;color:var(--green)">${fmt(o.total)}</td>
    <td><span class="badge ${sl[o.status]}">${ll[o.status]}</span></td>
    <td><div style="display:flex;gap:5px;flex-wrap:wrap">
      ${o.status!=="entregado"?`<button class="btn btn-xs ${o.status==="en_cocina"?"btn-green":""}" onclick="advOrder(${o.id})">${o.status==="en_cocina"?"✓ Listo":"💰 Cobrar"}</button>`:""}
      <button class="btn btn-xs" onclick="openModal('recibo',${o.id})">🖨 Recibo</button>
    </div></td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderVentas(){
  const sales=getSales(S.period);
  const total=sales.reduce((a,s)=>a+s.total,0);
  return`<div class="page-header"><div><div class="page-title">Ventas</div><div class="page-sub">${sales.length} transacciones registradas</div></div>
    <div class="header-actions"><button class="btn" onclick="exportPDF('ventas')">⤓ Exportar PDF</button></div>
  </div>
  <div class="tabs">${["dia","semana","mes","anual"].map(p=>`<div class="tab${S.period===p?" active":""}" onclick="setPer('${p}')">${p==="dia"?"Hoy":p==="semana"?"Esta semana":p==="mes"?"Este mes":"Este año"}</div>`).join("")}</div>
  <div class="metrics" style="margin-bottom:1rem">
    <div class="metric-card"><div class="metric-icon" style="background:var(--accentbg)">💵</div><div class="metric-val" style="color:var(--accent)">${fmt(total)}</div><div class="metric-lbl">Total ventas</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--greenbg)">🔢</div><div class="metric-val">${sales.length}</div><div class="metric-lbl">Transacciones</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--bluebg)">⌀</div><div class="metric-val">${sales.length?fmt(Math.round(total/sales.length)):fmt(0)}</div><div class="metric-lbl">Ticket promedio</div></div>
  </div>
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>Fecha</th><th>Mesa</th><th>Mesero</th><th>Productos</th><th>Total</th><th>Recibo</th></tr></thead><tbody>
  ${sales.length===0?`<tr><td colspan="6" style="text-align:center;color:var(--text2);padding:2rem">Sin ventas en este período</td></tr>`:""}
  ${sales.map(s=>`<tr>
    <td style="color:var(--text2);font-size:12px">${s.date}</td><td style="font-weight:500">${s.table}</td>
    <td style="font-size:12px">${s.waiter}</td>
    <td style="font-size:12px;color:var(--text2)">${s.items.map(i=>`${i.qty}× ${i.name}`).join(", ")}</td>
    <td style="font-weight:700;color:var(--green)">${fmt(s.total)}</td>
    <td><button class="btn btn-xs" onclick="openModal('recibo_sale',${s.id})">🖨</button></td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderGastos(){
  const total=DB.expenses.reduce((a,e)=>a+e.amount,0);
  return`<div class="page-header"><div><div class="page-title">Gastos</div><div class="page-sub">Registro de egresos del negocio</div></div>
    <div class="header-actions"><button class="btn" onclick="exportPDF('gastos')">⤓ Exportar PDF</button><button class="btn btn-accent" onclick="openModal('gasto')">+ Nuevo gasto</button></div>
  </div>
  <div class="metrics" style="margin-bottom:1rem">
    <div class="metric-card"><div class="metric-icon" style="background:var(--redbg)">💸</div><div class="metric-val" style="color:var(--red)">${fmt(total)}</div><div class="metric-lbl">Total gastos</div></div>
    ${[...new Set(DB.expenses.map(e=>e.cat))].map(cat=>{const v=DB.expenses.filter(e=>e.cat===cat).reduce((a,e)=>a+e.amount,0);return`<div class="metric-card"><div class="metric-icon" style="background:var(--yellowbg)">📂</div><div class="metric-val">${fmt(v)}</div><div class="metric-lbl">${cat}</div></div>`;}).join("")}
  </div>
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>Fecha</th><th>Concepto</th><th>Categoría</th><th>Monto</th><th>Registrado por</th></tr></thead><tbody>
  ${DB.expenses.length===0?`<tr><td colspan="5" style="text-align:center;color:var(--text2);padding:2rem">Sin gastos registrados</td></tr>`:""}
  ${DB.expenses.map(e=>`<tr>
    <td style="color:var(--text2);font-size:12px">${e.date}</td><td style="font-weight:500">${e.concept}</td>
    <td><span class="badge badge-orange">${e.cat}</span></td>
    <td style="font-weight:700;color:var(--red)">${fmt(e.amount)}</td>
    <td style="font-size:12px;color:var(--text2)">${e.by}</td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderBalance(){
  const b=calcBal(S.period);
  const pn={dia:"Hoy",semana:"Esta semana",mes:"Este mes",anual:"Este año"};
  const bars=[{l:"Ingresos",v:b.ventas,c:"var(--accent)"},{l:"Costos",v:b.costo,c:"var(--text3)"},{l:"Gastos",v:b.gastos,c:"var(--red)"},{l:"Utilidad",v:b.gan,c:b.gan>=0?"var(--green)":"var(--red)"}];
  const maxV=Math.max(...bars.map(x=>Math.abs(x.v)),1);
  return`<div class="page-header"><div><div class="page-title">Balance financiero</div><div class="page-sub">${pn[S.period]}</div></div>
    <div class="header-actions"><button class="btn" onclick="exportPDF('balance')">⤓ Exportar PDF</button></div>
  </div>
  <div class="tabs">${["dia","semana","mes","anual"].map(p=>`<div class="tab${S.period===p?" active":""}" onclick="setPer('${p}')">${p==="dia"?"Hoy":p==="semana"?"Semana":p==="mes"?"Mes":"Año"}</div>`).join("")}</div>
  <div class="metrics">
    <div class="metric-card"><div class="metric-icon" style="background:var(--accentbg)">📥</div><div class="metric-val" style="color:var(--accent)">${fmt(b.ventas)}</div><div class="metric-lbl">Ingresos totales</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--redbg)">📤</div><div class="metric-val" style="color:var(--red)">${fmt(b.gastos+b.costo)}</div><div class="metric-lbl">Egresos totales</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--greenbg)">✦</div><div class="metric-val" style="color:${b.gan>=0?"var(--green)":"var(--red)"}">${fmt(b.gan)}</div><div class="metric-lbl">Utilidad neta</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--yellowbg)">%</div><div class="metric-val">${b.ventas?Math.round(b.gan/b.ventas*100):0}%</div><div class="metric-lbl">Margen neto</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--bluebg)">🔢</div><div class="metric-val">${b.txn}</div><div class="metric-lbl">Transacciones</div></div>
    <div class="metric-card"><div class="metric-icon" style="background:var(--bg4)">📦</div><div class="metric-val">${fmt(b.costo)}</div><div class="metric-lbl">Costo mercancía</div></div>
  </div>
  <div class="card"><div class="card-header"><div class="card-title">Resumen visual — ${pn[S.period]}</div></div>
    <div class="chart-wrap" style="height:190px">${bars.map(b2=>`<div class="bar-col"><div class="bar-num" style="font-size:12px">${fmtk(b2.v)}</div><div class="bar-rect" style="height:${Math.round((Math.abs(b2.v)/maxV)*165)+4}px;background:${b2.c};opacity:.9"></div><div class="bar-lbl" style="font-size:12px">${b2.l}</div></div>`).join("")}</div>
  </div>`;
}

function renderInventario(){
  const low=DB.products.filter(p=>p.stock<=p.minStock);
  return`<div class="page-header"><div><div class="page-title">Inventario</div><div class="page-sub">Control de existencias en tiempo real</div></div></div>
  ${low.length>0?`<div class="alert alert-warn">⚠️ Productos con stock bajo: ${low.map(p=>`<b>${p.name}</b> (${p.stock} uds.)`).join(" · ")}</div>`:"<div class='alert alert-success'>✅ Todos los productos tienen stock suficiente</div>"}
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>Producto</th><th>Categoría</th><th>Stock actual</th><th>Stock mínimo</th><th>Estado</th></tr></thead><tbody>
  ${DB.products.map(p=>`<tr>
    <td><span style="margin-right:10px;font-size:16px">${p.emoji}</span><span style="font-weight:600">${p.name}</span></td>
    <td><span class="badge badge-purple">${p.cat}</span></td>
    <td><span style="font-weight:700;font-size:15px;color:${p.stock===0?"var(--red)":p.stock<=p.minStock?"var(--yellow)":"var(--green)"}">${p.stock}</span></td>
    <td style="color:var(--text2)">${p.minStock}</td>
    <td><span class="badge ${p.stock===0?"badge-red":p.stock<=p.minStock?"badge-yellow":"badge-green"}">${p.stock===0?"Agotado":p.stock<=p.minStock?"Stock bajo":"Normal"}</span></td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderProductos(){
  return`<div class="page-header"><div><div class="page-title">Productos</div><div class="page-sub">Gestiona tu catálogo de productos y precios</div></div>
    <button class="btn btn-accent" onclick="openModal('producto')">+ Nuevo producto</button>
  </div>
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>Producto</th><th>Categoría</th><th>Precio venta</th><th>Costo</th><th>Margen</th><th>Stock</th><th>Acciones</th></tr></thead><tbody>
  ${DB.products.map(p=>`<tr>
    <td><span style="margin-right:10px;font-size:15px">${p.emoji}</span><span style="font-weight:600">${p.name}</span></td>
    <td><span class="badge badge-purple">${p.cat}</span></td>
    <td style="font-weight:600;color:var(--green)">${fmt(p.price)}</td>
    <td style="color:var(--text2)">${fmt(p.cost)}</td>
    <td><span style="font-weight:600;color:var(--blue)">${Math.round((p.price-p.cost)/p.price*100)}%</span></td>
    <td style="font-weight:500">${p.stock}</td>
    <td><div style="display:flex;gap:6px"><button class="btn btn-xs" onclick="openModal('producto',${p.id})">Editar</button><button class="btn btn-xs btn-red" onclick="delProd(${p.id})">Eliminar</button></div></td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderUsuarios(){
  const rl={admin:"Administrador",mesero:"Mesero",caja:"Cajero"};
  const rb={admin:"badge-purple",mesero:"badge-green",caja:"badge-yellow"};
  return`<div class="page-header"><div><div class="page-title">Usuarios</div><div class="page-sub">Gestiona accesos, roles y PINs</div></div>
    <button class="btn btn-accent" onclick="openModal('usuario')">+ Nuevo usuario</button>
  </div>
  <div class="card"><div class="tbl-wrap">
  <table><thead><tr><th>Usuario</th><th>Rol</th><th>PIN</th><th>Módulos accesibles</th><th>Acciones</th></tr></thead><tbody>
  ${USERS.map(u=>`<tr>
    <td><div style="display:flex;align-items:center;gap:10px">
      <div style="width:36px;height:36px;border-radius:50%;background:${u.color}22;color:${u.color};display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;flex-shrink:0">${u.initials}</div>
      <div><div style="font-weight:600">${u.name}</div></div>
    </div></td>
    <td><span class="badge ${rb[u.role]}">${rl[u.role]}</span></td>
    <td style="font-family:monospace;letter-spacing:4px;color:var(--text3)">••••</td>
    <td style="font-size:11px;color:var(--text2)">${u.perms.slice(0,4).join(", ")}${u.perms.length>4?` +${u.perms.length-4} más`:""}</td>
    <td><button class="btn btn-xs" onclick="openModal('usuario',${u.id})">Editar PIN</button></td>
  </tr>`).join("")}
  </tbody></table></div></div>`;
}

function renderConfig(){
  const n=DB.negocio;
  return`<div class="page-header"><div><div class="page-title">Configuración</div><div class="page-sub">Datos del negocio e impresora Epson</div></div></div>
  <div class="two-col">
    <div class="card">
      <div class="card-title" style="margin-bottom:1.25rem">📋 Datos del negocio</div>
      <div class="form-group" style="margin-bottom:10px"><div class="form-label">Nombre del negocio</div><input class="form-input" id="cfg-nombre" value="${n.nombre}"></div>
      <div class="form-group" style="margin-bottom:10px"><div class="form-label">NIT / RUT</div><input class="form-input" id="cfg-nit" value="${n.nit}"></div>
      <div class="form-group" style="margin-bottom:10px"><div class="form-label">Dirección</div><input class="form-input" id="cfg-dir" value="${n.dir}"></div>
      <div class="form-grid" style="margin-bottom:10px">
        <div class="form-group"><div class="form-label">Teléfono</div><input class="form-input" id="cfg-tel" value="${n.tel}"></div>
        <div class="form-group"><div class="form-label">Ciudad</div><input class="form-input" id="cfg-ciudad" value="${n.ciudad}"></div>
      </div>
      <button class="btn btn-accent" onclick="saveConfig()">💾 Guardar cambios</button>
      <div style="margin-top:1rem;padding:12px;background:var(--greenbg);border-radius:var(--r3);border:1px solid rgba(0,196,140,.2);font-size:12px;color:var(--green)">
        ✅ Guardado automático activado — los datos se guardan en este navegador/computador cada vez que haces un cambio.
      </div>
    </div>
    <div>
      <div class="card" style="margin-bottom:1rem">
        <div class="card-title" style="margin-bottom:1rem">🖨️ Configurar impresora Epson</div>
        <div style="display:flex;flex-direction:column;gap:8px;font-size:13px;color:var(--text2);line-height:1.7">
          <div><span style="color:var(--accent);font-weight:600">Modelos compatibles:</span><br>TM-T20, TM-T88, TM-T20III, TM-U220, TM-T82</div>
          <div><span style="color:var(--accent);font-weight:600">Papel:</span> 58mm o 80mm térmico</div>
          <div><span style="color:var(--yellow);font-weight:600">Pasos para configurar:</span>
            <ol style="margin-left:18px;margin-top:4px">
              <li>Descarga e instala <b style="color:var(--text)">Epson APD 5</b> desde epson.com.co</li>
              <li>Conecta la impresora por USB o red</li>
              <li>En Windows: Configura la Epson como <b style="color:var(--text)">impresora predeterminada</b></li>
              <li>Al imprimir desde Chrome:<br>→ Desmarca "Encabezados y pies de página"<br>→ Márgenes: <b style="color:var(--text)">Ninguno</b><br>→ Tamaño: <b style="color:var(--text)">80mm × automático</b></li>
              <li>¡Listo! Haz clic en 🖨 Recibo en cualquier orden</li>
            </ol>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-title" style="margin-bottom:1rem">Vista previa recibo</div>
        ${buildReceiptHTML(DB.orders[0]||DB.sales[0],true)}
      </div>
      <div class="card" style="border-color:rgba(255,92,122,.35)">
        <div class="card-title" style="color:var(--red);margin-bottom:.5rem">⚠️ Zona peligrosa</div>
        <div style="font-size:12px;color:var(--text2);margin-bottom:12px;line-height:1.6">Borra permanentemente <b style="color:var(--text)">todas las ventas, gastos, órdenes, productos y usuarios</b> guardados en este navegador. No se puede deshacer.</div>
        <button class="btn btn-red" onclick="borrarTodosDatos()">🗑️ Borrar todos los datos</button>
      </div>
    </div>
  </div>`;
}

function saveConfig(){
  DB.negocio.nombre=document.getElementById("cfg-nombre")?.value||DB.negocio.nombre;
  DB.negocio.nit=document.getElementById("cfg-nit")?.value||DB.negocio.nit;
  DB.negocio.dir=document.getElementById("cfg-dir")?.value||DB.negocio.dir;
  DB.negocio.tel=document.getElementById("cfg-tel")?.value||DB.negocio.tel;
  DB.negocio.ciudad=document.getElementById("cfg-ciudad")?.value||DB.negocio.ciudad;
  toast("Configuración guardada","Los datos del negocio han sido actualizados","success");
  render();
}

// ===================== RECIBO =====================
function buildReceiptHTML(order,isPreview=false){
  if(!order)return`<div style="color:var(--text2);font-size:13px;text-align:center;padding:1rem">Sin datos de orden</div>`;
  const n=DB.negocio;
  const now=new Date();
  const fecha=`${hoy()} ${now.toLocaleTimeString("es-CO",{hour:"2-digit",minute:"2-digit"})}`;
  const subtotal=order.items.reduce((a,i)=>a+i.price*i.qty,0);
  const iva=Math.round(subtotal*0.19);
  const fs=isPreview?"11px":"10pt";
  const w=isPreview?"280px":"74mm";
  const p=isPreview?"14px 12px":"4mm 3mm";
  return`<div style="font-family:'Courier New',Courier,monospace;font-size:${fs};color:#000;background:#fff;padding:${p};width:${w};margin:${isPreview?"0 auto":"0"}">
    <div style="text-align:center;margin-bottom:8px">
      <div style="font-size:${isPreview?"14px":"13pt"};font-weight:bold;margin-bottom:2px">${n.nombre}</div>
      <div>NIT: ${n.nit}</div>
      <div>${n.dir}</div>
      <div>${n.ciudad}</div>
      <div>Tel: ${n.tel}</div>
    </div>
    <div style="border-top:1px dashed #000;margin:6px 0"></div>
    <div style="display:flex;justify-content:space-between"><span>Fecha:</span><span>${fecha}</span></div>
    <div style="display:flex;justify-content:space-between"><span>Mesa:</span><span style="font-weight:bold">${order.table}</span></div>
    <div style="display:flex;justify-content:space-between"><span>Mesero:</span><span>${order.waiter}</span></div>
    <div style="display:flex;justify-content:space-between"><span>Orden #:</span><span>${order.id}</span></div>
    <div style="border-top:1px dashed #000;margin:6px 0"></div>
    <div style="display:flex;justify-content:space-between;font-weight:bold;font-size:${isPreview?"10px":"9pt"};margin-bottom:4px">
      <span style="flex:3">DESCRIPCIÓN</span><span style="flex:1;text-align:center">CANT</span><span style="flex:2;text-align:right">VALOR</span>
    </div>
    <div style="border-top:1px dashed #000;margin-bottom:5px"></div>
    ${order.items.map(it=>`
      <div style="display:flex;justify-content:space-between;margin-bottom:1px">
        <span style="flex:3;overflow:hidden;white-space:nowrap;text-overflow:ellipsis">${it.name.substring(0,15)}</span>
        <span style="flex:1;text-align:center">${it.qty}</span>
        <span style="flex:2;text-align:right">${(it.price*it.qty).toLocaleString("es-CO")}</span>
      </div>
      <div style="font-size:${isPreview?"9px":"8pt"};color:#555;margin-bottom:4px;padding-left:2px">  @ ${it.price.toLocaleString("es-CO")} c/u</div>`).join("")}
    <div style="border-top:1px dashed #000;margin:6px 0"></div>
    <div style="display:flex;justify-content:space-between;margin-bottom:2px"><span>SUBTOTAL:</span><span>$ ${subtotal.toLocaleString("es-CO")}</span></div>
    <div style="display:flex;justify-content:space-between;margin-bottom:2px;color:#555"><span>IVA (19%):</span><span>$ ${iva.toLocaleString("es-CO")}</span></div>
    <div style="border-top:1px dashed #000;margin:4px 0"></div>
    <div style="display:flex;justify-content:space-between;font-weight:bold;font-size:${isPreview?"14px":"12pt"};margin-top:2px">
      <span>TOTAL A PAGAR:</span><span>$ ${subtotal.toLocaleString("es-CO")}</span>
    </div>
    <div style="border-top:1px dashed #000;margin:8px 0"></div>
    <div style="text-align:center;font-size:${isPreview?"9px":"8pt"}">
      <div style="font-weight:bold;margin-bottom:2px">¡Gracias por su visita!</div>
      <div>${n.nombre}</div>
      <div style="margin:4px 0">* * * * * * * * * * * * * * * *</div>
      <div style="color:#777">Factura de venta • No fiscal</div>
    </div>
  </div>`;
}

function printReceipt(order){
  const pa=document.getElementById("print-area");
  pa.innerHTML=buildReceiptHTML(order,false);
  pa.style.display="block";
  setTimeout(()=>{window.print();setTimeout(()=>{pa.style.display="none";},800);},150);
  toast("Enviando a impresora","Recibo enviado a la Epson","success");
}

// ===================== MODAL =====================
function renderModal(){
  const m=S.modal;
  if(m.type==="recibo"||m.type==="recibo_sale"){
    const order=m.type==="recibo"?DB.orders.find(x=>x.id===m.id):DB.sales.find(x=>x.id===m.id);
    if(!order)return"";
    const orderStr=JSON.stringify(order).replace(/"/g,"&quot;");
    return`<div class="modal-backdrop" onclick="if(event.target===this)closeModal()"><div class="modal" style="max-width:360px">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1.25rem">
        <div class="modal-title" style="margin-bottom:0">🖨️ Vista previa del recibo</div>
        <button style="background:none;border:none;color:var(--text2);cursor:pointer;font-size:18px" onclick="closeModal()">✕</button>
      </div>
      <div style="background:#f0f0f0;border-radius:var(--r2);padding:12px;margin-bottom:1rem;overflow:hidden">
        ${buildReceiptHTML(order,true)}
      </div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-accent" style="flex:1;padding:13px;font-size:14px" onclick='printReceipt(JSON.parse(decodeURIComponent("${encodeURIComponent(JSON.stringify(order))}")))'>🖨️ Imprimir en Epson</button>
        <button class="btn" onclick="closeModal()">Cerrar</button>
      </div>
      <div style="font-size:11px;color:var(--text3);margin-top:10px;text-align:center">Asegúrate que la impresora esté encendida y conectada</div>
    </div></div>`;
  }
  if(m.type==="gasto"){
    return`<div class="modal-backdrop" onclick="if(event.target===this)closeModal()"><div class="modal">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1.25rem"><div class="modal-title" style="margin-bottom:0">Registrar gasto</div><button style="background:none;border:none;color:var(--text2);cursor:pointer;font-size:18px" onclick="closeModal()">✕</button></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Concepto</div><input class="form-input" id="m-concept" placeholder="Descripción del gasto"></div><div class="form-group"><div class="form-label">Monto (COP)</div><input class="form-input" type="number" id="m-amount" placeholder="0"></div></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Categoría</div><select class="form-input" id="m-cat"><option>Insumos</option><option>Servicios</option><option>Nómina</option><option>Mantenimiento</option><option>Arriendo</option><option>Otro</option></select></div><div class="form-group"><div class="form-label">Fecha</div><input class="form-input" id="m-date" type="date" value="${hoy()}"></div></div>
      <div class="form-actions"><button class="btn btn-accent" onclick="saveGasto()">Guardar</button><button class="btn" onclick="closeModal()">Cancelar</button></div>
    </div></div>`;
  }
  if(m.type==="producto"){
    const p=m.id?DB.products.find(x=>x.id===m.id):null;
    return`<div class="modal-backdrop" onclick="if(event.target===this)closeModal()"><div class="modal">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1.25rem"><div class="modal-title" style="margin-bottom:0">${p?"Editar producto":"Nuevo producto"}</div><button style="background:none;border:none;color:var(--text2);cursor:pointer;font-size:18px" onclick="closeModal()">✕</button></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Nombre del producto</div><input class="form-input" id="m-pname" value="${p?.name||""}" placeholder="Ej: Bandeja paisa"></div><div class="form-group"><div class="form-label">Categoría</div><input class="form-input" id="m-pcat" value="${p?.cat||""}" placeholder="Ej: Platos"></div></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Precio de venta (COP)</div><input class="form-input" type="number" id="m-pprice" value="${p?.price||""}" placeholder="0"></div><div class="form-group"><div class="form-label">Costo (COP)</div><input class="form-input" type="number" id="m-pcost" value="${p?.cost||""}" placeholder="0"></div></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Stock actual</div><input class="form-input" type="number" id="m-pstock" value="${p?.stock||""}" placeholder="0"></div><div class="form-group"><div class="form-label">Stock mínimo (alerta)</div><input class="form-input" type="number" id="m-pmin" value="${p?.minStock||""}" placeholder="5"></div></div>
      <div class="form-group" style="margin-bottom:14px"><div class="form-label">Emoji del producto</div><input class="form-input" id="m-pemoji" value="${p?.emoji||"🍽️"}" maxlength="2" style="font-size:20px;text-align:center;width:80px"></div>
      <div class="form-actions"><button class="btn btn-accent" onclick="saveProd(${p?p.id:"null"})">Guardar producto</button><button class="btn" onclick="closeModal()">Cancelar</button></div>
    </div></div>`;
  }
  if(m.type==="usuario"){
    const u=m.id?USERS.find(x=>x.id===m.id):null;
    return`<div class="modal-backdrop" onclick="if(event.target===this)closeModal()"><div class="modal">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1.25rem"><div class="modal-title" style="margin-bottom:0">${u?"Editar usuario":"Nuevo usuario"}</div><button style="background:none;border:none;color:var(--text2);cursor:pointer;font-size:18px" onclick="closeModal()">✕</button></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">Nombre completo</div><input class="form-input" id="m-uname" value="${u?.name||""}" placeholder="Nombre y apellido"></div><div class="form-group"><div class="form-label">Rol</div><select class="form-input" id="m-urole"><option value="admin"${u?.role==="admin"?" selected":""}>👑 Administrador</option><option value="mesero"${u?.role==="mesero"?" selected":""}>🍽️ Mesero</option><option value="caja"${u?.role==="caja"?" selected":""}>💰 Cajero</option></select></div></div>
      <div class="form-grid"><div class="form-group"><div class="form-label">PIN (4 dígitos)</div><input class="form-input" id="m-upin" type="password" maxlength="4" placeholder="••••"></div><div class="form-group"><div class="form-label">Confirmar PIN</div><input class="form-input" id="m-upin2" type="password" maxlength="4" placeholder="••••"></div></div>
      <div id="m-uerr" style="color:var(--red);font-size:12px;margin-bottom:8px;min-height:16px"></div>
      <div class="form-actions"><button class="btn btn-accent" onclick="saveUser(${u?u.id:"null"})">Guardar usuario</button><button class="btn" onclick="closeModal()">Cancelar</button></div>
    </div></div>`;
  }
  return"";
}

// ===================== ACTIONS =====================
function openModal(type,id=null){S.modal={type,id};render();}
function closeModal(){S.modal=null;render();}
function goPage(p){S.page=p;S.modal=null;render();}
function setPer(p){S.period=p;render();}
function logout(){S.user=null;S.selUser=null;S.pinInput="";S.pinErr="";S.order={table:"",items:[]};render();}

function addItem(pid){
  const p=DB.products.find(x=>x.id===pid);if(!p||p.stock===0)return;
  const ex=S.order.items.find(i=>i.id===pid);
  if(ex)ex.qty++;else S.order.items.push({id:p.id,name:p.name,price:p.price,qty:1});
  render();
}
function chgQty(idx,d){
  S.order.items[idx].qty+=d;
  if(S.order.items[idx].qty<=0)S.order.items.splice(idx,1);
  render();
}
function submitOrder(){
  if(!S.order.table){toast("Mesa requerida","Por favor selecciona una mesa","warn");return;}
  if(!S.order.items.length){toast("Orden vacía","Agrega al menos un producto","warn");return;}
  const ord={id:DB.nxt.order++,table:S.order.table,waiter:S.user.name,items:[...S.order.items],total:S.order.items.reduce((a,i)=>a+i.price*i.qty,0),status:"en_cocina",time:new Date().toLocaleTimeString("es-CO",{hour:"2-digit",minute:"2-digit"})};
  DB.orders.push(ord);
  S.order={table:"",items:[]};S.page="ordenes";
  toast("🚀 Orden enviada a cocina",`${ord.table} · ${ord.items.length} producto(s) · ${fmt(ord.total)}`,"success");
  render();
}
function advOrder(id){
  const o=DB.orders.find(x=>x.id===id);
  if(o.status==="en_cocina"){
    o.status="listo";
    toast("✅ Orden lista",`${o.table} está lista para ser entregada`,"info");
  } else if(o.status==="listo"){
    o.status="entregado";
    const sale={id:DB.nxt.sale++,date:hoy(),items:o.items,total:o.total,waiter:o.waiter,table:o.table,status:"pagado"};
    DB.sales.push(sale);
    o.items.forEach(it=>{const p=DB.products.find(p=>p.id===it.id);if(p)p.stock=Math.max(0,p.stock-it.qty);});
    checkStock();
    toast("💰 Venta cobrada",`${o.table} · ${fmt(o.total)}`,"success");
    setTimeout(()=>{if(confirm(`¿Imprimir recibo para ${o.table}?`))printReceipt(o);},300);
  }
  render();
}
function saveGasto(){
  const concept=document.getElementById("m-concept")?.value?.trim();
  const amount=parseFloat(document.getElementById("m-amount")?.value);
  const cat=document.getElementById("m-cat")?.value;
  const date=document.getElementById("m-date")?.value;
  if(!concept||!amount||amount<=0){toast("Datos incompletos","Completa concepto y monto","warn");return;}
  DB.expenses.push({id:DB.nxt.exp++,concept,amount,cat,date:date||hoy(),by:S.user.name});
  closeModal();toast("Gasto registrado",`${concept} · ${fmt(amount)}`,"success");
}
function saveProd(id){
  const name=document.getElementById("m-pname")?.value?.trim();
  const cat=document.getElementById("m-pcat")?.value?.trim();
  const price=parseFloat(document.getElementById("m-pprice")?.value);
  const cost=parseFloat(document.getElementById("m-pcost")?.value)||0;
  const stock=parseInt(document.getElementById("m-pstock")?.value)||0;
  const minStock=parseInt(document.getElementById("m-pmin")?.value)||5;
  const emoji=document.getElementById("m-pemoji")?.value||"🍽️";
  if(!name||!price||price<=0){toast("Datos incompletos","Nombre y precio son obligatorios","warn");return;}
  if(id&&id!=="null"){const p=DB.products.find(x=>x.id===id);Object.assign(p,{name,cat,price,cost,stock,minStock,emoji});}
  else DB.products.push({id:DB.nxt.prod++,name,cat,price,cost,stock,minStock,emoji});
  closeModal();toast("Producto guardado","Catálogo actualizado correctamente","success");
}
function delProd(id){
  if(confirm("¿Estás seguro de eliminar este producto?\nEsta acción no se puede deshacer.")){
    DB.products=DB.products.filter(p=>p.id!==id);
    toast("Producto eliminado","El producto fue removido del catálogo","info");
    render();
  }
}
function saveUser(id){
  const name=document.getElementById("m-uname")?.value?.trim();
  const role=document.getElementById("m-urole")?.value;
  const pin=document.getElementById("m-upin")?.value;
  const pin2=document.getElementById("m-upin2")?.value;
  const errEl=document.getElementById("m-uerr");
  if(!name){if(errEl)errEl.textContent="El nombre es obligatorio";return;}
  if(pin&&pin!==pin2){if(errEl)errEl.textContent="Los PINs no coinciden";return;}
  if(pin&&(pin.length!==4||isNaN(pin))){if(errEl)errEl.textContent="El PIN debe ser exactamente 4 dígitos numéricos";return;}
  const rolePerms={
    admin:["dashboard","nueva_orden","ordenes","ventas","gastos","inventario","productos","balance","usuarios","config"],
    mesero:["nueva_orden","ordenes"],
    caja:["ordenes","ventas","balance"]
  };
  if(id&&id!=="null"){
    const u=USERS.find(x=>x.id===id);
    u.name=name;u.role=role;u.perms=rolePerms[role];
    if(pin)u.pin=pin;
    u.initials=name.split(" ").map(w=>w[0]).join("").slice(0,2).toUpperCase();
  }else{
    const cols=["#6c63ff","#00c48c","#4da6ff","#ffb547","#ff5c7a","#ff7a3d"];
    const newU={id:Math.max(...USERS.map(u=>u.id))+1,name,role,pin:pin||"0000",initials:name.split(" ").map(w=>w[0]).join("").slice(0,2).toUpperCase(),color:cols[USERS.length%cols.length],perms:rolePerms[role]};
    USERS.push(newU);
  }
  closeModal();toast("Usuario guardado","Acceso actualizado correctamente","success");
}

// ===================== PDF =====================
function exportPDF(section){
  try{
    const{jsPDF}=window.jspdf;
    const doc=new jsPDF();
    doc.setFillColor(15,17,23);doc.rect(0,0,210,297,"F");
    doc.setFillColor(108,99,255);doc.rect(0,0,210,44,"F");
    doc.setTextColor(255,255,255);doc.setFontSize(20);doc.setFont("helvetica","bold");
    doc.text(DB.negocio.nombre,15,18);
    const titles={ventas:"📊 Reporte de Ventas",gastos:"💸 Reporte de Gastos",balance:"📈 Balance Financiero"};
    doc.setFontSize(13);doc.setFont("helvetica","normal");doc.text(titles[section],15,29);
    doc.setFontSize(9);doc.text(`Generado el ${hoy()} por ${S.user.name} · NIT: ${DB.negocio.nit}`,15,39);
    let y=56;
    const pn={dia:"Hoy",semana:"Esta semana",mes:"Este mes",anual:"Este año"};
    if(section==="ventas"){
      const sales=getSales(S.period);const total=sales.reduce((a,s)=>a+s.total,0);
      doc.setTextColor(139,145,176);doc.setFontSize(10);doc.text(`Período: ${pn[S.period]} · ${sales.length} transacciones`,15,y);y+=14;
      doc.setFillColor(30,35,53);doc.roundedRect(10,y,90,22,3,3,"F");doc.roundedRect(110,y,90,22,3,3,"F");
      doc.setTextColor(100,106,140);doc.setFontSize(8.5);doc.text("TOTAL VENTAS",14,y+9);doc.text("TRANSACCIONES",114,y+9);
      doc.setTextColor(108,99,255);doc.setFontSize(13);doc.setFont("helvetica","bold");
      doc.text("$ "+total.toLocaleString("es-CO"),14,y+19);
      doc.setTextColor(0,196,140);doc.text(String(sales.length),114,y+19);
      y+=32;
      doc.setFillColor(37,42,61);doc.roundedRect(10,y,190,8,2,2,"F");
      doc.setTextColor(80,90,130);doc.setFontSize(8);doc.setFont("helvetica","bold");
      doc.text("FECHA",13,y+5.5);doc.text("MESA",52,y+5.5);doc.text("MESERO",88,y+5.5);doc.text("TOTAL",178,y+5.5);
      y+=12;
      sales.forEach(s=>{
        if(y>272){doc.addPage();doc.setFillColor(15,17,23);doc.rect(0,0,210,297,"F");y=15;}
        doc.setTextColor(155,160,185);doc.setFontSize(9);doc.setFont("helvetica","normal");
        doc.text(s.date,13,y);doc.text(s.table,52,y);doc.text(s.waiter.substring(0,22),88,y);
        doc.setTextColor(0,196,140);doc.setFont("helvetica","bold");
        doc.text("$ "+s.total.toLocaleString("es-CO"),196,y,{align:"right"});
        y+=7;doc.setDrawColor(40,46,72);doc.line(10,y-1.5,200,y-1.5);
      });
    }else if(section==="gastos"){
      const total=DB.expenses.reduce((a,e)=>a+e.amount,0);
      doc.setFillColor(30,35,53);doc.roundedRect(10,y,190,22,3,3,"F");
      doc.setTextColor(100,106,140);doc.setFontSize(8.5);doc.text("TOTAL GASTOS",14,y+9);
      doc.setTextColor(255,92,122);doc.setFontSize(13);doc.setFont("helvetica","bold");
      doc.text("$ "+total.toLocaleString("es-CO"),14,y+19);y+=32;
      DB.expenses.forEach(e=>{
        if(y>272){doc.addPage();doc.setFillColor(15,17,23);doc.rect(0,0,210,297,"F");y=15;}
        doc.setFillColor(30,35,53);doc.roundedRect(10,y,190,16,2,2,"F");
        doc.setTextColor(155,160,185);doc.setFontSize(9);doc.setFont("helvetica","normal");
        doc.text(e.date,14,y+11);doc.text(e.concept.substring(0,28),44,y+11);doc.text(e.cat,135,y+11);
        doc.setTextColor(255,92,122);doc.setFont("helvetica","bold");
        doc.text("$ "+e.amount.toLocaleString("es-CO"),196,y+11,{align:"right"});
        y+=20;
      });
    }else if(section==="balance"){
      const b=calcBal(S.period);
      doc.setTextColor(139,145,176);doc.setFontSize(10);doc.text(`Período: ${pn[S.period]}`,15,y);y+=14;
      [{l:"Ingresos totales",v:b.ventas,c:[108,99,255]},{l:"Costo de mercancía",v:b.costo,c:[100,110,150]},{l:"Gastos operativos",v:b.gastos,c:[255,92,122]},{l:"Utilidad neta",v:b.gan,c:b.gan>=0?[0,196,140]:[255,92,122]},{l:"Margen neto",v:null,extra:`${b.ventas?Math.round(b.gan/b.ventas*100):0}%`,c:[77,166,255]},{l:"Transacciones",v:null,extra:String(b.txn),c:[255,181,71]}].forEach(item=>{
        doc.setFillColor(30,35,53);doc.roundedRect(10,y,190,18,3,3,"F");
        doc.setTextColor(155,160,185);doc.setFontSize(11);doc.setFont("helvetica","normal");doc.text(item.l,15,y+12);
        doc.setTextColor(...item.c);doc.setFont("helvetica","bold");
        const val=item.extra?item.extra:"$ "+(item.v||0).toLocaleString("es-CO");
        doc.text(val,196,y+12,{align:"right"});y+=22;
      });
    }
    doc.save(`${section}_${hoy()}.pdf`);
    toast("✅ PDF generado","Archivo descargado correctamente","success");
  }catch(e){alert("Error al generar PDF:\n"+e.message);}
}

// ===================== PERSISTENCIA localStorage =====================
const STORAGE_KEY = "minegocio_pos_v1";
const USERS_KEY   = "minegocio_users_v1";
let autoSaveTimer = null;
let lastSaveTime  = null;
let unsavedChanges = false;

const DEFAULT_DB = {
  negocio:{nombre:"Restaurante El Sabor",nit:"900.123.456-7",dir:"Cra 5 #10-23, Santa Marta",tel:"300 123 4567",ciudad:"Santa Marta, Magdalena"},
  products:[
    {id:1,name:"Bandeja paisa",cat:"Platos",price:28000,cost:12000,stock:50,minStock:5,emoji:"🍱"},
    {id:2,name:"Ajiaco santafereño",cat:"Sopas",price:22000,cost:9000,stock:30,minStock:5,emoji:"🍲"},
    {id:3,name:"Arepa con queso",cat:"Desayunos",price:8000,cost:2500,stock:4,minStock:5,emoji:"🫓"},
    {id:4,name:"Jugo de lulo",cat:"Bebidas",price:7000,cost:2000,stock:60,minStock:10,emoji:"🧃"},
    {id:5,name:"Café tinto",cat:"Bebidas",price:3000,cost:800,stock:3,minStock:20,emoji:"☕"},
    {id:6,name:"Sancocho de gallina",cat:"Sopas",price:25000,cost:11000,stock:20,minStock:5,emoji:"🍗"},
  ],
  sales:[],
  expenses:[],
  orders:[],
  nxt:{prod:7,sale:1,exp:1,order:101}
};

const DEFAULT_USERS = [
  {id:1,name:"Carlos Ruiz",role:"admin",pin:"1234",color:"#6c63ff",initials:"CR",perms:["dashboard","nueva_orden","ordenes","ventas","gastos","inventario","productos","balance","usuarios","config"]},
  {id:2,name:"María López",role:"mesero",pin:"2222",color:"#00c48c",initials:"ML",perms:["nueva_orden","ordenes"]},
  {id:3,name:"Juan Pérez",role:"mesero",pin:"3333",color:"#4da6ff",initials:"JP",perms:["nueva_orden","ordenes"]},
  {id:4,name:"Ana Torres",role:"caja",pin:"4444",color:"#ffb547",initials:"AT",perms:["ordenes","ventas","balance"]},
];

function saveData(){
  try{
    localStorage.setItem(STORAGE_KEY, JSON.stringify({
      negocio: DB.negocio,
      products: DB.products,
      sales: DB.sales,
      expenses: DB.expenses,
      orders: DB.orders,
      nxt: DB.nxt
    }));
    localStorage.setItem(USERS_KEY, JSON.stringify(USERS));
    lastSaveTime = new Date();
    unsavedChanges = false;
    updateSaveIndicator();
  }catch(e){
    console.error("Error guardando datos:", e);
    toast("Error al guardar","No se pudieron guardar los datos","warn");
  }
}

function loadData(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    const rawUsers = localStorage.getItem(USERS_KEY);
    if(raw){
      const saved = JSON.parse(raw);
      DB.negocio   = saved.negocio   || DB.negocio;
      DB.products  = saved.products  || DB.products;
      DB.sales     = saved.sales     || DB.sales;
      DB.expenses  = saved.expenses  || DB.expenses;
      DB.orders    = saved.orders    || DB.orders;
      DB.nxt       = saved.nxt       || DB.nxt;
    }
    if(rawUsers){
      const savedUsers = JSON.parse(rawUsers);
      USERS.length = 0;
      savedUsers.forEach(u => USERS.push(u));
    }
    lastSaveTime = raw ? new Date() : null;
    return !!raw;
  }catch(e){
    console.error("Error cargando datos:", e);
    return false;
  }
}

function markDirty(){
  unsavedChanges = true;
  clearTimeout(autoSaveTimer);
  autoSaveTimer = setTimeout(()=>{
    saveData();
    toast("💾 Guardado automático","Todos los datos han sido guardados","success",2500);
  }, 3000); // guarda 3 segundos después de la última acción
  updateSaveIndicator();
}

function updateSaveIndicator(){
  const el = document.getElementById("save-indicator");
  if(!el) return;
  if(unsavedChanges){
    el.innerHTML = `<span style="color:var(--yellow)">● Guardando...</span>`;
  } else if(lastSaveTime){
    const t = lastSaveTime.toLocaleTimeString("es-CO",{hour:"2-digit",minute:"2-digit"});
    el.innerHTML = `<span style="color:var(--green)">✓ Guardado ${t}</span>`;
  }
}

function borrarTodosDatos(){
  if(!confirm("⚠️ ATENCIÓN\n\n¿Estás seguro de que quieres BORRAR TODOS los datos guardados?\n\nSe eliminarán ventas, gastos, productos, usuarios y configuración.\n\nEsta acción NO se puede deshacer.")){return;}
  if(!confirm("Segunda confirmación: ¿Borrar TODO definitivamente?")){return;}
  localStorage.removeItem(STORAGE_KEY);
  localStorage.removeItem(USERS_KEY);
  // Restore defaults
  Object.assign(DB, JSON.parse(JSON.stringify(DEFAULT_DB)));
  USERS.length = 0;
  JSON.parse(JSON.stringify(DEFAULT_USERS)).forEach(u=>USERS.push(u));
  lastSaveTime = null;
  unsavedChanges = false;
  toast("Datos borrados","Se restauraron los datos de ejemplo","info");
  render();
}

// Patch all write operations to call markDirty automatically
const _origSubmitOrder    = submitOrder;
const _origAdvOrder       = advOrder;
const _origSaveGasto      = saveGasto;
const _origSaveProd       = saveProd;
const _origDelProd        = delProd;
const _origSaveUser       = saveUser;
const _origSaveConfig     = saveConfig;

// Override to add markDirty (wrap after definition)
function patchedRender(){
  render();
  updateSaveIndicator();
}

// ===================== INIT =====================
const hadSavedData = loadData();
render();
setTimeout(()=>{
  checkStock();
  if(hadSavedData){
    toast("💾 Datos cargados",`Se cargaron tus datos guardados correctamente`,"success",3000);
  }
},600);

// Auto-save on page unload (seguridad extra)
window.addEventListener("beforeunload", ()=>{ if(unsavedChanges) saveData(); });

// Patch mutations to trigger markDirty
// We override the key action functions with wrappers
const _rawSaveGasto = saveGasto;
const _rawSaveProd = saveProd;
const _rawDelProd = delProd;
const _rawSaveUser = saveUser;
const _rawSaveConfig = saveConfig;
const _rawSubmitOrder = submitOrder;
const _rawAdvOrder = advOrder;

// Replace global functions with dirty-marking wrappers
window.submitOrder = function(){_rawSubmitOrder.apply(this,arguments);markDirty();};
window.advOrder    = function(){_rawAdvOrder.apply(this,arguments);markDirty();};
window.saveGasto   = function(){_rawSaveGasto.apply(this,arguments);markDirty();};
window.saveProd    = function(){_rawSaveProd.apply(this,arguments);markDirty();};
window.delProd     = function(){_rawDelProd.apply(this,arguments);markDirty();};
window.saveUser    = function(){_rawSaveUser.apply(this,arguments);markDirty();};
window.saveConfig  = function(){_rawSaveConfig.apply(this,arguments);markDirty();};
</script>
</body>
</html>
