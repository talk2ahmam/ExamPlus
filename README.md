
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>School Registry Portal</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700&family=Public+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@500&display=swap" rel="stylesheet">
  <style>
    /* ============================= TOKENS ============================= */
    :root{
      --ink:#14213D;
      --ink-2:#1F3159;
      --ink-3:#28407A;
      --parchment:#FAF6EE;
      --paper:#FFFFFF;
      --gold:#C9962C;
      --gold-deep:#9C7419;
      --ledger:#DDD0B0;
      --text:#262A31;
      --muted:#6B7280;
      --success:#2F6F4E;
      --success-bg:#E8F1EC;
      --danger:#B23A2E;
      --danger-bg:#FBEAE7;
      --radius:10px;
      --font-display:'Fraunces', Georgia, serif;
      --font-body:'Public Sans', -apple-system, Segoe UI, sans-serif;
      --font-mono:'IBM Plex Mono', monospace;
    }
    *{box-sizing:border-box;}
    html,body{margin:0;padding:0;}
    body{
      background:var(--parchment);
      color:var(--text);
      font-family:var(--font-body);
      font-size:15px;
      line-height:1.5;
      min-height:100vh;
    }
    h1,h2,h3,h4{font-family:var(--font-display); margin:0; color:var(--ink); font-weight:600;}
    a{color:inherit;}
    button{font-family:var(--font-body);}
    :focus-visible{outline:2px solid var(--gold-deep); outline-offset:2px;}
    @media (prefers-reduced-motion: reduce){ *{animation:none !important; transition:none !important;} }

    .hidden{display:none !important;}

    .ledger-rule{
      height:10px; margin:10px 0 20px;
      border-top:2px solid var(--gold);
      position:relative;
    }
    .ledger-rule::after{
      content:""; position:absolute; top:4px; left:0; right:0; height:1px; background:var(--ledger);
    }
    .ledger-rule.tight{margin:6px 0 14px;}

    /* ============================= BUTTONS / FORM ============================= */
    .btn{
      font-family:var(--font-body); font-weight:600; font-size:14px;
      padding:10px 18px; border-radius:8px; border:1px solid transparent;
      cursor:pointer; transition:transform .12s ease, background .15s ease, box-shadow .15s ease;
      display:inline-flex; align-items:center; gap:8px;
    }
    .btn:active{transform:translateY(1px);}
    .btn-primary{background:var(--gold); color:var(--ink);}
    .btn-primary:hover{background:var(--gold-deep); color:#fff;}
    .btn-ghost{background:transparent; border-color:var(--ledger); color:var(--ink);}
    .btn-ghost:hover{background:#F0EAD9;}
    .btn-danger{background:var(--danger-bg); color:var(--danger); border-color:#EFC8C0;}
    .btn-danger:hover{background:var(--danger); color:#fff;}
    .btn-success{background:var(--success-bg); color:var(--success); border-color:#A3C6B5;}
    .btn-success:hover{background:var(--success); color:#fff;}
    .btn-sm{padding:6px 12px; font-size:13px; border-radius:6px;}
    .btn-block{width:100%; justify-content:center;}

    label{display:block; font-size:13px; font-weight:600; color:var(--ink); margin:12px 0 6px;}
    input, select, textarea{
      width:100%; padding:10px 12px; border:1px solid var(--ledger); border-radius:8px;
      font-family:var(--font-body); font-size:14px; background:#fff; color:var(--text);
    }
    textarea{resize:vertical; min-height:80px;}
    .field-row{display:grid; grid-template-columns:1fr 1fr; gap:16px;}
    @media (max-width:640px){.field-row{grid-template-columns:1fr;}}
    .hint{font-size:12px; color:var(--muted); margin-top:4px;}

    .badge{display:inline-block; padding:3px 10px; border-radius:20px; font-size:12px; font-weight:700; letter-spacing:.02em;}
    .badge-success{background:var(--success-bg); color:var(--success);}
    .badge-danger{background:var(--danger-bg); color:var(--danger);}
    .badge-neutral{background:#EEE9DC; color:var(--ink-2);}
    .badge-warning{background:#FFF3CD; color:#856404;}

    /* ============================= LANDING ============================= */
    #landing{
      min-height:100vh; display:flex; flex-direction:column; align-items:center;
      justify-content:center;
      padding:40px 24px;
    }
    .landing-title{
      font-size:38px;
      margin-bottom:32px;
      text-align:center;
      color:var(--ink);
    }
    .role-grid{
      display:grid;
      grid-template-columns:repeat(auto-fit, minmax(200px,1fr));
      gap:18px;
      max-width:900px;
      width:100%;
    }
    .role-card{
      background:var(--paper);
      border:1px solid var(--ledger);
      border-radius:var(--radius);
      padding:22px 18px;
      text-align:center;
      cursor:pointer;
      transition:transform .15s ease, box-shadow .15s ease, border-color .15s ease;
      position:relative;
      overflow:hidden;
    }
    .role-card:hover{transform:translateY(-3px); box-shadow:0 10px 24px rgba(20,33,61,.10); border-color:var(--gold);}
    .role-card::before{
      content:""; position:absolute; left:0; top:0; bottom:0; width:4px; background:var(--gold);
      transform:scaleY(0); transform-origin:bottom; transition:transform .18s ease;
    }
    .role-card:hover::before{transform:scaleY(1);}
    .role-icon{width:34px; height:34px; color:var(--ink); margin-bottom:10px;}
    .role-card h3{font-size:18px; margin:0;}
    .role-card p{color:var(--muted); font-size:13px; margin-top:4px; line-height:1.4;}
    .role-lock{position:absolute; top:14px; right:14px; color:var(--ledger);}

    /* ============================= MODAL ============================= */
    .modal-backdrop{
      position:fixed; inset:0; background:rgba(20,33,61,.55); display:flex; align-items:center; justify-content:center;
      z-index:100; padding:20px;
    }
    .modal{
      background:var(--paper); border-radius:var(--radius); padding:30px; width:100%; max-width:400px;
      box-shadow:0 20px 60px rgba(0,0,0,.3);
    }
    .modal.modal-wide{max-width:700px;}
    .modal-close{
      float:right; background:none; border:none; font-size:20px; cursor:pointer; color:var(--muted); line-height:1;
    }
    .modal-error{color:var(--danger); font-size:13px; margin-top:8px; min-height:16px;}

    /* ============================= DASHBOARD SHELL ============================= */
    .shell{display:flex; min-height:100vh;}
    .sidebar{
      width:220px; background:var(--ink); color:#fff; flex-shrink:0;
      display:flex; flex-direction:column; padding:20px 0;
    }
    .sidebar-brand{padding:0 18px 16px; border-bottom:1px solid rgba(255,255,255,.12); margin-bottom:12px;}
    .sidebar-brand .role-name{font-family:var(--font-display); font-size:18px; font-weight:600;}
    .sidebar-brand .school-name{font-size:11px; color:#B9C2D8; margin-top:2px;}
    .nav-item{
      display:flex; align-items:center; gap:12px; padding:10px 18px; color:#CBD3E6; cursor:pointer;
      font-size:13px; font-weight:500; border-left:3px solid transparent; transition:all .15s ease;
    }
    .nav-item:hover{background:var(--ink-2); color:#fff;}
    .nav-item.active{background:var(--ink-2); color:#fff; border-left-color:var(--gold);}
    .nav-item svg{width:16px; height:16px; flex-shrink:0;}
    .sidebar-footer{margin-top:auto; padding:12px 18px 0;}

    .main{flex:1; min-width:0; display:flex; flex-direction:column;}
    .topbar{
      display:flex; align-items:center; justify-content:space-between; padding:14px 28px;
      border-bottom:1px solid var(--ledger); background:var(--paper);
    }
    .topbar h2{font-size:18px;}
    .content{padding:24px 28px 40px; flex:1;}

    .card{background:var(--paper); border:1px solid var(--ledger); border-radius:var(--radius); padding:20px; margin-bottom:18px;}
    .card-header{display:flex; align-items:center; justify-content:space-between; gap:12px; flex-wrap:wrap;}
    .toolbar{display:flex; gap:10px; align-items:center; flex-wrap:wrap; margin-bottom:6px;}
    .toolbar select, .toolbar input{width:auto; min-width:140px;}

    table{width:100%; border-collapse:collapse; font-size:13px;}
    thead th{
      text-align:left; font-family:var(--font-mono); font-size:10px; letter-spacing:.06em; text-transform:uppercase;
      color:var(--muted); padding:8px 10px; border-bottom:2px solid var(--gold);
    }
    tbody td{padding:8px 10px; border-bottom:1px solid var(--ledger);}
    tbody tr:hover{background:#FBF8F1;}
    .table-wrap{overflow-x:auto;}
    .row-actions{display:flex; gap:6px;}
    .empty-state{text-align:center; padding:30px 16px; color:var(--muted);}
    .empty-state .btn{margin-top:12px;}

    .stat-grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(140px,1fr)); gap:14px; margin-bottom:18px;}
    .stat-card{background:var(--paper); border:1px solid var(--ledger); border-left:4px solid var(--gold); border-radius:8px; padding:14px 16px;}
    .stat-card .num{font-family:var(--font-display); font-size:24px; color:var(--ink);}
    .stat-card .lbl{font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:.05em;}

    /* Report card / print */
    .report-card{background:#fff; border:1px solid var(--ledger); padding:24px; max-width:680px; margin-bottom:20px;}
    .report-head{text-align:center; border-bottom:3px double var(--gold); padding-bottom:12px; margin-bottom:16px;}
    .report-head h2{font-size:20px;}
    .report-meta{display:flex; justify-content:space-between; font-size:12px; color:var(--muted); margin-bottom:14px; flex-wrap:wrap; gap:4px;}
    @media print{
      .sidebar, .topbar, .toolbar, .no-print{display:none !important;}
      .content{padding:0;}
      body{background:#fff;}
      .report-card{border:none; margin-bottom:0; page-break-after:always;}
      .report-card:last-of-type{page-break-after:auto;}
    }

    .banner{
      background:#FFF7E6; border:1px solid #F0DBA6; color:#7A5A0A; padding:10px 16px; border-radius:8px;
      font-size:13px; margin-bottom:16px;
    }
    .banner-success{
      background:#E8F1EC; border-color:#A3C6B5; color:#1F4B36;
    }
    .banner-danger{
      background:#FBEAE7; border-color:#EFC8C0; color:#7A2318;
    }

    @media (max-width:768px){
      .shell{flex-direction:column;}
      .sidebar{width:100%; flex-direction:row; overflow-x:auto; padding:8px 0;}
      .sidebar-brand{display:none;}
      .nav-item{border-left:none; border-bottom:3px solid transparent; white-space:nowrap; padding:8px 14px;}
      .nav-item.active{border-left:none; border-bottom-color:var(--gold);}
      .sidebar-footer{display:none;}
      .content{padding:16px;}
      .topbar{padding:12px 16px;}
      .landing-title{font-size:28px;}
      .role-grid{grid-template-columns:repeat(auto-fit, minmax(160px,1fr));}
    }

    .config-preview {
      background: var(--parchment);
      border-radius: 8px;
      padding: 10px 14px;
      margin-top: 6px;
    }
    .upload-preview {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-top: 4px;
      flex-wrap: wrap;
    }
    .upload-preview img {
      max-height: 50px;
      max-width: 100px;
      object-fit: contain;
      border: 1px solid var(--ledger);
      border-radius: 4px;
      padding: 4px;
      background: white;
    }
    .bulk-score-grid input {
      width: 55px;
      padding: 4px 6px;
      text-align: center;
    }
    .bulk-score-grid td {
      vertical-align: middle;
    }
    .fee-checkbox-group {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;
      padding: 6px 0;
    }
    .fee-checkbox-group label {
      display: flex;
      align-items: center;
      gap: 6px;
      font-weight: normal;
      margin: 0;
      font-size: 13px;
      cursor: pointer;
    }
    .fee-checkbox-group input[type="checkbox"] {
      width: 16px;
      height: 16px;
      cursor: pointer;
    }
    .total-amount-display {
      font-size: 18px;
      font-weight: bold;
      color: var(--gold-deep);
      padding: 6px 0;
    }
    .payment-details-box {
      background: #f8f4ec;
      padding: 10px 14px;
      border-radius: 6px;
      margin: 6px 0;
      border-left: 3px solid var(--gold);
    }
    .payment-details-box p {
      margin: 3px 0;
      font-size: 13px;
    }
    .action-btn-group {
      display: flex;
      gap: 4px;
      flex-wrap: wrap;
    }

    /* POS Receipt Styles */
    .pos-receipt {
      max-width: 300px;
      margin: 0 auto;
      background: #fff;
      padding: 14px;
      font-family: 'Courier New', monospace;
      font-size: 12px;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .pos-receipt .receipt-header {
      text-align: center;
      border-bottom: 1px dashed #999;
      padding-bottom: 6px;
      margin-bottom: 6px;
    }
    .pos-receipt .receipt-header img {
      max-height: 40px;
      margin-bottom: 4px;
    }
    .pos-receipt .receipt-header h3 {
      margin: 0;
      font-size: 14px;
      font-family: 'Courier New', monospace;
    }
    .pos-receipt .receipt-header p {
      margin: 2px 0;
      font-size: 10px;
      color: #555;
    }
    .pos-receipt .receipt-row {
      display: flex;
      justify-content: space-between;
      padding: 2px 0;
      border-bottom: 1px dotted #eee;
    }
    .pos-receipt .receipt-row.total {
      border-top: 2px solid #333;
      border-bottom: none;
      font-weight: bold;
      font-size: 14px;
      padding-top: 4px;
      margin-top: 4px;
    }
    .pos-receipt .receipt-footer {
      text-align: center;
      border-top: 1px dashed #999;
      padding-top: 6px;
      margin-top: 6px;
      font-size: 10px;
      color: #555;
    }
    .pos-receipt .receipt-label {
      font-weight: bold;
    }
    @media print {
      .pos-receipt { border: none; padding: 10px; max-width: 100%; }
    }
  </style>
</head>
<body>

<!-- ============================= LANDING ============================= -->
<div id="landing">
  <h1 class="landing-title"> School  Portal</h1>
  <div class="role-grid">
    <div class="role-card" data-role="superadmin">
      <svg class="role-lock" viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2"><rect x="5" y="11" width="14" height="9" rx="2"></rect><path d="M8 11V8a4 4 0 0 1 8 0v3"></path></svg>
      <svg class="role-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 3l9 4.5-9 4.5-9-4.5L12 3z"></path><path d="M7.5 10v5c0 1.5 2 3 4.5 3s4.5-1.5 4.5-3v-5"></path><path d="M21 8v6"></path></svg>
      <h3>Super Admin</h3>
      <p>Platform owner</p>
    </div>
    <div class="role-card" data-role="admin">
      <svg class="role-lock" viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2"><rect x="5" y="11" width="14" height="9" rx="2"></rect><path d="M8 11V8a4 4 0 0 1 8 0v3"></path></svg>
      <svg class="role-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M3 21V9l9-6 9 6v12"></path><path d="M9 21v-6h6v6"></path></svg>
      <h3>Admin</h3>
      <p>School administrator</p>
    </div>
    <div class="role-card" data-role="bursar">
      <svg class="role-lock" viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2"><rect x="5" y="11" width="14" height="9" rx="2"></rect><path d="M8 11V8a4 4 0 0 1 8 0v3"></path></svg>
      <svg class="role-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M3 7h18M3 12h18M3 17h18" opacity="0"></path><circle cx="12" cy="12" r="9"></circle><path d="M9.5 15s1 1.5 2.5 1.5 2.5-1 2.5-2-1-1.5-2.5-2-2.5-1-2.5-2 1-2 2.5-2 2.5 1.5 2.5 1.5"></path></svg>
      <h3>Bursar</h3>
      <p>Accounts office</p>
    </div>
    <div class="role-card" data-role="teacher">
      <svg class="role-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"></path><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"></path><path d="M9 7h7M9 11h7"></path></svg>
      <h3>Teacher</h3>
      <p>Class desk</p>
    </div>
    <div class="role-card" data-role="signup">
      <svg class="role-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><circle cx="12" cy="8" r="4"></circle><path d="M4 21c0-4 4-6 8-6s8 2 8 6"></path><path d="M19 8h3M20.5 6.5v3"></path></svg>
      <h3>Sign Up</h3>
      <p>Register a new school</p>
    </div>
  </div>
</div>

<!-- ============================= PASSWORD MODAL ============================= -->
<div id="passwordModal" class="modal-backdrop hidden">
  <div class="modal">
    <button class="modal-close" data-close-modal>×</button>
    <h3 id="pwModalTitle">Enter password</h3>
    <div class="ledger-rule tight"></div>
    <form id="pwForm">
      <label for="pwUsername">Username</label>
      <input id="pwUsername" autocomplete="username" required>
      <label for="pwInput">Password</label>
      <input id="pwInput" type="password" autocomplete="current-password" required>
      <div class="modal-error" id="pwError"></div>
      <button type="submit" class="btn btn-primary btn-block" style="margin-top:18px;">Continue</button>
    </form>
  </div>
</div>

<!-- ============================= GENERIC RECORD MODAL ============================= -->
<div id="recordModal" class="modal-backdrop hidden">
  <div class="modal modal-wide">
    <button class="modal-close" data-close-modal>×</button>
    <h3 id="recordModalTitle">Add record</h3>
    <div class="ledger-rule tight"></div>
    <form id="recordForm"></form>
    <div class="modal-error" id="recordError"></div>
    <div style="display:flex; gap:10px; margin-top:18px;">
      <button type="button" class="btn btn-ghost" data-close-modal style="flex:1;">Cancel</button>
      <button type="submit" form="recordForm" class="btn btn-primary" style="flex:1;">Save</button>
    </div>
  </div>
</div>

<!-- ============================= BULK ADD MODAL ============================= -->
<div id="bulkModal" class="modal-backdrop hidden">
  <div class="modal modal-wide">
    <button class="modal-close" data-close-modal>×</button>
    <h3 id="bulkModalTitle">Bulk Add</h3>
    <div class="ledger-rule tight"></div>
    <div id="bulkModalBody"></div>
  </div>
</div>

<!-- ============================= SUPER ADMIN ============================= -->
<div id="dash-superadmin" class="shell hidden">
  <aside class="sidebar">
    <div class="sidebar-brand"><div class="role-name">Super Admin</div><div class="school-name">Platform owner</div></div>
    <div class="nav-item active" data-target="sa-create">Create School</div>
    <div class="nav-item" data-target="sa-view">View Schools</div>
    <div class="nav-item" data-target="sa-payment-settings">Payment Settings</div>
    <div class="nav-item" data-target="sa-registrations">Registrations</div>
    <div class="sidebar-footer"><button class="btn btn-ghost btn-block" data-logout>Log out</button></div>
  </aside>
  <div class="main">
    <div class="topbar"><h2 id="sa-title">Create School</h2></div>
    <div class="content">
      <div id="sa-create" class="panel"></div>
      <div id="sa-view" class="panel hidden"></div>
      <div id="sa-payment-settings" class="panel hidden"></div>
      <div id="sa-registrations" class="panel hidden"></div>
    </div>
  </div>
</div>

<!-- ============================= ADMIN ============================= -->
<div id="dash-admin" class="shell hidden">
  <aside class="sidebar">
    <div class="sidebar-brand"><div class="role-name">Admin</div><div class="school-name" id="admin-school-label">Your school</div></div>
    <div class="nav-item active" data-target="ad-school">School Details</div>
    <div class="nav-item" data-target="ad-students">Students</div>
    <div class="nav-item" data-target="ad-subjects">All Subjects</div>
    <div class="nav-item" data-target="ad-classview">Student / Class</div>
    <div class="nav-item" data-target="ad-files">Student Files</div>
    <div class="nav-item" data-target="ad-reports">Report Sheets</div>
    <div class="nav-item" data-target="ad-assess">Assessment Sheet</div>
    <div class="nav-item" data-target="ad-contact">Contact Us</div>
    <div class="sidebar-footer"><button class="btn btn-ghost btn-block" data-logout>Log out</button></div>
  </aside>
  <div class="main">
    <div class="topbar"><h2 id="admin-title">School Details</h2></div>
    <div class="content">
      <div id="ad-school" class="panel"></div>
      <div id="ad-students" class="panel hidden"></div>
      <div id="ad-subjects" class="panel hidden"></div>
      <div id="ad-classview" class="panel hidden"></div>
      <div id="ad-files" class="panel hidden"></div>
      <div id="ad-reports" class="panel hidden"></div>
      <div id="ad-assess" class="panel hidden"></div>
      <div id="ad-contact" class="panel hidden"></div>
    </div>
  </div>
</div>

<!-- ============================= BURSAR ============================= -->
<div id="dash-bursar" class="shell hidden">
  <aside class="sidebar">
    <div class="sidebar-brand"><div class="role-name">Bursar</div><div class="school-name">Accounts office</div></div>
    <div class="nav-item active" data-target="bu-fee-settings">Fee Settings</div>
    <div class="nav-item" data-target="bu-receipt">Generate Receipt</div>
    <div class="nav-item" data-target="bu-payments">Fee Payments</div>
    <div class="nav-item" data-target="bu-students">Student Accounts</div>
    <div class="sidebar-footer"><button class="btn btn-ghost btn-block" data-logout>Log out</button></div>
  </aside>
  <div class="main">
    <div class="topbar"><h2 id="bursar-title">Fee Settings</h2></div>
    <div class="content">
      <div id="bu-fee-settings" class="panel"></div>
      <div id="bu-receipt" class="panel hidden"></div>
      <div id="bu-payments" class="panel hidden"></div>
      <div id="bu-students" class="panel hidden"></div>
    </div>
  </div>
</div>

<!-- ============================= TEACHER ============================= -->
<div id="dash-teacher" class="shell hidden">
  <aside class="sidebar">
    <div class="sidebar-brand"><div class="role-name">Teacher</div><div class="school-name">Class desk</div></div>
    <div class="nav-item active" data-target="te-upload">Upload Results</div>
    <div class="nav-item" data-target="te-view">View Results</div>
    <div class="sidebar-footer"><button class="btn btn-ghost btn-block" data-logout>Log out</button></div>
  </aside>
  <div class="main">
    <div class="topbar"><h2 id="teacher-title">Upload Results</h2></div>
    <div class="content">
      <div id="te-upload" class="panel"></div>
      <div id="te-view" class="panel hidden"></div>
    </div>
  </div>
</div>

<!-- ============================= SIGNUP / ACTIVATE ============================= -->
<div id="dash-signup" class="shell hidden">
  <aside class="sidebar">
    <div class="sidebar-brand"><div class="role-name">Sign Up</div><div class="school-name">New school</div></div>
    <div class="nav-item active" data-target="su-signup">Sign Up</div>
    <div class="nav-item" data-target="su-activate">Activate Account</div>
    <div class="sidebar-footer"><button class="btn btn-ghost btn-block" data-logout>Back to menu</button></div>
  </aside>
  <div class="main">
    <div class="topbar"><h2 id="signup-title">Sign Up</h2></div>
    <div class="content">
      <div id="su-signup" class="panel"></div>
      <div id="su-activate" class="panel hidden"></div>
    </div>
  </div>
</div>

<script>
/* =====================================================================
   CONFIG — point this at your Apps Script Web App URL once deployed.
   Leave API_URL empty to run in DEMO MODE (data kept in this browser's
   localStorage only, nothing sent anywhere). See SETUP-GUIDE.md.
===================================================================== */
const CONFIG = {
  API_URL: "https://script.google.com/macros/s/AKfycbzWzySdRXJFwyiS7XR0TZyNiLtksVIfVFSOChIGlAqeSqwvDHe8CKqoTd-N0mnjb6YwJA/exec",              // <-- paste your Google Apps Script /exec URL here
  DEMO_PASSWORDS: {         // used only while API_URL is empty
    superadmin: "super123",
    admin: "admin123",
    bursar: "bursar123"
  },
  CLASS_LIST: ["Nursery 1","Nursery 2","Primary 1","Primary 2","Primary 3","Primary 4","Primary 5","Primary 6","JSS1","JSS2","JSS3","SS1","SS2","SS3"],
  TERMS: ["First Term","Second Term","Third Term"]
};

/* =====================================================================
   DATA LAYER — every screen talks to api(action, sheet, payload).
   When CONFIG.API_URL is set, this calls your Google Sheet through the
   Apps Script web app (Code.gs). Otherwise it falls back to a local
   demo store so the whole app is testable with zero setup.
===================================================================== */
async function api(action, sheet, payload = {}) {
  if (CONFIG.API_URL) {
    const res = await fetch(CONFIG.API_URL, {
      method: "POST",
      headers: { "Content-Type": "text/plain;charset=utf-8" }, // avoids CORS preflight on Apps Script
      body: JSON.stringify({ action, sheet, ...payload })
    });
    if (!res.ok) throw new Error("Network error talking to Google Sheet");
    return res.json();
  }
  return demoApi(action, sheet, payload);
}

const DEMO_KEY = "srp_demo_store_v1";
function loadStore() {
  let s = localStorage.getItem(DEMO_KEY);
  if (!s) { seedDemoStore(); s = localStorage.getItem(DEMO_KEY); }
  return JSON.parse(s);
}
function saveStore(store) { localStorage.setItem(DEMO_KEY, JSON.stringify(store)); }
function seedDemoStore() {
  const store = {
    Schools: [{ id: "sch_1", name: "Bright Future Academy", address: "12 Palm Ave, Yola", phone: "08012345678", email: "info@brightfuture.edu.ng", principal: "Mrs. A. Bello", dateCreated: new Date().toISOString().slice(0,10), status: "Active", logoUrl: "", signatureUrl: "", config: { ca1Label: "TEST 1", ca2Label: "TEST 2", examLabel: "EXAM", ca1Weight: 15, ca2Weight: 15, examWeight: 70 } }],
    Students: [
      { id: "st_1", schoolId: "sch_1", name: "Amina Bello", class: "JSS1", gender: "Female", admissionNo: "BFA/24/003", guardianPhone: "08045678901" },
      { id: "st_2", schoolId: "sch_1", name: "Halima Yusuf", class: "JSS1", gender: "Female", admissionNo: "BFA/24/002", guardianPhone: "08034567890" },
      { id: "st_3", schoolId: "sch_1", name: "Musa Ibrahim", class: "JSS1", gender: "Male", admissionNo: "BFA/24/001", guardianPhone: "08023456789" }
    ],
    Subjects: [
      { id: "sub_1", schoolId: "sch_1", name: "Mathematics", class: "JSS1" },
      { id: "sub_2", schoolId: "sch_1", name: "English Language", class: "JSS1" }
    ],
    Results: [],
    Assessments: [],
    Payments: [],
    StudentFiles: [],
    Comments: [],
    Messages: [],
    Users: [],
    FeeSettings: [{ id: "fee_1", schoolId: "sch_1", registration: 5000, schoolFees: 25000, booksFee: 8000 }],
    PaymentSettings: [{ id: "pay_1", accountName: "School Registry Platform", accountNumber: "0123456789", bankName: "First Bank", registrationFee: 15000 }]
  };
  saveStore(store);
}
function uid(prefix) { return prefix + "_" + Date.now().toString(36) + Math.random().toString(36).slice(2,6); }

async function demoApi(action, sheet, payload) {
  await new Promise(r => setTimeout(r, 120));
  const store = loadStore();
  store[sheet] = store[sheet] || [];

  if (action === "list") {
    let rows = store[sheet];
    if (payload.filters) {
      rows = rows.filter(r => Object.entries(payload.filters).every(([k,v]) => !v || String(r[k]) === String(v)));
    }
    return { ok: true, rows };
  }
  if (action === "create") {
    const row = { id: uid(sheet.slice(0,3).toLowerCase()), ...payload.data };
    store[sheet].push(row);
    saveStore(store);
    return { ok: true, row };
  }
  if (action === "update") {
    const idx = store[sheet].findIndex(r => r.id === payload.id);
    if (idx === -1) return { ok: false, error: "Record not found" };
    store[sheet][idx] = { ...store[sheet][idx], ...payload.data };
    saveStore(store);
    return { ok: true, row: store[sheet][idx] };
  }
  if (action === "delete") {
    store[sheet] = store[sheet].filter(r => r.id !== payload.id);
    saveStore(store);
    return { ok: true };
  }
  if (action === "bulkSave") {
    const matchKeys = payload.matchKeys || [];
    const rows = payload.rows || [];
    let created = 0, updated = 0;
    rows.forEach(r => {
      let idx = -1;
      if (matchKeys.length) {
        idx = store[sheet].findIndex(existing => matchKeys.every(k => String(existing[k]) === String(r[k])));
      }
      if (idx >= 0) { store[sheet][idx] = { ...store[sheet][idx], ...r }; updated++; }
      else { store[sheet].push({ id: uid(sheet.slice(0,3).toLowerCase()), ...r }); created++; }
    });
    saveStore(store);
    return { ok: true, created, updated };
  }
  if (action === "signup") {
    const row = { id: uid("usr"), ...payload.data, status: "pending", activationCode: Math.random().toString(36).slice(2,8).toUpperCase() };
    store.Users.push(row);
    saveStore(store);
    return { ok: true, row, note: "Demo mode: activation code shown on screen instead of emailed." };
  }
  if (action === "activate") {
    const idx = store.Users.findIndex(u => u.email === payload.email && u.activationCode === payload.code);
    if (idx === -1) return { ok: false, error: "Email or code not recognized." };
    store.Users[idx].status = "Active";
    saveStore(store);
    return { ok: true };
  }
  if (action === "activateUser") {
    const idx = store.Users.findIndex(u => u.id === payload.id);
    if (idx === -1) return { ok: false, error: "User not found." };
    store.Users[idx].status = "Active";
    saveStore(store);
    return { ok: true, row: store.Users[idx] };
  }
  if (action === "deactivateUser") {
    const idx = store.Users.findIndex(u => u.id === payload.id);
    if (idx === -1) return { ok: false, error: "User not found." };
    store.Users[idx].status = "inactive";
    saveStore(store);
    return { ok: true, row: store.Users[idx] };
  }
  return { ok: false, error: "Unknown action" };
}

/* =====================================================================
   APP STATE + NAVIGATION
===================================================================== */
const state = { role: null, schoolId: "sch_1" };

function showOnly(id) {
  document.querySelectorAll("#landing, .shell").forEach(el => el.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}
function goLanding() { state.role = null; showOnly("landing"); }
document.querySelectorAll("[data-logout]").forEach(b => b.addEventListener("click", goLanding));

document.querySelectorAll(".role-card").forEach(card => {
  card.addEventListener("click", () => {
    const role = card.dataset.role;
    if (role === "teacher") { enterDashboard("teacher"); return; }
    if (role === "signup") { enterDashboard("signup"); return; }
    openPasswordModal(role);
  });
});

function enterDashboard(role) {
  state.role = role;
  showOnly("dash-" + role);
  const initFns = { superadmin: initSuperAdmin, admin: initAdmin, bursar: initBursar, teacher: initTeacher, signup: initSignup };
  initFns[role]();
  wireSidebarNav(role);
}

function wireSidebarNav(role) {
  const shell = document.getElementById("dash-" + role);
  const titleEl = document.getElementById(role === "superadmin" ? "sa-title" : role + "-title");
  shell.querySelectorAll(".nav-item").forEach(item => {
    item.onclick = () => {
      shell.querySelectorAll(".nav-item").forEach(i => i.classList.remove("active"));
      item.classList.add("active");
      shell.querySelectorAll(".panel").forEach(p => p.classList.add("hidden"));
      document.getElementById(item.dataset.target).classList.remove("hidden");
      if (titleEl) titleEl.textContent = item.textContent;
    };
  });
}

/* Password modal */
let pendingRole = null;
function openPasswordModal(role) {
  pendingRole = role;
  const titles = { superadmin: "Super Admin login", admin: "Admin login", bursar: "Bursar login" };
  document.getElementById("pwModalTitle").textContent = titles[role];
  document.getElementById("pwError").textContent = "";
  document.getElementById("pwForm").reset();
  document.getElementById("passwordModal").classList.remove("hidden");
  document.getElementById("pwUsername").focus();
}
document.querySelectorAll("[data-close-modal]").forEach(b => b.addEventListener("click", e => {
  e.target.closest(".modal-backdrop").classList.add("hidden");
}));
document.getElementById("pwForm").addEventListener("submit", async (e) => {
  e.preventDefault();
  const username = document.getElementById("pwUsername").value.trim();
  const password = document.getElementById("pwInput").value;
  const errEl = document.getElementById("pwError");
  errEl.textContent = "";
  try {
    let ok = false;
    if (CONFIG.API_URL) {
      const res = await api("login", "Users", { username, password, role: pendingRole });
      ok = res.ok;
      if (!ok) errEl.textContent = res.error || "Incorrect username or password.";
    } else {
      ok = password === CONFIG.DEMO_PASSWORDS[pendingRole];
      if (!ok) errEl.textContent = `Incorrect password. Demo password: ${CONFIG.DEMO_PASSWORDS[pendingRole]}`;
    }
    if (ok) {
      document.getElementById("passwordModal").classList.add("hidden");
      enterDashboard(pendingRole);
    }
  } catch (err) {
    errEl.textContent = "Could not reach the server. Check your connection.";
  }
});

function gradeBadge(g) {
  if (g === "A" || g === "B") return "success";
  if (g === "F") return "danger";
  return "neutral";
}

function ordinal(n) {
  const s = ["th", "st", "nd", "rd"], v = n % 100;
  return n + (s[(v - 20) % 10] || s[v] || s[0]);
}

function generateAutoComment(avg, position, totalRanked) {
  if (avg === null || avg === undefined) return "No results recorded for this term yet.";
  let remark;
  if (avg >= 70) remark = "Excellent performance. Keep up the outstanding work.";
  else if (avg >= 60) remark = "Very good result. Aim even higher next term.";
  else if (avg >= 50) remark = "Good effort, but there is still room for improvement.";
  else if (avg >= 45) remark = "Fair result. More effort is needed to improve.";
  else if (avg >= 40) remark = "Below average. Needs to work much harder.";
  else remark = "Poor performance. Requires serious attention and extra study.";
  const posText = position ? `Came ${ordinal(position)} out of ${totalRanked} students this term. ` : "";
  return `${posText}${remark}`;
}

async function computeClassStats(schoolId, cls, term, session) {
  const [studentsRes, resultsRes] = await Promise.all([
    api("list", "Students", { filters: { schoolId, class: cls } }),
    api("list", "Results", { filters: { schoolId, class: cls, term, session } })
  ]);
  const students = studentsRes.rows || [];
  const resultsByStudent = {};
  (resultsRes.rows || []).forEach(r => { (resultsByStudent[r.studentId] = resultsByStudent[r.studentId] || []).push(r); });
  const withAvg = students.map(s => {
    const rows = resultsByStudent[s.id] || [];
    const avg = rows.length ? rows.reduce((a, r) => a + Number(r.total || 0), 0) / rows.length : null;
    return { student: s, avg };
  });
  const ranked = withAvg.filter(x => x.avg !== null).sort((a, b) => b.avg - a.avg);
  let lastAvg = null, lastPos = 0;
  ranked.forEach((x, i) => {
    if (x.avg === lastAvg) { x.position = lastPos; }
    else { x.position = i + 1; lastPos = x.position; lastAvg = x.avg; }
  });
  const totalRanked = ranked.length;
  const byId = {};
  withAvg.forEach(x => { byId[x.student.id] = { position: x.position || null, avg: x.avg }; });
  return { students, resultsByStudent, byId, totalRanked };
}

/* =====================================================================
   BULK ADD MODAL
===================================================================== */
function openBulkAddModal({ title, instructions, sheet, staticFilters = {}, extraFields = [], lineFields, onSaved }) {
  document.getElementById("bulkModalTitle").textContent = title;
  const body = document.getElementById("bulkModalBody");
  body.innerHTML = `
    <p class="hint">${instructions}</p>
    ${extraFields.map(f => `<label>${f.label}</label><select data-extra="${f.key}"><option value="">Select ${f.label}…</option>${f.options.map(o => `<option value="${o}">${o}</option>`).join("")}</select>`).join("")}
    <label>One per line — ${lineFields.map(f => f.label).join(", ")}</label>
    <textarea id="bulk-textarea" rows="8" placeholder="${lineFields.map(f => f.label).join(", ")}"></textarea>
    <p class="hint">Separate values on each line with a comma. Only the first field is required.</p>
    <div class="modal-error" id="bulk-error"></div>
    <div style="display:flex; gap:10px; margin-top:16px;">
      <button type="button" class="btn btn-ghost" data-close-modal style="flex:1;">Cancel</button>
      <button type="button" class="btn btn-primary" id="bulk-submit" style="flex:1;">Add All</button>
    </div>`;
  document.getElementById("bulkModal").classList.remove("hidden");
  body.querySelectorAll("[data-close-modal]").forEach(b => b.onclick = () => document.getElementById("bulkModal").classList.add("hidden"));

  body.querySelector("#bulk-submit").onclick = async () => {
    const errEl = body.querySelector("#bulk-error");
    errEl.textContent = "";
    const extras = {};
    extraFields.forEach(f => { extras[f.key] = body.querySelector(`[data-extra="${f.key}"]`).value; });
    if (extraFields.some(f => !extras[f.key])) { errEl.textContent = "Please choose an option above first."; return; }
    const lines = body.querySelector("#bulk-textarea").value.split("\n").map(l => l.trim()).filter(Boolean);
    if (!lines.length) { errEl.textContent = "Paste at least one line first."; return; }
    const rows = lines.map(line => {
      const parts = line.split(",").map(p => p.trim());
      const obj = { ...staticFilters, ...extras };
      lineFields.forEach((f, i) => { obj[f.key] = parts[i] || ""; });
      return obj;
    });
    const btn = body.querySelector("#bulk-submit");
    btn.disabled = true; btn.textContent = "Adding…";
    try {
      await api("bulkSave", sheet, { rows, matchKeys: [] });
      document.getElementById("bulkModal").classList.add("hidden");
      if (onSaved) onSaved();
    } catch (err) {
      errEl.textContent = "Could not add these records. Please try again.";
    } finally {
      btn.disabled = false; btn.textContent = "Add All";
    }
  };
}

/* =====================================================================
   GENERIC CRUD SCREEN
===================================================================== */
async function mountCrudScreen(container, opts) {
  const { sheet, title, subtitle, columns, filters = {}, staticFilters = {}, beforeSave, canEdit = true, canDelete = true, addLabel = "Add New" } = opts;
  container.innerHTML = `
    <div class="card">
      <div class="card-header">
        <div><h3>${title}</h3>${subtitle ? `<p class="hint" style="margin-top:4px;">${subtitle}</p>` : ""}</div>
        <div style="display:flex; gap:10px;">
          ${opts.bulkAdd ? `<button class="btn btn-ghost" id="${sheet}-bulk-btn">${opts.bulkAdd.buttonLabel || "Bulk Add"}</button>` : ""}
          <button class="btn btn-primary" id="${sheet}-add-btn">+ ${addLabel}</button>
        </div>
      </div>
      <div class="ledger-rule"></div>
      <div class="toolbar" id="${sheet}-filters"></div>
      <div class="table-wrap"><div id="${sheet}-table"></div></div>
    </div>`;

  const filterBar = container.querySelector(`#${sheet}-filters`);
  const filterState = {};
  for (const [key, cfg] of Object.entries(filters)) {
    const wrap = document.createElement("div");
    const sel = document.createElement("select");
    sel.innerHTML = `<option value="">All ${cfg.label}</option>` + cfg.options.map(o => `<option value="${o.value}">${o.label}</option>`).join("");
    sel.addEventListener("change", () => { filterState[key] = sel.value; render(); });
    wrap.appendChild(sel);
    filterBar.appendChild(wrap);
  }

  async function render() {
    const tableEl = container.querySelector(`#${sheet}-table`);
    tableEl.innerHTML = `<p class="hint">Loading…</p>`;
    const res = await api("list", sheet, { filters: { ...staticFilters, ...filterState } });
    const rows = res.rows || [];
    if (!rows.length) {
      tableEl.innerHTML = `<div class="empty-state">No records yet. Nothing here till you add one.<br><button class="btn btn-primary" id="${sheet}-empty-add">+ ${addLabel}</button></div>`;
      tableEl.querySelector(`#${sheet}-empty-add`).onclick = () => openRecordForm();
      return;
    }
    const visibleCols = columns.filter(c => c.hideInTable !== true);
    tableEl.innerHTML = `<table><thead><tr>${visibleCols.map(c => `<th>${c.label}</th>`).join("")}<th></th></tr></thead>
      <tbody>${rows.map(r => `<tr data-id="${r.id}">${visibleCols.map(c => `<td>${renderCell(c, r)}</td>`).join("")}
        <td class="row-actions">
          ${canEdit ? `<button class="btn btn-ghost btn-sm" data-edit="${r.id}">Edit</button>` : ""}
          ${canDelete ? `<button class="btn btn-danger btn-sm" data-del="${r.id}">Delete</button>` : ""}
        </td></tr>`).join("")}</tbody></table>`;
    tableEl.querySelectorAll("[data-edit]").forEach(b => b.onclick = () => openRecordForm(rows.find(r => r.id === b.dataset.edit)));
    tableEl.querySelectorAll("[data-del]").forEach(b => b.onclick = async () => {
      if (!confirm("Delete this record? This can't be undone.")) return;
      await api("delete", sheet, { id: b.dataset.del });
      render();
    });
  }

  function renderCell(col, row) {
    let val = row[col.key];
    if (col.type === "ref" && col._optionsMap) val = col._optionsMap[val] || val || "—";
    if (col.type === "badge") return `<span class="badge badge-${col.badgeClass ? col.badgeClass(val) : "neutral"}">${val || "—"}</span>`;
    return val === undefined || val === "" ? "—" : val;
  }

  async function openRecordForm(existing = null) {
    for (const c of columns) {
      if (c.type === "ref") {
        const refRes = await api("list", c.refSheet, { filters: c.refFilter ? c.refFilter() : {} });
        c._options = refRes.rows || [];
        c._optionsMap = Object.fromEntries(c._options.map(o => [o.id, o[c.refLabelKey]]));
      }
    }
    document.getElementById("recordModalTitle").textContent = existing ? `Edit ${title}` : `Add ${title}`;
    document.getElementById("recordError").textContent = "";
    const form = document.getElementById("recordForm");
    form.innerHTML = columns.filter(c => !c.readonly).map(c => fieldHtml(c, existing)).join("");
    document.getElementById("recordModal").classList.remove("hidden");
    form.onsubmit = async (e) => {
      e.preventDefault();
      const data = {};
      columns.filter(c => !c.readonly).forEach(c => {
        const el = form.querySelector(`[name="${c.key}"]`);
        data[c.key] = c.type === "number" ? Number(el.value) : el.value;
      });
      Object.assign(data, staticFilters);
      if (beforeSave) Object.assign(data, beforeSave(data) || {});
      try {
        if (existing) await api("update", sheet, { id: existing.id, data });
        else await api("create", sheet, { data });
        document.getElementById("recordModal").classList.add("hidden");
        render();
      } catch (err) {
        document.getElementById("recordError").textContent = "Could not save. Please try again.";
      }
    };
  }

  function fieldHtml(c, existing) {
    const val = existing ? (existing[c.key] ?? "") : (c.default ?? "");
    if (c.type === "select") {
      return `<label>${c.label}</label><select name="${c.key}" ${c.required ? "required" : ""}>
        <option value="">Select ${c.label}</option>
        ${c.options.map(o => `<option value="${o}" ${o === val ? "selected" : ""}>${o}</option>`).join("")}
      </select>`;
    }
    if (c.type === "ref") {
      return `<label>${c.label}</label><select name="${c.key}" ${c.required ? "required" : ""}>
        <option value="">Select ${c.label}</option>
        ${(c._options || []).map(o => `<option value="${o.id}" ${o.id === val ? "selected" : ""}>${o[c.refLabelKey]}</option>`).join("")}
      </select>`;
    }
    if (c.type === "textarea") {
      return `<label>${c.label}</label><textarea name="${c.key}" ${c.required ? "required" : ""}>${val}</textarea>`;
    }
    return `<label>${c.label}</label><input name="${c.key}" type="${c.type === "number" ? "number" : (c.inputType || "text")}" value="${val}" ${c.required ? "required" : ""} ${c.max !== undefined ? `max="${c.max}"` : ""} ${c.min !== undefined ? `min="${c.min}"` : ""}>`;
  }

  container.querySelector(`#${sheet}-add-btn`).onclick = () => openRecordForm();
  if (opts.bulkAdd) {
    container.querySelector(`#${sheet}-bulk-btn`).onclick = () => openBulkAddModal({
      ...opts.bulkAdd, sheet, staticFilters, onSaved: render
    });
  }
  render();
}

/* =====================================================================
   SUPER ADMIN SCREENS
===================================================================== */
function initSuperAdmin() {
  mountCrudScreen(document.getElementById("sa-create"), {
    sheet: "Schools", title: "Schools", addLabel: "New School",
    subtitle: "Every school your platform hosts. Add one to get it started.",
    columns: [
      { key: "name", label: "School Name", required: true },
      { key: "address", label: "Address" },
      { key: "phone", label: "Phone", required: true },
      { key: "email", label: "Email", inputType: "email" },
      { key: "principal", label: "Principal / Head" },
      { key: "status", label: "Status", type: "select", options: ["Active","Inactive","Pending"], default: "Pending", hideInTable: false }
    ],
    beforeSave: (data) => ({ dateCreated: data.dateCreated || new Date().toISOString().slice(0,10), status: data.status || "Pending" })
  });
  
  mountCrudScreen(document.getElementById("sa-view"), {
    sheet: "Schools", title: "All Schools", addLabel: "New School",
    subtitle: "Browse, edit or deactivate any school on the platform.",
    columns: [
      { key: "name", label: "School Name", required: true },
      { key: "phone", label: "Phone", required: true },
      { key: "email", label: "Email", inputType: "email" },
      { key: "principal", label: "Principal / Head" },
      { key: "status", label: "Status", type: "select", options: ["Active","Inactive","Pending"], default: "Pending" }
    ]
  });

  renderPaymentSettings(document.getElementById("sa-payment-settings"));
  renderRegistrations(document.getElementById("sa-registrations"));
}

/* ---- Payment Settings (Super Admin) ---- */
async function renderPaymentSettings(container) {
  container.innerHTML = `<div class="card"><p class="hint">Loading payment settings…</p></div>`;
  
  const res = await api("list", "PaymentSettings", { filters: {} });
  let settings = (res.rows || [])[0];
  
  if (!settings) {
    const createRes = await api("create", "PaymentSettings", {
      data: { accountName: "", accountNumber: "", bankName: "", registrationFee: 0 }
    });
    settings = createRes.row;
  }

  container.innerHTML = `
    <div class="card" style="max-width:640px;">
      <h3>Payment Settings</h3>
      <p class="hint" style="margin-top:4px;">Set the bank account details and registration fee for school signups.</p>
      <div class="ledger-rule"></div>
      <form id="payment-settings-form">
        <div class="field-row">
          <div>
            <label>Account Name</label>
            <input name="accountName" value="${settings.accountName || ""}" required>
          </div>
          <div>
            <label>Account Number</label>
            <input name="accountNumber" value="${settings.accountNumber || ""}" required>
          </div>
        </div>
        <div class="field-row">
          <div>
            <label>Bank Name</label>
            <input name="bankName" value="${settings.bankName || ""}" required>
          </div>
          <div>
            <label>Registration Fee (₦)</label>
            <input name="registrationFee" type="number" min="0" step="100" value="${settings.registrationFee || 0}" required>
          </div>
        </div>
        <button class="btn btn-primary" style="margin-top:16px;">Save Payment Settings</button>
        <span class="hint" id="payment-settings-msg" style="margin-left:12px;"></span>
        <div class="modal-error" id="payment-settings-error"></div>
      </form>
    </div>
  `;

  const form = container.querySelector("#payment-settings-form");
  const msgEl = container.querySelector("#payment-settings-msg");
  const errorEl = container.querySelector("#payment-settings-error");

  form.onsubmit = async (e) => {
    e.preventDefault();
    errorEl.textContent = "";
    const data = Object.fromEntries(new FormData(e.target).entries());
    
    try {
      await api("update", "PaymentSettings", { id: settings.id, data });
      msgEl.textContent = "Payment settings saved successfully!";
      msgEl.style.color = "var(--success)";
    } catch (err) {
      errorEl.textContent = "Could not save settings. Please try again.";
    }
  };
}

/* ---- Registrations with Activate/Deactivate (Super Admin) ---- */
async function renderRegistrations(container) {
  container.innerHTML = `
    <div class="card">
      <div class="card-header">
        <div><h3>School Registrations</h3><p class="hint" style="margin-top:4px;">View all school registration requests. Click "Activate" to confirm payment and activate a school, or "Deactivate" to suspend access.</p></div>
      </div>
      <div class="ledger-rule"></div>
      <div class="table-wrap"><div id="registrations-table"><p class="hint">Loading…</p></div></div>
    </div>
  `;

  async function loadRegistrations() {
    const tableEl = container.querySelector("#registrations-table");
    const res = await api("list", "Users", { filters: {} });
    const rows = res.rows || [];
    
    if (!rows.length) {
      tableEl.innerHTML = `<div class="empty-state">No registrations yet.</div>`;
      return;
    }

    tableEl.innerHTML = `
      <table>
        <thead>
          <tr>
            <th>School</th>
            <th>Admin</th>
            <th>Email</th>
            <th>Phone</th>
            <th>Status</th>
            <th>Activation Code</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          ${rows.map(r => `
            <tr data-id="${r.id}">
              <td>${r.schoolName || "—"}</td>
              <td>${r.adminName || "—"}</td>
              <td>${r.email || "—"}</td>
              <td>${r.phone || "—"}</td>
              <td><span class="badge ${r.status === 'Active' ? 'badge-success' : r.status === 'inactive' ? 'badge-danger' : 'badge-warning'}">${r.status || 'pending'}</span></td>
              <td><code style="background:#f0ebe0; padding:2px 8px; border-radius:4px; font-size:12px;">${r.activationCode || "—"}</code></td>
              <td>
                <div class="action-btn-group">
                  ${r.status !== 'Active' ? `<button class="btn btn-success btn-sm" onclick="activateUser('${r.id}')">✅ Activate</button>` : `<button class="btn btn-danger btn-sm" onclick="deactivateUser('${r.id}')">⛔ Deactivate</button>`}
                </div>
              </td>
            </tr>
          `).join("")}
        </tbody>
      </table>
    `;
  }

  window.activateUser = async function(userId) {
    if (!confirm("Confirm that payment has been received and you want to activate this school?")) return;
    const result = await api("activateUser", "Users", { id: userId });
    if (result.ok) {
      alert("School activated successfully! The school can now log in.");
      loadRegistrations();
    } else {
      alert("Could not activate: " + (result.error || "Unknown error"));
    }
  };

  window.deactivateUser = async function(userId) {
    if (!confirm("Are you sure you want to deactivate this school? They will lose access until reactivated.")) return;
    const result = await api("deactivateUser", "Users", { id: userId });
    if (result.ok) {
      alert("School deactivated successfully. They can be reactivated later.");
      loadRegistrations();
    } else {
      alert("Could not deactivate: " + (result.error || "Unknown error"));
    }
  };

  loadRegistrations();
}

/* =====================================================================
   ADMIN SCREENS
===================================================================== */
function initAdmin() {
  const schoolId = state.schoolId;

  renderSchoolDetails(document.getElementById("ad-school"), schoolId);

  mountCrudScreen(document.getElementById("ad-students"), {
    sheet: "Students", title: "Students", addLabel: "New Student",
    staticFilters: { schoolId },
    columns: [
      { key: "name", label: "Full Name", required: true },
      { key: "admissionNo", label: "Admission No." },
      { key: "class", label: "Class", type: "select", options: CONFIG.CLASS_LIST, required: true },
      { key: "gender", label: "Gender", type: "select", options: ["Male","Female"] },
      { key: "guardianPhone", label: "Guardian Phone" }
    ],
    bulkAdd: {
      buttonLabel: "Bulk Add",
      title: "Bulk Add Students",
      instructions: "Choose the class these students belong to, then paste one student per line.",
      extraFields: [{ key: "class", label: "Class", options: CONFIG.CLASS_LIST }],
      lineFields: [
        { key: "name", label: "Full Name" },
        { key: "admissionNo", label: "Admission No." },
        { key: "gender", label: "Gender" },
        { key: "guardianPhone", label: "Guardian Phone" }
      ]
    }
  });

  mountCrudScreen(document.getElementById("ad-subjects"), {
    sheet: "Subjects", title: "All Subjects", addLabel: "New Subject",
    staticFilters: { schoolId },
    columns: [
      { key: "name", label: "Subject Name", required: true },
      { key: "class", label: "Class", type: "select", options: CONFIG.CLASS_LIST, required: true }
    ],
    bulkAdd: {
      buttonLabel: "Bulk Add",
      title: "Bulk Add Subjects",
      instructions: "Choose the class these subjects apply to, then paste one subject name per line.",
      extraFields: [{ key: "class", label: "Class", options: CONFIG.CLASS_LIST }],
      lineFields: [{ key: "name", label: "Subject Name" }]
    }
  });

  renderClassView(document.getElementById("ad-classview"), schoolId);
  renderStudentFiles(document.getElementById("ad-files"), schoolId);
  renderReportSheets(document.getElementById("ad-reports"), schoolId);
  renderAssessmentSheet(document.getElementById("ad-assess"), schoolId);
  renderContactUs(document.getElementById("ad-contact"));
}

/* ---- helpers for image upload (data URL) ---- */
function fileToDataURL(file) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = (e) => resolve(e.target.result);
    reader.readAsDataURL(file);
  });
}

async function fetchSchoolConfig(schoolId) {
  const res = await api("list", "Schools", { filters: { id: schoolId } });
  const school = (res.rows || [])[0] || {};
  const cfg = school.config || { ca1Label: "TEST 1", ca2Label: "TEST 2", examLabel: "EXAM", ca1Weight: 20, ca2Weight: 20, examWeight: 60 };
  return { school, cfg };
}

function getMaxScores(cfg) {
  return {
    ca1Max: Number(cfg.ca1Weight) || 20,
    ca2Max: Number(cfg.ca2Weight) || 20,
    examMax: Number(cfg.examWeight) || 60
  };
}

async function renderSchoolDetails(container, schoolId) {
  container.innerHTML = `<div class="card"><p class="hint">Loading…</p></div>`;
  const { school, cfg } = await fetchSchoolConfig(schoolId);
  container.innerHTML = `
    <div class="card" style="max-width:640px;">
      <h3>School Details</h3>
      <p class="hint" style="margin-top:4px;">These details appear on report sheets and receipts.</p>
      <div class="ledger-rule"></div>
      <form id="school-details-form" enctype="multipart/form-data">
        <label>School Name</label><input name="name" value="${school.name || ""}" required>
        <div class="field-row">
          <div><label>Phone</label><input name="phone" value="${school.phone || ""}"></div>
          <div><label>Email</label><input name="email" type="email" value="${school.email || ""}"></div>
        </div>
        <label>Address</label><input name="address" value="${school.address || ""}">

        <div class="ledger-rule"></div>
        <h4>Assessment Configuration</h4>
        <div class="field-row">
          <div><label>CA1 Label</label><input name="config.ca1Label" value="${cfg.ca1Label || "TEST 1"}"></div>
          <div><label>CA1 Max Score</label><input name="config.ca1Weight" type="number" min="1" value="${cfg.ca1Weight || 20}"></div>
        </div>
        <div class="field-row">
          <div><label>CA2 Label</label><input name="config.ca2Label" value="${cfg.ca2Label || "TEST 2"}"></div>
          <div><label>CA2 Max Score</label><input name="config.ca2Weight" type="number" min="1" value="${cfg.ca2Weight || 20}"></div>
        </div>
        <div class="field-row">
          <div><label>Exam Label</label><input name="config.examLabel" value="${cfg.examLabel || "EXAM"}"></div>
          <div><label>Exam Max Score</label><input name="config.examWeight" type="number" min="1" value="${cfg.examWeight || 60}"></div>
        </div>
        <div class="config-preview">
          <strong>Preview:</strong> <span id="preview-labels">${cfg.ca1Label || "TEST 1"} (${cfg.ca1Weight || 20}), ${cfg.ca2Label || "TEST 2"} (${cfg.ca2Weight || 20}), ${cfg.examLabel || "EXAM"} (${cfg.examWeight || 60})</span>
        </div>

        <div class="ledger-rule"></div>
        <label>School Logo (upload image)</label>
        <input type="file" id="logoFile" accept="image/*" style="padding:6px;">
        <div class="upload-preview">
          ${school.logoUrl ? `<img src="${school.logoUrl}" alt="Logo preview" id="logoPreview">` : '<span class="hint">No logo uploaded</span>'}
          <span class="hint" id="logoFileName"></span>
        </div>

        <label>Headteacher / Principal Name</label><input name="principal" value="${school.principal || ""}">

        <label>Headteacher Signature (upload image)</label>
        <input type="file" id="signatureFile" accept="image/*" style="padding:6px;">
        <div class="upload-preview">
          ${school.signatureUrl ? `<img src="${school.signatureUrl}" alt="Signature preview" id="signaturePreview">` : '<span class="hint">No signature uploaded</span>'}
          <span class="hint" id="signatureFileName"></span>
        </div>
        <p class="hint">Upload logo and signature images from your gallery.</p>
        <button class="btn btn-primary" style="margin-top:18px;">Save Changes</button>
        <span class="hint" id="school-save-msg" style="margin-left:12px;"></span>
      </form>
    </div>`;

  const form = container.querySelector("#school-details-form");
  const previewEl = container.querySelector("#preview-labels");
  form.querySelectorAll("[name^='config.']").forEach(inp => {
    inp.addEventListener("input", () => {
      const ca1L = form.querySelector("[name='config.ca1Label']").value || "TEST 1";
      const ca2L = form.querySelector("[name='config.ca2Label']").value || "TEST 2";
      const examL = form.querySelector("[name='config.examLabel']").value || "EXAM";
      const ca1W = form.querySelector("[name='config.ca1Weight']").value || 20;
      const ca2W = form.querySelector("[name='config.ca2Weight']").value || 20;
      const examW = form.querySelector("[name='config.examWeight']").value || 60;
      previewEl.textContent = `${ca1L} (${ca1W}), ${ca2L} (${ca2W}), ${examL} (${examW})`;
    });
  });

  const logoInput = container.querySelector("#logoFile");
  const sigInput = container.querySelector("#signatureFile");
  const logoName = container.querySelector("#logoFileName");
  const sigName = container.querySelector("#signatureFileName");

  logoInput.addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (file) {
      logoName.textContent = file.name;
      const reader = new FileReader();
      reader.onload = (ev) => {
        const img = container.querySelector("#logoPreview") || document.createElement("img");
        img.id = "logoPreview";
        img.src = ev.target.result;
        img.alt = "Logo preview";
        img.style.maxHeight = "50px";
        img.style.maxWidth = "100px";
        img.style.objectFit = "contain";
        img.style.border = "1px solid var(--ledger)";
        img.style.borderRadius = "4px";
        img.style.padding = "4px";
        img.style.background = "white";
        const parent = logoInput.parentElement.querySelector(".upload-preview");
        const oldImg = parent.querySelector("#logoPreview");
        if (oldImg) oldImg.remove();
        const span = parent.querySelector(".hint:last-child");
        parent.insertBefore(img, span);
      };
      reader.readAsDataURL(file);
    }
  });

  sigInput.addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (file) {
      sigName.textContent = file.name;
      const reader = new FileReader();
      reader.onload = (ev) => {
        const img = container.querySelector("#signaturePreview") || document.createElement("img");
        img.id = "signaturePreview";
        img.src = ev.target.result;
        img.alt = "Signature preview";
        img.style.maxHeight = "50px";
        img.style.maxWidth = "100px";
        img.style.objectFit = "contain";
        img.style.border = "1px solid var(--ledger)";
        img.style.borderRadius = "4px";
        img.style.padding = "4px";
        img.style.background = "white";
        const parent = sigInput.parentElement.querySelector(".upload-preview");
        const oldImg = parent.querySelector("#signaturePreview");
        if (oldImg) oldImg.remove();
        const span = parent.querySelector(".hint:last-child");
        parent.insertBefore(img, span);
      };
      reader.readAsDataURL(file);
    }
  });

  form.onsubmit = async (e) => {
    e.preventDefault();
    const data = {};
    const fd = new FormData(e.target);
    for (let [key, val] of fd.entries()) {
      if (key.startsWith("config.")) {
        const cfgKey = key.replace("config.", "");
        if (!data.config) data.config = { ...school.config };
        data.config[cfgKey] = isNaN(Number(val)) ? val : Number(val);
      } else if (key !== "logoFile" && key !== "signatureFile") {
        data[key] = val;
      }
    }

    const logoFile = container.querySelector("#logoFile").files[0];
    const sigFile = container.querySelector("#signatureFile").files[0];
    if (logoFile) {
      data.logoUrl = await fileToDataURL(logoFile);
    } else if (school.logoUrl) {
      data.logoUrl = school.logoUrl;
    }
    if (sigFile) {
      data.signatureUrl = await fileToDataURL(sigFile);
    } else if (school.signatureUrl) {
      data.signatureUrl = school.signatureUrl;
    }

    await api("update", "Schools", { id: schoolId, data });
    container.querySelector("#school-save-msg").textContent = "Saved.";
  };
}

async function renderClassView(container, schoolId) {
  container.innerHTML = `
    <div class="card">
      <h3>Student / Class</h3>
      <p class="hint" style="margin-top:4px;">Pick a class to see who's in it.</p>
      <div class="ledger-rule"></div>
      <div class="toolbar"><select id="cv-class"><option value="">Select class…</option>${CONFIG.CLASS_LIST.map(c => `<option value="${c}">${c}</option>`).join("")}</select></div>
      <div class="table-wrap"><div id="cv-table"><p class="hint">Choose a class above to list its students.</p></div></div>
    </div>`;
  container.querySelector("#cv-class").onchange = async (e) => {
    const cls = e.target.value;
    const tableEl = container.querySelector("#cv-table");
    if (!cls) { tableEl.innerHTML = `<p class="hint">Choose a class above to list its students.</p>`; return; }
    tableEl.innerHTML = `<p class="hint">Loading…</p>`;
    const res = await api("list", "Students", { filters: { schoolId, class: cls } });
    const rows = res.rows || [];
    tableEl.innerHTML = rows.length ? `<table><thead><tr><th>Name</th><th>Admission No.</th><th>Gender</th><th>Guardian Phone</th></tr></thead>
      <tbody>${rows.map(r => `<tr><td>${r.name}</td><td>${r.admissionNo || "—"}</td><td>${r.gender || "—"}</td><td>${r.guardianPhone || "—"}</td></tr>`).join("")}</tbody></table>`
      : `<div class="empty-state">No students in ${cls} yet.</div>`;
  };
}

function renderStudentFiles(container, schoolId) {
  mountCrudScreen(container, {
    sheet: "StudentFiles", title: "Student Files", addLabel: "Add File Link",
    subtitle: "Store a shareable link (e.g. a Google Drive file) against a student's record.",
    staticFilters: { schoolId },
    columns: [
      { key: "studentId", label: "Student", type: "ref", refSheet: "Students", refLabelKey: "name", refFilter: () => ({ schoolId }), required: true },
      { key: "fileName", label: "File Name", required: true },
      { key: "fileUrl", label: "File Link (URL)", required: true }
    ]
  });
}

function buildReportCardHtml(student, results, subjMap, school, term, session, extra = {}) {
  const avgNum = results.length ? results.reduce((a, r) => a + Number(r.total || 0), 0) / results.length : null;
  const avgDisplay = avgNum !== null ? avgNum.toFixed(1) : "—";
  const { resumptionDate = "", position = null, totalRanked = 0, assessments = [] } = extra;
  const comment = generateAutoComment(avgNum, position, totalRanked);
  const signatureBlock = school.signatureUrl
    ? `<img src="${school.signatureUrl}" alt="Headteacher signature" style="max-height:40px; max-width:160px; object-fit:contain;">`
    : `<div style="height:36px; border-bottom:1px solid var(--ledger); width:160px;"></div>`;

  const cfg = school.config || { ca1Label: "TEST 1", ca2Label: "TEST 2", examLabel: "EXAM", ca1Weight: 20, ca2Weight: 20, examWeight: 60 };
  const ca1Label = cfg.ca1Label || "TEST 1";
  const ca2Label = cfg.ca2Label || "TEST 2";
  const examLabel = cfg.examLabel || "EXAM";

  return `
    <div class="report-card">
      <div class="report-head">
        ${school.logoUrl ? `<img src="${school.logoUrl}" alt="School logo" style="height:50px; margin-bottom:6px;">` : ""}
        <h2>${school.name || "School"}</h2>
        <div class="hint">${school.address || ""}</div>
      </div>
      <div class="report-meta">
        <span><strong>Student:</strong> ${student ? student.name : ""}</span>
        <span><strong>Class:</strong> ${student ? student.class : ""}</span>
        <span><strong>Term:</strong> ${term}</span>
        <span><strong>Session:</strong> ${session}</span>
      </div>
      <h4 style="margin-bottom:6px;">Academic Results</h4>
      <table><thead><tr><th>Subject</th><th>${ca1Label}</th><th>${ca2Label}</th><th>${examLabel}</th><th>Total</th><th>Grade</th></tr></thead>
      <tbody>${results.length ? results.map(r => `<tr><td>${subjMap[r.subjectId] || r.subjectId}</td><td>${r.ca1 ?? "—"}</td><td>${r.ca2 ?? "—"}</td><td>${r.exam ?? "—"}</td><td>${r.total ?? "—"}</td><td>${r.grade ?? "—"}</td></tr>`).join("") : `<tr><td colspan="6" class="hint">No results recorded for this term yet.</td></tr>`}</tbody></table>
      <div class="report-meta" style="margin-top:8px;">
        <span><strong>Average:</strong> ${avgDisplay}${avgNum !== null ? "%" : ""}</span>
        <span><strong>Position in Class:</strong> ${position ? `${ordinal(position)} of ${totalRanked}` : "—"}</span>
      </div>
      ${assessments.length ? `
      <h4 style="margin:14px 0 6px;">Assessment</h4>
      <table><thead><tr><th>Category</th><th>Score (1–5)</th></tr></thead>
      <tbody>${assessments.map(a => `<tr><td>${a.category}</td><td>${a.score}</td></tr>`).join("")}</tbody></table>` : ""}

      <div class="ledger-rule tight"></div>
      <h4 style="margin-bottom:4px;">Class Teacher's Comment</h4>
      <p style="min-height:18px;">${comment}</p>

      <div style="display:flex; justify-content:space-between; align-items:flex-end; margin-top:20px; gap:16px; flex-wrap:wrap;">
        <div>
          <p class="hint" style="margin-bottom:2px;">Next Term Resumption Date</p>
          <p style="font-weight:600;">${resumptionDate || " "}</p>
        </div>
        <div style="text-align:center;">
          ${signatureBlock}
          <p class="hint" style="margin-top:2px;">${school.principal || "Headteacher"}'s Signature</p>
        </div>
      </div>
    </div>`;
}

async function renderReportSheets(container, schoolId) {
  container.innerHTML = `
    <div class="card no-print">
      <h3>Report Sheets</h3>
      <p class="hint" style="margin-top:4px;">Generate one student's report, or a whole class's report cards at once.</p>
      <div class="ledger-rule"></div>
      <div class="toolbar">
        <select id="rp-mode">
          <option value="single">Single Student</option>
          <option value="class">Whole Class</option>
        </select>
        <select id="rp-student"><option value="">Select student…</option></select>
        <select id="rp-class" class="hidden"><option value="">Select class…</option>${CONFIG.CLASS_LIST.map(c => `<option value="${c}">${c}</option>`).join("")}</select>
        <select id="rp-term">${CONFIG.TERMS.map(t => `<option value="${t}">${t}</option>`).join("")}</select>
        <input id="rp-session" placeholder="Session e.g. 2025/2026" value="2025/2026">
        <span class="hint" style="align-self:center; white-space:nowrap;">Resumption date:</span>
        <input id="rp-resumption" type="date">
        <button class="btn btn-primary" id="rp-generate">Generate</button>
      </div>
    </div>
    <div id="rp-output"></div>`;

  const studentsRes = await api("list", "Students", { filters: { schoolId } });
  const allStudents = studentsRes.rows || [];
  const studentSel = container.querySelector("#rp-student");
  const classSel = container.querySelector("#rp-class");
  const modeSel = container.querySelector("#rp-mode");
  studentSel.innerHTML += allStudents.map(s => `<option value="${s.id}">${s.name} — ${s.class}</option>`).join("");

  modeSel.addEventListener("change", () => {
    const isClass = modeSel.value === "class";
    studentSel.classList.toggle("hidden", isClass);
    classSel.classList.toggle("hidden", !isClass);
  });

  container.querySelector("#rp-generate").addEventListener("click", async () => {
    const term = container.querySelector("#rp-term").value;
    const session = container.querySelector("#rp-session").value;
    const resumptionDate = container.querySelector("#rp-resumption").value;
    const out = container.querySelector("#rp-output");

    if (modeSel.value === "single") {
      const studentId = studentSel.value;
      if (!studentId) { out.innerHTML = `<p class="hint no-print">Choose a student first.</p>`; return; }
      out.innerHTML = `<p class="hint">Loading…</p>`;
      const student = allStudents.find(s => s.id === studentId);
      const [stats, assessRes, subjectsRes, schoolRes] = await Promise.all([
        computeClassStats(schoolId, student.class, term, session),
        api("list", "Assessments", { filters: { studentId, term, session } }),
        api("list", "Subjects", { filters: { schoolId } }),
        api("list", "Schools", { filters: { id: schoolId } })
      ]);
      const subjMap = Object.fromEntries((subjectsRes.rows || []).map(s => [s.id, s.name]));
      const school = (schoolRes.rows || [])[0] || {};
      const studentStat = stats.byId[studentId] || {};
      out.innerHTML = `
        <div class="no-print" style="margin-bottom:14px;"><button class="btn btn-primary" id="rp-print">Print Report</button></div>
        ${buildReportCardHtml(student, stats.resultsByStudent[studentId] || [], subjMap, school, term, session, {
          resumptionDate, position: studentStat.position, totalRanked: stats.totalRanked, assessments: assessRes.rows || []
        })}`;
      out.querySelector("#rp-print").addEventListener("click", () => window.print());
      return;
    }

    const cls = classSel.value;
    if (!cls) { out.innerHTML = `<p class="hint no-print">Choose a class first.</p>`; return; }
    out.innerHTML = `<p class="hint">Loading…</p>`;
    const [stats, assessRes, subjectsRes, schoolRes] = await Promise.all([
      computeClassStats(schoolId, cls, term, session),
      api("list", "Assessments", { filters: { schoolId, class: cls, term, session } }),
      api("list", "Subjects", { filters: { schoolId } }),
      api("list", "Schools", { filters: { id: schoolId } })
    ]);
    if (!stats.students.length) {
      out.innerHTML = `<div class="card no-print"><div class="empty-state">No students in ${cls} yet.</div></div>`;
      return;
    }
    const subjMap = Object.fromEntries((subjectsRes.rows || []).map(s => [s.id, s.name]));
    const school = (schoolRes.rows || [])[0] || {};
    const assessByStudent = {};
    (assessRes.rows || []).forEach(a => { (assessByStudent[a.studentId] = assessByStudent[a.studentId] || []).push(a); });

    const cardsHtml = stats.students.map(student => {
      const studentStat = stats.byId[student.id] || {};
      return buildReportCardHtml(student, stats.resultsByStudent[student.id] || [], subjMap, school, term, session, {
        resumptionDate, position: studentStat.position, totalRanked: stats.totalRanked, assessments: assessByStudent[student.id] || []
      });
    }).join("");
    out.innerHTML = `
      <div class="no-print" style="margin-bottom:14px;">
        <button class="btn btn-primary" id="rp-print-all">Print All ${stats.students.length} Report Cards</button>
        <span class="hint" style="margin-left:8px;">${cls} · ${term} · ${session}</span>
      </div>
      ${cardsHtml}`;
    out.querySelector("#rp-print-all").addEventListener("click", () => window.print());
  });
}

function renderAssessmentSheet(container, schoolId) {
  mountCrudScreen(container, {
    sheet: "Assessments", title: "Assessment Sheet", addLabel: "New Assessment",
    subtitle: "Affective and psychomotor scores (1–5) per student, per term.",
    staticFilters: { schoolId },
    columns: [
      { key: "studentId", label: "Student", type: "ref", refSheet: "Students", refLabelKey: "name", refFilter: () => ({ schoolId }), required: true },
      { key: "category", label: "Category", type: "select", options: ["Punctuality","Neatness","Honesty","Leadership","Sports","Cooperation","Attentiveness"], required: true },
      { key: "term", label: "Term", type: "select", options: CONFIG.TERMS, required: true },
      { key: "session", label: "Session", default: "2025/2026", required: true },
      { key: "score", label: "Score (1–5)", type: "number", min: 1, max: 5, required: true }
    ]
  });
}

async function renderContactUs(container) {
  container.innerHTML = `
    <div class="card" style="max-width:640px;">
      <h3>Contact Us</h3>
      <p class="hint" style="margin-top:4px;">Send a note to the platform team.</p>
      <div class="ledger-rule"></div>
      <form id="contact-form">
        <label>Your Name</label><input name="name" required>
        <label>Email</label><input name="email" type="email" required>
        <label>Message</label><textarea name="message" required></textarea>
        <button class="btn btn-primary" style="margin-top:16px;">Send Message</button>
        <span class="hint" id="contact-msg" style="margin-left:10px;"></span>
      </form>
    </div>
    <div class="card" style="max-width:640px;">
      <h4>Other ways to reach us</h4>
      <p class="hint" style="margin-top:6px;">Phone: 0800-000-0000  ·  Email: support@schoolregistry.ng</p>
    </div>`;
  container.querySelector("#contact-form").onsubmit = async (e) => {
    e.preventDefault();
    const data = Object.fromEntries(new FormData(e.target).entries());
    await api("create", "Messages", { data: { ...data, date: new Date().toISOString().slice(0,10) } });
    e.target.reset();
    container.querySelector("#contact-msg").textContent = "Message sent — thank you.";
  };
}

/* =====================================================================
   BURSAR SCREENS
===================================================================== */
function initBursar() {
  const schoolId = state.schoolId;

  renderFeeSettings(document.getElementById("bu-fee-settings"), schoolId);
  renderReceiptGenerator(document.getElementById("bu-receipt"), schoolId);

  mountCrudScreen(document.getElementById("bu-payments"), {
    sheet: "Payments", title: "Fee Payments", addLabel: "Record Payment",
    staticFilters: { schoolId },
    columns: [
      { key: "studentId", label: "Student", type: "ref", refSheet: "Students", refLabelKey: "name", refFilter: () => ({ schoolId }), required: true },
      { key: "purpose", label: "Purpose", type: "select", options: ["Registration","School Fees","Books Fee","Other"], required: true },
      { key: "amount", label: "Amount (₦)", type: "number", required: true },
      { key: "date", label: "Date", inputType: "date", required: true },
      { key: "receiptNo", label: "Receipt No." }
    ],
    beforeSave: (data) => {
      const receiptNo = data.receiptNo || generateShortReceiptNo();
      return { receiptNo };
    }
  });

  renderClassView(document.getElementById("bu-students"), schoolId);
}

function generateShortReceiptNo() {
  const date = new Date();
  const dateStr = String(date.getFullYear()).slice(-2) +
                  String(date.getMonth() + 1).padStart(2, '0') +
                  String(date.getDate()).padStart(2, '0');
  const datePart = dateStr.slice(-3);
  const randomPart = Math.random().toString(36).slice(2, 4).toUpperCase();
  return (datePart + randomPart).slice(0, 5);
}

async function renderFeeSettings(container, schoolId) {
  container.innerHTML = `<div class="card"><p class="hint">Loading fee settings…</p></div>`;
  
  const res = await api("list", "FeeSettings", { filters: { schoolId } });
  let settings = (res.rows || [])[0];
  
  if (!settings) {
    const createRes = await api("create", "FeeSettings", {
      data: { schoolId, registration: 0, schoolFees: 0, booksFee: 0 }
    });
    settings = createRes.row;
  }

  container.innerHTML = `
    <div class="card" style="max-width:640px;">
      <h3>Fee Settings</h3>
      <p class="hint" style="margin-top:4px;">Set the default prices for each fee type. These will auto-populate when generating receipts.</p>
      <div class="ledger-rule"></div>
      <form id="fee-settings-form">
        <div class="field-row">
          <div>
            <label>Registration Fee (₦)</label>
            <input name="registration" type="number" min="0" step="100" value="${settings.registration || 0}" required>
          </div>
          <div>
            <label>School Fees (₦)</label>
            <input name="schoolFees" type="number" min="0" step="100" value="${settings.schoolFees || 0}" required>
          </div>
        </div>
        <div class="field-row">
          <div>
            <label>Books Fee (₦)</label>
            <input name="booksFee" type="number" min="0" step="100" value="${settings.booksFee || 0}" required>
          </div>
          <div style="display:flex; align-items:flex-end;">
            <button class="btn btn-primary" style="width:100%;">Save Settings</button>
          </div>
        </div>
        <div class="modal-error" id="fee-settings-error"></div>
        <span class="hint" id="fee-settings-msg" style="margin-top:8px; display:block;"></span>
      </form>
    </div>
  `;

  const form = container.querySelector("#fee-settings-form");
  const msgEl = container.querySelector("#fee-settings-msg");
  const errorEl = container.querySelector("#fee-settings-error");

  form.onsubmit = async (e) => {
    e.preventDefault();
    errorEl.textContent = "";
    const data = Object.fromEntries(new FormData(e.target).entries());
    data.schoolId = schoolId;
    
    try {
      await api("update", "FeeSettings", { id: settings.id, data });
      msgEl.textContent = "Settings saved successfully!";
      msgEl.style.color = "var(--success)";
    } catch (err) {
      errorEl.textContent = "Could not save settings. Please try again.";
    }
  };
}

async function renderReceiptGenerator(container, schoolId) {
  const [schoolRes, feeRes] = await Promise.all([
    api("list", "Schools", { filters: { id: schoolId } }),
    api("list", "FeeSettings", { filters: { schoolId } })
  ]);
  
  const school = (schoolRes.rows || [])[0] || { name: "School", address: "", phone: "", logoUrl: "" };
  const feeSettings = (feeRes.rows || [])[0] || { registration: 0, schoolFees: 0, booksFee: 0 };

  container.innerHTML = `
    <div class="card no-print">
      <h3>Generate POS Receipt</h3>
      <p class="hint" style="margin-top:4px;">Select a student, choose which fees to include, and generate a POS-sized receipt.</p>
      <div class="ledger-rule"></div>
      <div class="field-row">
        <div>
          <label>Select Class</label>
          <select id="receipt-class">
            <option value="">Choose class…</option>
            ${CONFIG.CLASS_LIST.map(c => `<option value="${c}">${c}</option>`).join("")}
          </select>
        </div>
        <div>
          <label>Select Student</label>
          <select id="receipt-student">
            <option value="">Choose class first…</option>
          </select>
        </div>
      </div>
      
      <div class="field-row">
        <div>
          <label>Term</label>
          <select id="receipt-term">
            ${CONFIG.TERMS.map(t => `<option value="${t}">${t}</option>`).join("")}
          </select>
        </div>
        <div>
          <label>Session</label>
          <input id="receipt-session" placeholder="e.g. 2025/2026" value="2025/2026">
        </div>
      </div>
      
      <label style="margin-top:6px;">Select Fees to Include</label>
      <div class="fee-checkbox-group">
        <label>
          <input type="checkbox" class="fee-checkbox" data-fee="registration" data-amount="${feeSettings.registration || 0}">
          Registration (₦${(feeSettings.registration || 0).toLocaleString()})
        </label>
        <label>
          <input type="checkbox" class="fee-checkbox" data-fee="schoolFees" data-amount="${feeSettings.schoolFees || 0}">
          School Fees (₦${(feeSettings.schoolFees || 0).toLocaleString()})
        </label>
        <label>
          <input type="checkbox" class="fee-checkbox" data-fee="booksFee" data-amount="${feeSettings.booksFee || 0}">
          Books Fee (₦${(feeSettings.booksFee || 0).toLocaleString()})
        </label>
      </div>
      
      <div class="field-row">
        <div>
          <label>Total Amount (₦)</label>
          <div class="total-amount-display" id="receipt-total-display">₦0.00</div>
          <input id="receipt-total-amount" type="hidden" value="0">
        </div>
        <div>
          <label>Payment Date</label>
          <input id="receipt-date" type="date" value="${new Date().toISOString().slice(0,10)}">
        </div>
      </div>
      <div class="field-row">
        <div>
          <label>Receipt Number</label>
          <input id="receipt-number" readonly style="background:#f5f0e8; font-weight:bold; font-size:16px;">
        </div>
        <div style="display:flex; align-items:flex-end;">
          <button class="btn btn-primary" id="generate-receipt-btn" style="width:100%;">🖨️ Generate Receipt</button>
        </div>
      </div>
      <div class="modal-error" id="receipt-error"></div>
    </div>
    <div id="receipt-output" style="margin-top:14px;"></div>
  `;

  const classSel = container.querySelector("#receipt-class");
  const studentSel = container.querySelector("#receipt-student");
  const termSel = container.querySelector("#receipt-term");
  const sessionInput = container.querySelector("#receipt-session");
  const dateInput = container.querySelector("#receipt-date");
  const receiptNoInput = container.querySelector("#receipt-number");
  const totalDisplay = container.querySelector("#receipt-total-display");
  const totalHidden = container.querySelector("#receipt-total-amount");
  const errorEl = container.querySelector("#receipt-error");
  const outputEl = container.querySelector("#receipt-output");
  const generateBtn = container.querySelector("#generate-receipt-btn");
  const checkboxes = container.querySelectorAll(".fee-checkbox");

  receiptNoInput.value = generateShortReceiptNo();

  function updateTotal() {
    let total = 0;
    const selectedFees = [];
    checkboxes.forEach(cb => {
      if (cb.checked) {
        const amount = Number(cb.dataset.amount) || 0;
        total += amount;
        selectedFees.push(cb.dataset.fee);
      }
    });
    totalHidden.value = total;
    totalDisplay.textContent = `₦${total.toLocaleString('en-NG', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
    return { total, selectedFees };
  }

  updateTotal();

  checkboxes.forEach(cb => {
    cb.addEventListener("change", updateTotal);
  });

  classSel.addEventListener("change", async () => {
    const cls = classSel.value;
    studentSel.innerHTML = `<option value="">Select student…</option>`;
    if (!cls) {
      studentSel.disabled = true;
      return;
    }
    studentSel.disabled = false;
    const res = await api("list", "Students", { filters: { schoolId, class: cls } });
    const students = (res.rows || []).sort((a, b) => a.name.localeCompare(b.name));
    students.forEach(s => {
      const opt = document.createElement("option");
      opt.value = s.id;
      opt.textContent = `${s.name} (${s.admissionNo || "—"})`;
      studentSel.appendChild(opt);
    });
  });

  dateInput.addEventListener("change", () => {
    receiptNoInput.value = generateShortReceiptNo();
  });

  generateBtn.addEventListener("click", () => {
    const studentId = studentSel.value;
    const date = dateInput.value;
    const receiptNo = receiptNoInput.value;
    const total = Number(totalHidden.value) || 0;
    const term = termSel.value;
    const session = sessionInput.value.trim();

    if (!studentId) {
      errorEl.textContent = "Please select a student.";
      return;
    }
    if (total <= 0) {
      errorEl.textContent = "Please select at least one fee to generate a receipt.";
      return;
    }
    if (!session) {
      errorEl.textContent = "Please enter the session.";
      return;
    }

    errorEl.textContent = "";

    const selectedFees = [];
    checkboxes.forEach(cb => {
      if (cb.checked) {
        const label = cb.parentElement.textContent.trim();
        const amount = Number(cb.dataset.amount) || 0;
        selectedFees.push({ label, amount });
      }
    });

    const studentOpt = studentSel.options[studentSel.selectedIndex];
    const studentName = studentOpt ? studentOpt.text.split(" (")[0] : "Student";
    const cls = classSel.options[classSel.selectedIndex].text;

    const formattedTotal = total.toLocaleString('en-NG', { minimumFractionDigits: 2, maximumFractionDigits: 2 });

    let feeBreakdownHtml = selectedFees.map(f => `
      <div class="receipt-row">
        <span>${f.label}</span>
        <span>₦${f.amount.toLocaleString()}</span>
      </div>
    `).join("");

    const receiptHtml = `
      <div class="pos-receipt" id="pos-receipt-print">
        <div class="receipt-header">
          ${school.logoUrl ? `<img src="${school.logoUrl}" alt="Logo">` : ""}
          <h3>${school.name || "School"}</h3>
          <p>${school.address || ""}</p>
          <p>${school.phone || ""}</p>
          <div style="border-top:1px dashed #999; margin-top:4px; padding-top:4px;">
            <strong>PAYMENT RECEIPT</strong>
          </div>
        </div>

        <div class="receipt-row">
          <span class="receipt-label">Receipt #</span>
          <span>${receiptNo}</span>
        </div>
        <div class="receipt-row">
          <span class="receipt-label">Date</span>
          <span>${date}</span>
        </div>
        <div class="receipt-row">
          <span class="receipt-label">Term</span>
          <span>${term}</span>
        </div>
        <div class="receipt-row">
          <span class="receipt-label">Session</span>
          <span>${session}</span>
        </div>
        <div class="receipt-row">
          <span class="receipt-label">Student</span>
          <span>${studentName}</span>
        </div>
        <div class="receipt-row">
          <span class="receipt-label">Class</span>
          <span>${cls}</span>
        </div>
        
        <div style="border-top:1px dashed #999; margin:4px 0;"></div>
        ${feeBreakdownHtml}
        
        <div class="receipt-row total">
          <span>TOTAL</span>
          <span>₦${formattedTotal}</span>
        </div>

        <div class="receipt-footer">
          <p>Thank you for your payment!</p>
          <p style="font-size:10px;">This is a system-generated receipt.</p>
          <button class="btn btn-primary btn-sm" onclick="window.print()" style="margin-top:4px;">🖨️ Print Receipt</button>
        </div>
      </div>
    `;

    outputEl.innerHTML = receiptHtml;

    selectedFees.forEach(fee => {
      savePayment(schoolId, studentId, studentName, fee.label, fee.amount, date, receiptNo, term, session);
    });
  });

  async function savePayment(schoolId, studentId, studentName, purpose, amount, date, receiptNo, term, session) {
    try {
      await api("create", "Payments", {
        data: {
          schoolId,
          studentId,
          purpose,
          amount,
          date,
          receiptNo,
          studentName,
          term,
          session
        }
      });
      console.log("Payment saved:", purpose);
    } catch (err) {
      console.error("Could not save payment:", err);
    }
  }
}

/* =====================================================================
   TEACHER SCREENS
===================================================================== */
function calcGrade(total, ca1Weight = 20, ca2Weight = 20, examWeight = 60) {
  const maxTotal = ca1Weight + ca2Weight + examWeight;
  const t = Number(total);
  const pct = (t / maxTotal) * 100;
  if (pct >= 70) return "A";
  if (pct >= 60) return "B";
  if (pct >= 50) return "C";
  if (pct >= 45) return "D";
  if (pct >= 40) return "E";
  return "F";
}

function initTeacher() {
  const schoolId = state.schoolId;
  const container = document.getElementById("te-view");
  mountCrudScreenWithDynamicMax(container, schoolId);
  renderScoreSheet(document.getElementById("te-upload"), schoolId);
}

async function mountCrudScreenWithDynamicMax(container, schoolId) {
  const { cfg } = await fetchSchoolConfig(schoolId);
  const { ca1Max, ca2Max, examMax } = getMaxScores(cfg);
  const ca1Label = cfg.ca1Label || "TEST 1";
  const ca2Label = cfg.ca2Label || "TEST 2";
  const examLabel = cfg.examLabel || "EXAM";

  mountCrudScreen(container, {
    sheet: "Results", title: "View Results", addLabel: "Add Result",
    subtitle: `Max scores: ${ca1Label} (${ca1Max}), ${ca2Label} (${ca2Max}), ${examLabel} (${examMax})`,
    staticFilters: { schoolId },
    filters: {
      class: { label: "Classes", options: CONFIG.CLASS_LIST.map(c => ({ value: c, label: c })) },
      term: { label: "Terms", options: CONFIG.TERMS.map(t => ({ value: t, label: t })) }
    },
    columns: [
      { key: "studentId", label: "Student", type: "ref", refSheet: "Students", refLabelKey: "name", refFilter: () => ({ schoolId }), required: true },
      { key: "subjectId", label: "Subject", type: "ref", refSheet: "Subjects", refLabelKey: "name", refFilter: () => ({ schoolId }), required: true },
      { key: "class", label: "Class", type: "select", options: CONFIG.CLASS_LIST, required: true },
      { key: "term", label: "Term", type: "select", options: CONFIG.TERMS, required: true },
      { key: "session", label: "Session", default: "2025/2026", required: true },
      { key: "ca1", label: `${ca1Label} (/${ca1Max})`, type: "number", min: 0, max: ca1Max, required: true },
      { key: "ca2", label: `${ca2Label} (/${ca2Max})`, type: "number", min: 0, max: ca2Max, required: true },
      { key: "exam", label: `${examLabel} (/${examMax})`, type: "number", min: 0, max: examMax, required: true },
      { key: "total", label: "Total", readonly: true },
      { key: "grade", label: "Grade", type: "badge", badgeClass: gradeBadge, readonly: true }
    ],
    beforeSave: (data) => {
      const total = Number(data.ca1 || 0) + Number(data.ca2 || 0) + Number(data.exam || 0);
      return { total, grade: calcGrade(total, ca1Max, ca2Max, examMax) };
    }
  });
}

function openBulkScoreModal({ schoolId, classList, subjectList, term, session, onSaved }) {
  const body = document.getElementById("bulkModalBody");
  const title = document.getElementById("bulkModalTitle");
  title.textContent = "Bulk Score Entry";

  body.innerHTML = `
    <p class="hint">Enter scores for multiple students at once. All fields are required for each student.</p>
    <div class="field-row">
      <div>
        <label>Class</label>
        <select id="bulk-score-class">
          <option value="">Select class…</option>
          ${classList.map(c => `<option value="${c}">${c}</option>`).join("")}
        </select>
      </div>
      <div>
        <label>Subject</label>
        <select id="bulk-score-subject">
          <option value="">Select subject…</option>
          ${subjectList.map(s => `<option value="${s.id}">${s.name}</option>`).join("")}
        </select>
      </div>
    </div>
    <div class="field-row">
      <div>
        <label>Term</label>
        <select id="bulk-score-term">
          ${CONFIG.TERMS.map(t => `<option value="${t}" ${t === term ? "selected" : ""}>${t}</option>`).join("")}
        </select>
      </div>
      <div>
        <label>Session</label>
        <input id="bulk-score-session" value="${session || "2025/2026"}">
      </div>
    </div>
    <div id="bulk-score-student-list" style="margin-top:10px;">
      <p class="hint">Select a class and subject above, then click "Load Students" to populate the score grid.</p>
      <button class="btn btn-primary" id="bulk-load-students">Load Students</button>
    </div>
    <div id="bulk-score-grid-container" style="margin-top:10px;"></div>
    <div class="modal-error" id="bulk-score-error"></div>
    <div style="display:flex; gap:10px; margin-top:14px;">
      <button type="button" class="btn btn-ghost" data-close-modal style="flex:1;">Cancel</button>
      <button type="button" class="btn btn-primary" id="bulk-score-save" style="flex:1;">Save All Scores</button>
    </div>
  `;

  document.getElementById("bulkModal").classList.remove("hidden");

  let loadedStudents = [];
  let config = {};
  let currentMaxScores = { ca1Max: 20, ca2Max: 20, examMax: 60 };

  const loadBtn = body.querySelector("#bulk-load-students");
  const gridContainer = body.querySelector("#bulk-score-grid-container");
  const classSel = body.querySelector("#bulk-score-class");
  const subjectSel = body.querySelector("#bulk-score-subject");
  const termSel = body.querySelector("#bulk-score-term");
  const sessionInput = body.querySelector("#bulk-score-session");
  const errorEl = body.querySelector("#bulk-score-error");

  loadBtn.addEventListener("click", async () => {
    const cls = classSel.value;
    const subjectId = subjectSel.value;
    if (!cls || !subjectId) {
      errorEl.textContent = "Please select both a class and a subject.";
      return;
    }
    errorEl.textContent = "";
    gridContainer.innerHTML = `<p class="hint">Loading students…</p>`;

    const [studentsRes, resultsRes, schoolRes] = await Promise.all([
      api("list", "Students", { filters: { schoolId, class: cls } }),
      api("list", "Results", { filters: { schoolId, subjectId, term: termSel.value, session: sessionInput.value } }),
      api("list", "Schools", { filters: { id: schoolId } })
    ]);

    const students = studentsRes.rows || [];
    const existingResults = resultsRes.rows || [];
    const school = (schoolRes.rows || [])[0] || {};
    config = school.config || { ca1Label: "TEST 1", ca2Label: "TEST 2", examLabel: "EXAM", ca1Weight: 20, ca2Weight: 20, examWeight: 60 };
    currentMaxScores = getMaxScores(config);
    const { ca1Max, ca2Max, examMax } = currentMaxScores;

    if (!students.length) {
      gridContainer.innerHTML = `<div class="empty-state">No students found in ${cls}. Add students first.</div>`;
      return;
    }

    const resultMap = Object.fromEntries(existingResults.map(r => [r.studentId, r]));
    loadedStudents = students;

    let html = `<div class="table-wrap"><table class="bulk-score-grid"><thead><tr>
      <th>#</th><th>Student</th><th>${config.ca1Label || "TEST 1"} (/${ca1Max})</th>
      <th>${config.ca2Label || "TEST 2"} (/${ca2Max})</th>
      <th>${config.examLabel || "EXAM"} (/${examMax})</th>
      <th>Total</th><th>Grade</th>
    </tr></thead><tbody>`;

    students.forEach((s, i) => {
      const r = resultMap[s.id] || {};
      html += `<tr data-student-id="${s.id}">
        <td>${i + 1}</td>
        <td>${s.name} (${s.admissionNo || "—"})</td>
        <td><input type="number" min="0" max="${ca1Max}" class="bulk-ca1" value="${r.ca1 ?? ""}" style="width:55px;"></td>
        <td><input type="number" min="0" max="${ca2Max}" class="bulk-ca2" value="${r.ca2 ?? ""}" style="width:55px;"></td>
        <td><input type="number" min="0" max="${examMax}" class="bulk-exam" value="${r.exam ?? ""}" style="width:55px;"></td>
        <td class="bulk-total">${r.total ?? "—"}</td>
        <td class="bulk-grade"><span class="badge badge-${gradeBadge(r.grade)}">${r.grade || "—"}</span></td>
      </tr>`;
    });

    html += `</tbody></table></div>
      <p class="hint">Enter scores for each student. Empty fields will be skipped. Max values are enforced.</p>`;

    gridContainer.innerHTML = html;

    gridContainer.querySelectorAll("tbody tr").forEach(tr => {
      const recalc = () => {
        const ca1v = tr.querySelector(".bulk-ca1").value;
        const ca2v = tr.querySelector(".bulk-ca2").value;
        const examv = tr.querySelector(".bulk-exam").value;
        
        [".bulk-ca1", ".bulk-ca2", ".bulk-exam"].forEach((selector, idx) => {
          const input = tr.querySelector(selector);
          const max = [ca1Max, ca2Max, examMax][idx];
          if (input.value !== "" && Number(input.value) > max) {
            input.value = max;
          }
        });
        
        const ca1Val = Number(tr.querySelector(".bulk-ca1").value) || 0;
        const ca2Val = Number(tr.querySelector(".bulk-ca2").value) || 0;
        const examVal = Number(tr.querySelector(".bulk-exam").value) || 0;
        const hasAny = tr.querySelector(".bulk-ca1").value !== "" || 
                       tr.querySelector(".bulk-ca2").value !== "" || 
                       tr.querySelector(".bulk-exam").value !== "";
        const total = ca1Val + ca2Val + examVal;
        tr.querySelector(".bulk-total").textContent = hasAny ? total : "—";
        const grade = hasAny ? calcGrade(total, ca1Max, ca2Max, examMax) : "—";
        tr.querySelector(".bulk-grade").innerHTML = `<span class="badge badge-${gradeBadge(grade)}">${grade}</span>`;
      };
      tr.querySelectorAll("input").forEach(inp => inp.addEventListener("input", recalc));
      tr.querySelectorAll("input").forEach(inp => inp.addEventListener("blur", () => {
        const max = inp.classList.contains("bulk-ca1") ? ca1Max :
                    inp.classList.contains("bulk-ca2") ? ca2Max : examMax;
        if (inp.value !== "" && Number(inp.value) > max) {
          inp.value = max;
          recalc();
        }
      }));
    });
  });

  body.querySelector("#bulk-score-save").addEventListener("click", async () => {
    const cls = classSel.value;
    const subjectId = subjectSel.value;
    const term = termSel.value;
    const session = sessionInput.value.trim();

    if (!cls || !subjectId || !term || !session) {
      errorEl.textContent = "Please select class, subject, term, and session.";
      return;
    }

    const rows = [];
    const trs = gridContainer.querySelectorAll("tbody tr");
    if (!trs.length) {
      errorEl.textContent = "No students loaded. Click 'Load Students' first.";
      return;
    }

    const { ca1Max, ca2Max, examMax } = currentMaxScores;

    trs.forEach(tr => {
      const studentId = tr.dataset.studentId;
      const ca1Input = tr.querySelector(".bulk-ca1");
      const ca2Input = tr.querySelector(".bulk-ca2");
      const examInput = tr.querySelector(".bulk-exam");
      
      let ca1 = ca1Input.value !== "" ? Number(ca1Input.value) : null;
      let ca2 = ca2Input.value !== "" ? Number(ca2Input.value) : null;
      let exam = examInput.value !== "" ? Number(examInput.value) : null;
      
      if (ca1 !== null && ca1 > ca1Max) ca1 = ca1Max;
      if (ca2 !== null && ca2 > ca2Max) ca2 = ca2Max;
      if (exam !== null && exam > examMax) exam = examMax;
      
      if (ca1 === null && ca2 === null && exam === null) return;
      
      const total = (ca1 || 0) + (ca2 || 0) + (exam || 0);
      rows.push({
        studentId, subjectId, class: cls, term, session, schoolId,
        ca1: ca1 || 0, ca2: ca2 || 0, exam: exam || 0,
        total, grade: calcGrade(total, ca1Max, ca2Max, examMax)
      });
    });

    if (!rows.length) {
      errorEl.textContent = "No scores entered. Fill in at least one student's scores.";
      return;
    }

    const saveBtn = body.querySelector("#bulk-score-save");
    saveBtn.disabled = true;
    saveBtn.textContent = "Saving…";
    errorEl.textContent = "";

    try {
      const result = await api("bulkSave", "Results", {
        rows,
        matchKeys: ["studentId", "subjectId", "term", "session"]
      });
      if (result.ok) {
        document.getElementById("bulkModal").classList.add("hidden");
        if (onSaved) onSaved();
      } else {
        errorEl.textContent = result.error || "Could not save scores.";
      }
    } catch (err) {
      errorEl.textContent = "Could not save scores. Please try again.";
    } finally {
      saveBtn.disabled = false;
      saveBtn.textContent = "Save All Scores";
    }
  });
}

async function renderScoreSheet(container, schoolId) {
  const { cfg } = await fetchSchoolConfig(schoolId);
  const ca1Weight = Number(cfg.ca1Weight) || 20;
  const ca2Weight = Number(cfg.ca2Weight) || 20;
  const examWeight = Number(cfg.examWeight) || 60;
  const ca1Label = cfg.ca1Label || "TEST 1";
  const ca2Label = cfg.ca2Label || "TEST 2";
  const examLabel = cfg.examLabel || "EXAM";

  container.innerHTML = `
    <div class="card no-print">
      <h3>Score Sheet</h3>
      <p class="hint" style="margin-top:4px;">Choose a class and subject to enter scores for all students at once.</p>
      <div class="ledger-rule"></div>
      <div class="toolbar">
        <select id="ss-class"><option value="">Select class…</option>${CONFIG.CLASS_LIST.map(c => `<option value="${c}">${c}</option>`).join("")}</select>
        <select id="ss-subject" disabled><option value="">Select class first…</option></select>
        <select id="ss-term"><option value="">Select term…</option>${CONFIG.TERMS.map(t => `<option value="${t}">${t}</option>`).join("")}</select>
        <input id="ss-session" placeholder="Session e.g. 2025/2026" value="2025/2026">
        <button class="btn btn-primary" id="ss-bulk-btn">📊 Bulk Score Entry</button>
      </div>
      <p class="hint">Max scores: ${ca1Label} (${ca1Weight}), ${ca2Label} (${ca2Weight}), ${examLabel} (${examWeight})</p>
      <p class="hint">Click "Bulk Score Entry" to enter scores for multiple students in a grid.</p>
    </div>
    <div id="ss-output"><p class="hint">Choose class, subject, term and session above to load the score sheet.</p></div>`;

  const classSel = container.querySelector("#ss-class");
  const subjectSel = container.querySelector("#ss-subject");
  const termSel = container.querySelector("#ss-term");
  const sessionInput = container.querySelector("#ss-session");
  const output = container.querySelector("#ss-output");

  let currentSubjectList = [];

  classSel.addEventListener("change", async () => {
    const cls = classSel.value;
    output.innerHTML = `<p class="hint">Choose class, subject, term and session above to load the score sheet.</p>`;
    if (!cls) { subjectSel.innerHTML = `<option value="">Select class first…</option>`; subjectSel.disabled = true; return; }
    subjectSel.innerHTML = `<option value="">Loading…</option>`;
    const res = await api("list", "Subjects", { filters: { schoolId, class: cls } });
    const subs = res.rows || [];
    currentSubjectList = subs;
    subjectSel.disabled = false;
    subjectSel.innerHTML = `<option value="">Select subject…</option>` + subs.map(s => `<option value="${s.id}">${s.name}</option>`).join("");
  });

  container.querySelector("#ss-bulk-btn").addEventListener("click", async () => {
    const cls = classSel.value;
    if (!cls) {
      alert("Please select a class first.");
      return;
    }
    if (!currentSubjectList.length) {
      const res = await api("list", "Subjects", { filters: { schoolId, class: cls } });
      currentSubjectList = res.rows || [];
    }
    if (!currentSubjectList.length) {
      alert("No subjects found for this class. Please add subjects first.");
      return;
    }

    const term = termSel.value || CONFIG.TERMS[0];
    const session = sessionInput.value.trim() || "2025/2026";

    openBulkScoreModal({
      schoolId,
      classList: CONFIG.CLASS_LIST,
      subjectList: currentSubjectList,
      term,
      session,
      onSaved: () => {
        if (classSel.value && subjectSel.value) {
          loadGrid();
        }
      }
    });
  });

  async function loadGrid() {
    const cls = classSel.value, subjectId = subjectSel.value, term = termSel.value, session = sessionInput.value.trim();
    if (!cls || !subjectId || !term || !session) {
      output.innerHTML = `<p class="hint">Choose class, subject, term and session above to load the score sheet.</p>`;
      return;
    }
    output.innerHTML = `<p class="hint">Loading…</p>`;
    const [studentsRes, resultsRes] = await Promise.all([
      api("list", "Students", { filters: { schoolId, class: cls } }),
      api("list", "Results", { filters: { schoolId, subjectId, term, session } })
    ]);
    const students = studentsRes.rows || [];
    const resultMap = Object.fromEntries((resultsRes.rows || []).map(r => [r.studentId, r]));
    if (!students.length) {
      output.innerHTML = `<div class="card"><div class="empty-state">No students in ${cls} yet. Add them from the Admin › Students screen first.</div></div>`;
      return;
    }
    const subjectName = subjectSel.options[subjectSel.selectedIndex].text;

    output.innerHTML = `
      <div class="card">
        <div class="card-header">
          <div><h3>${cls} — ${subjectName}</h3><p class="hint" style="margin-top:4px;">${term}, ${session}</p></div>
          <div style="display:flex; gap:10px;">
            <button class="btn btn-ghost no-print" id="ss-print">Print</button>
            <button class="btn btn-primary no-print" id="ss-save">Save All Scores</button>
          </div>
        </div>
        <div class="ledger-rule"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Student</th><th>Admission No.</th><th>${ca1Label} (/${ca1Weight})</th><th>${ca2Label} (/${ca2Weight})</th><th>${examLabel} (/${examWeight})</th><th>Total</th><th>Grade</th></tr></thead>
        <tbody>${students.map((s, i) => {
          const r = resultMap[s.id] || {};
          return `<tr data-student="${s.id}">
            <td>${i + 1}</td><td>${s.name}</td><td>${s.admissionNo || "—"}</td>
            <td><input type="number" min="0" max="${ca1Weight}" class="ss-ca1" value="${r.ca1 ?? ""}"></td>
            <td><input type="number" min="0" max="${ca2Weight}" class="ss-ca2" value="${r.ca2 ?? ""}"></td>
            <td><input type="number" min="0" max="${examWeight}" class="ss-exam" value="${r.exam ?? ""}"></td>
            <td class="ss-total">${r.total ?? "—"}</td>
            <td class="ss-grade"><span class="badge badge-${gradeBadge(r.grade)}">${r.grade || "—"}</span></td>
          </tr>`;
        }).join("")}</tbody></table></div>
        <p class="hint no-print" id="ss-save-msg" style="margin-top:10px;"></p>
      </div>`;

    output.querySelectorAll("tbody tr").forEach(tr => {
      const recalc = () => {
        const ca1v = tr.querySelector(".ss-ca1").value, ca2v = tr.querySelector(".ss-ca2").value, examv = tr.querySelector(".ss-exam").value;
        const hasAny = ca1v !== "" || ca2v !== "" || examv !== "";
        const total = Number(ca1v || 0) + Number(ca2v || 0) + Number(examv || 0);
        tr.querySelector(".ss-total").textContent = hasAny ? total : "—";
        const grade = hasAny ? calcGrade(total, ca1Weight, ca2Weight, examWeight) : "—";
        tr.querySelector(".ss-grade").innerHTML = `<span class="badge badge-${gradeBadge(grade)}">${grade}</span>`;
      };
      tr.querySelectorAll("input").forEach(inp => inp.addEventListener("input", recalc));
      tr.querySelectorAll("input").forEach(inp => inp.addEventListener("blur", () => {
        const max = inp.classList.contains("ss-ca1") ? ca1Weight :
                    inp.classList.contains("ss-ca2") ? ca2Weight : examWeight;
        if (inp.value !== "" && Number(inp.value) > max) {
          inp.value = max;
          recalc();
        }
      }));
    });

    output.querySelector("#ss-print").onclick = () => window.print();
    output.querySelector("#ss-save").onclick = async () => {
      const btn = output.querySelector("#ss-save");
      btn.disabled = true; btn.textContent = "Saving…";
      const rows = [...output.querySelectorAll("tbody tr")].map(tr => {
        const studentId = tr.dataset.student;
        let ca1 = tr.querySelector(".ss-ca1").value;
        let ca2 = tr.querySelector(".ss-ca2").value;
        let exam = tr.querySelector(".ss-exam").value;
        
        if (ca1 !== "" && Number(ca1) > ca1Weight) ca1 = ca1Weight;
        if (ca2 !== "" && Number(ca2) > ca2Weight) ca2 = ca2Weight;
        if (exam !== "" && Number(exam) > examWeight) exam = examWeight;
        
        if (ca1 === "" && ca2 === "" && exam === "") return null;
        const total = Number(ca1 || 0) + Number(ca2 || 0) + Number(exam || 0);
        return { studentId, subjectId, class: cls, term, session, schoolId, ca1: Number(ca1 || 0), ca2: Number(ca2 || 0), exam: Number(exam || 0), total, grade: calcGrade(total, ca1Weight, ca2Weight, examWeight) };
      }).filter(Boolean);
      const msg = output.querySelector("#ss-save-msg");
      if (!rows.length) { msg.textContent = "Nothing to save yet — enter at least one score."; btn.disabled = false; btn.textContent = "Save All Scores"; return; }
      try {
        await api("bulkSave", "Results", { rows, matchKeys: ["studentId", "subjectId", "term", "session"] });
        msg.textContent = `Saved ${rows.length} student score(s).`;
      } catch (err) {
        msg.textContent = "Could not save. Please try again.";
      } finally {
        btn.disabled = false; btn.textContent = "Save All Scores";
      }
    };
  }

  [classSel, subjectSel, termSel].forEach(el => el.addEventListener("change", loadGrid));
  sessionInput.addEventListener("change", loadGrid);
}

/* =====================================================================
   SIGNUP / ACTIVATE SCREENS
===================================================================== */
function initSignup() {
  renderSignup(document.getElementById("su-signup"));
  renderActivate(document.getElementById("su-activate"));
}

async function renderSignup(container) {
  const payRes = await api("list", "PaymentSettings", { filters: {} });
  const paymentSettings = (payRes.rows || [])[0] || { accountName: "", accountNumber: "", bankName: "", registrationFee: 0 };

  container.innerHTML = `
    <div class="card" style="max-width:560px;">
      <h3>Sign Up Your School</h3>
      <p class="hint" style="margin-top:4px;">Complete the registration form and make the required payment to activate your account.</p>
      <div class="ledger-rule"></div>
      
      ${paymentSettings.registrationFee > 0 ? `
        <div class="payment-details-box">
          <h4 style="margin:0 0 4px 0; font-size:14px;">Payment Required</h4>
          <p><strong>Amount:</strong> ₦${paymentSettings.registrationFee.toLocaleString()}</p>
          <p><strong>Account Name:</strong> ${paymentSettings.accountName || "Not set"}</p>
          <p><strong>Account Number:</strong> ${paymentSettings.accountNumber || "Not set"}</p>
          <p><strong>Bank:</strong> ${paymentSettings.bankName || "Not set"}</p>
          <p class="hint" style="margin-top:2px;">Pay the registration fee to the above account and use your email as reference.</p>
        </div>
      ` : `
        <div class="banner" style="background:#FFF3CD; border-color:#FFC107; color:#856404;">
          <strong>Note:</strong> Registration fee has not been configured by the Super Admin. Please contact the platform administrator.
        </div>
      `}
      
      <form id="signup-form">
        <label>School Name</label><input name="schoolName" required>
        <label>Your Name (Admin)</label><input name="adminName" required>
        <label>Email</label><input name="email" type="email" required>
        <label>Phone</label><input name="phone" required>
        <label>Choose a Password</label><input name="password" type="password" required>
        
        <div style="margin-top:10px;">
          <label style="display:flex; align-items:flex-start; gap:6px; font-weight:normal; cursor:pointer;">
            <input type="checkbox" id="payment-confirm" required style="width:16px; height:16px; margin-top:2px;">
            <span>I confirm that I have made the payment of <strong>₦${paymentSettings.registrationFee.toLocaleString()}</strong> to the account details above.</span>
          </label>
        </div>
        
        <button class="btn btn-primary btn-block" style="margin-top:16px;">Submit Registration</button>
      </form>
      <div id="signup-result"></div>
    </div>
  `;

  document.getElementById("signup-form").onsubmit = async (e) => {
    e.preventDefault();
    const data = Object.fromEntries(new FormData(e.target).entries());
    const confirmCheck = document.getElementById("payment-confirm");
    
    if (!confirmCheck.checked) {
      document.getElementById("signup-result").innerHTML = `<div class="banner banner-danger" style="margin-top:14px;">Please confirm that you have made the payment.</div>`;
      return;
    }
    
    const res = await api("signup", "Users", { data });
    const out = document.getElementById("signup-result");
    if (res.ok) {
      out.innerHTML = `
        <div class="banner banner-success" style="margin-top:14px;">
          <strong>Registration submitted successfully!</strong><br>
          ${res.note || "Your account is pending confirmation."}
          ${res.row && res.row.activationCode ? `<br>Your activation code: <strong>${res.row.activationCode}</strong>` : ""}
          <br><br>Please wait for the Super Admin to confirm your payment and activate your account.
        </div>`;
      e.target.reset();
      confirmCheck.checked = false;
    } else {
      out.innerHTML = `<div class="banner banner-danger" style="margin-top:14px;">${res.error || "Something went wrong."}</div>`;
    }
  };
}

function renderActivate(container) {
  container.innerHTML = `
    <div class="card" style="max-width:520px;">
      <h3>Activate Your Account</h3>
      <p class="hint" style="margin-top:4px;">Once your payment is confirmed, enter your email and activation code to activate your account.</p>
      <div class="ledger-rule"></div>
      <form id="activate-form">
        <label>Email</label><input name="email" type="email" required>
        <label>Activation Code</label><input name="code" required>
        <button class="btn btn-primary btn-block" style="margin-top:16px;">Activate</button>
      </form>
      <div id="activate-result"></div>
    </div>`;
    
  document.getElementById("activate-form").onsubmit = async (e) => {
    e.preventDefault();
    const data = Object.fromEntries(new FormData(e.target).entries());
    const res = await api("activate", "Users", data);
    const out = document.getElementById("activate-result");
    out.innerHTML = res.ok
      ? `<div class="banner banner-success" style="margin-top:14px;">Account activated. You can now log in from your role's tile on the home screen.</div>`
      : `<div class="banner banner-danger" style="margin-top:14px;">${res.error || "Could not activate. Check the email and code."}</div>`;
  };
}
</script>
</body>
</html>
