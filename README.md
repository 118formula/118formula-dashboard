<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>118 Formula — Trading Dashboard</title>
  <style>
    :root{
      --bg:#0f1724; --card:#0b1220; --muted:#98a0b3; --accent:#06b6d4; --glass: rgba(255,255,255,0.04);
      --success: #10b981; --danger:#ef4444; --radius:12px;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%;margin:0;background:linear-gradient(180deg,#071025 0%, #081427 60%);color:#e6eef6}
    .container{max-width:1100px;margin:28px auto;padding:20px}

    /* Top bar */
    .topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px}
    .top-left{display:flex;align-items:center;gap:14px}
    .avatar{width:46px;height:46px;border-radius:10px;background:linear-gradient(135deg,#0ea5a3, #06b6d4);display:flex;align-items:center;justify-content:center;font-weight:700;color:#021218}
    .user-info{display:flex;flex-direction:column}
    .user-info .email{font-weight:600}
    .user-info .date{font-size:13px;color:var(--muted)}

    /* Cards grid */
    .grid{display:grid;grid-template-columns:repeat(12,1fr);gap:14px}
    .card{background:var(--card);padding:16px;border-radius:var(--radius);box-shadow:0 6px 18px rgba(2,6,23,0.6);}

    .big-card{grid-column:span 8}
    .side-card{grid-column:span 4}

    .metrics{display:flex;gap:12px}
    .metric{flex:1;background:var(--glass);padding:12px;border-radius:10px}
    .metric .label{font-size:13px;color:var(--muted);margin-bottom:6px}
    .metric .value{font-size:20px;font-weight:700}

    table{width:100%;border-collapse:collapse;margin-top:10px}
    th,td{padding:10px;text-align:left;font-size:14px}
    th{color:var(--muted);font-weight:600}
    tbody tr{border-top:1px solid rgba(255,255,255,0.03)}

    .platform{display:inline-block;padding:6px 10px;border-radius:8px;background:rgba(6,182,212,0.12);color:var(--accent);font-weight:700}

    /* small responsive */
    @media (max-width:900px){.big-card{grid-column:span 12}.side-card{grid-column:span 12}.metrics{flex-direction:column}}

    /* footer */
    .footer{margin-top:18px;color:var(--muted);font-size:13px;text-align:center}

    /* simple control row */
    .controls{display:flex;gap:8px;align-items:center}
    .btn{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 12px;border-radius:8px;color:var(--muted);cursor:pointer}
    .btn.primary{background:linear-gradient(90deg,#06b6d4,#0ea5a3);color:#021218;border:0}

  </style>
</head>
<body>
  <div class="container">
    <div class="topbar">
      <div class="top-left">
        <div class="avatar">118</div>
        <div class="user-info">
          <div class="email" id="userEmail">user@example.com</div>
          <div class="date" id="topDate">Loading date...</div>
        </div>
      </div>
      <div class="controls">
        <div class="platform" id="platformType">MT5</div>
        <button class="btn" id="refreshBtn">Refresh</button>
        <button class="btn primary" id="exportBtn">Export CSV</button>
      </div>
    </div>

    <div class="grid">
      <div class="card big-card">
        <div style="display:flex;justify-content:space-between;align-items:center;">
          <div>
            <h2 style="margin:0 0 6px 0">Account Overview</h2>
            <div style="color:var(--muted);font-size:13px">Live snapshot of balances and equity</div>
          </div>
          <div style="text-align:right;color:var(--muted);font-size:13px">Account ID: <strong id="accountId">118-001</strong></div>
        </div>

        <div style="margin-top:14px" class="metrics">
          <div class="metric">
            <div class="label">Balance</div>
            <div class="value" id="balanceVal">$12,345.67</div>
          </div>
          <div class="metric">
            <div class="label">Equity</div>
            <div class="value" id="equityVal">$11,987.41</div>
          </div>
          <div class="metric">
            <div class="label">Free Margin</div>
            <div class="value" id="freeMarginVal">$3,210.11</div>
          </div>
        </div>

        <div style="margin-top:16px">
          <table>
            <thead>
              <tr><th>Asset</th><th>Type</th><th>Volume</th><th>Price</th><th>Profit</th><th>Date</th></tr>
            </thead>
            <tbody id="positionsTable">
              <tr><td>EURUSD</td><td>Sell</td><td>0.50</td><td>1.09234</td><td style="color:var(--success)">+$124.50</td><td>2025-11-12</td></tr>
              <tr><td>GOLD</td><td>Buy</td><td>1.00</td><td>2034.12</td><td style="color:var(--danger)">-$45.20</td><td>2025-11-12</td></tr>
            </tbody>
          </table>
        </div>

      </div>

      <div class="card side-card">
        <h3 style="margin:0 0 8px 0">Quick Details</h3>
        <div style="color:var(--muted);font-size:13px;margin-bottom:12px">Editable account metadata</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
          <div>
            <div style="font-size:13px;color:var(--muted)">Platform</div>
            <div id="platformText">MT5</div>
          </div>
          <div>
            <div style="font-size:13px;color:var(--muted)">Asset</div>
            <div id="assetText">EURUSD</div>
          </div>
          <div>
            <div style="font-size:13px;color:var(--muted)">Leverage</div>
            <div id="leverageText">1:100</div>
          </div>
          <div>
            <div style="font-size:13px;color:var(--muted)">Currency</div>
            <div id="currencyText">USD</div>
          </div>
        </div>

        <div style="margin-top:12px">
          <label style="font-size:13px;color:var(--muted)">Quick comment</label>
          <textarea id="quickNote" style="width:100%;height:78px;margin-top:6px;padding:8px;border-radius:8px;background:transparent;border:1px solid rgba(255,255,255,0.04);color:inherit"></textarea>
          <button class="btn primary" style="margin-top:10px;width:100%" id="saveNote">Save Note</button>
        </div>
      </div>

    </div>

    <div class="footer">118 Formula • Live trading dashboard • Last update: <span id="lastUpdate">—</span></div>
  </div>

  <script>
    // Simple script to allow quick demo behaviour. Replace with real data hooks as needed.
    (function(){
      const userEmailEl = document.getElementById('userEmail');
      const topDateEl = document.getElementById('topDate');
      const lastUpdateEl = document.getElementById('lastUpdate');

      // Example: you can set these values from server-side or via WebSocket
      const initialData = {
        email: 'trader@118formula.com',
        platform: 'MT5',
        accountId: '118-TR-09',
        balance: 12345.67,
        equity: 11987.41,
        freeMargin: 3210.11,
        positions: [
          {asset:'EURUSD', type:'Sell', volume:0.5, price:1.09234, profit:124.50, date:'2025-11-12'},
          {asset:'GOLD', type:'Buy', volume:1.0, price:2034.12, profit:-45.20, date:'2025-11-12'}
        ]
      };

      function fmt(n){return (typeof n === 'number')? n.toLocaleString(undefined,{minimumFractionDigits:2,maximumFractionDigits:2}): n}

      function render(d){
        userEmailEl.textContent = d.email || 'user@example.com';
        document.getElementById('platformType').textContent = d.platform || 'MT5';
        document.getElementById('platformText').textContent = d.platform || '';
        document.getElementById('accountId').textContent = d.accountId || '';
        document.getElementById('balanceVal').textContent = '$' + fmt(d.balance);
        document.getElementById('equityVal').textContent = '$' + fmt(d.equity);
        document.getElementById('freeMarginVal').textContent = '$' + fmt(d.freeMargin);

        const tbody = document.getElementById('positionsTable');
        tbody.innerHTML = '';
        (d.positions||[]).forEach(p=>{
          const tr = document.createElement('tr');
          tr.innerHTML = `<td>${p.asset}</td><td>${p.type}</td><td>${p.volume}</td><td>${p.price}</td><td style=\"color:${p.profit>=0? 'var(--success)':'var(--danger)'}\">${p.profit>=0? '+':'-'}$${fmt(Math.abs(p.profit))}</td><td>${p.date}</td>`;
          tbody.appendChild(tr);
        })

        const now = new Date();
        topDateEl.textContent = now.toLocaleString();
        lastUpdateEl.textContent = now.toISOString();
      }

      document.getElementById('refreshBtn').addEventListener('click', ()=>{
        // placeholder: in real app you'd fetch new data
        render(initialData);
      });

      document.getElementById('exportBtn').addEventListener('click', ()=>{
        // quick CSV export of positions
        const rows = [['Asset','Type','Volume','Price','Profit','Date']];
        (initialData.positions||[]).forEach(p=>rows.push([p.asset,p.type,p.volume,p.price,p.profit,p.date]));
        const csv = rows.map(r=>r.join(',')).join('\n');
        const blob = new Blob([csv],{type:'text/csv'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a'); a.href = url; a.download = 'positions.csv'; a.click(); URL.revokeObjectURL(url);
      });

      document.getElementById('saveNote').addEventListener('click', ()=>{
        const note = document.getElementById('quickNote').value;
        alert('Note saved: '+ (note? note : '(empty)'));
      });

      // initial render
      render(initialData);
    })();
  </script>
</body>
</html>
