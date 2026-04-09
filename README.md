# Trading-Journal- <!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FuturesEdge — GitHub Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Sora:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #0d0f1a;
            --surface: #141725;
            --surface2: #1c2035;
            --border: rgba(255,255,255,0.07);
            --text: #e8eaf6;
            --muted: #7b82a8;
            --cyan: #00e5ff;
            --green: #00e676;
            --red: #ff1744;
            --font: 'Sora', sans-serif;
            --mono: 'Space Mono', monospace;
        }

        * { margin:0; padding:0; box-sizing:border-box; }
        body { background:var(--bg); color:var(--text); font-family:var(--font); line-height: 1.5; }

        /* Layout */
        .app-container { display: flex; min-height: 100vh; }
        
        /* Sidebar mejorada para GitHub */
        .sidebar { 
            width: 240px; 
            background: var(--surface); 
            border-right: 1px solid var(--border);
            padding: 20px;
            display: flex;
            flex-direction: column;
            position: fixed;
            height: 100vh;
        }

        .main-content { margin-left: 240px; flex: 1; padding: 30px; }

        .logo { margin-bottom: 30px; }
        .logo h1 { font-size: 22px; font-weight: 800; }
        .logo span { color: var(--cyan); }

        /* Componentes de la interfaz */
        .stats-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); 
            gap: 20px; 
            margin-bottom: 30px; 
        }

        .stat-card { 
            background: var(--surface); 
            padding: 20px; 
            border-radius: 12px; 
            border: 1px solid var(--border); 
        }

        .stat-label { font-size: 12px; color: var(--muted); text-transform: uppercase; font-family: var(--mono); }
        .stat-value { font-size: 24px; font-weight: 700; font-family: var(--mono); margin-top: 5px; }

        /* Botones Estilo Moderno */
        .btn {
            padding: 10px 18px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: 600;
            font-family: var(--font);
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary { background: var(--cyan); color: #000; }
        .btn-secondary { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
        .btn:hover { opacity: 0.9; transform: translateY(-1px); }

        /* Tabla de Trades */
        .card { background: var(--surface); border-radius: 16px; border: 1px solid var(--border); overflow: hidden; }
        table { width: 100%; border-collapse: collapse; }
        th { text-align: left; padding: 15px; background: rgba(255,255,255,0.02); color: var(--muted); font-size: 12px; font-family: var(--mono); }
        td { padding: 15px; border-bottom: 1px solid var(--border); font-size: 14px; }

        .pnl-pos { color: var(--green); }
        .pnl-neg { color: var(--red); }

        /* Modal simple para GitHub */
        .modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background: var(--surface);
            padding: 30px;
            border-radius: 20px;
            width: 100%;
            max-width: 500px;
            border: 1px solid var(--border);
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            margin: 10px 0 20px;
            background: var(--bg);
            border: 1px solid var(--border);
            color: #fff;
            border-radius: 8px;
        }

        @media (max-width: 768px) {
            .app-container { flex-direction: column; }
            .sidebar { width: 100%; height: auto; position: relative; }
            .main-content { margin-left: 0; }
        }
    </style>
</head>
<body>

<div class="app-container">
    <aside class="sidebar">
        <div class="logo">
            <h1>GitHub<span>Edge</span></h1>
            <p style="font-size: 10px; color: var(--muted)">ESTADÍSTICAS EN VIVO</p>
        </div>
        
        <button class="btn btn-primary" onclick="openModal()" style="margin-bottom: 10px;">+ Nuevo Trade</button>
        <button class="btn btn-secondary" onclick="exportData()" style="margin-bottom: 10px;">📥 Exportar JSON</button>
        <input type="file" id="importFile" style="display:none" onchange="importData(event)">
        <button class="btn btn-secondary" onclick="document.getElementById('importFile').click()">📤 Importar</button>

        <div style="margin-top: auto; padding-top: 20px; border-top: 1px solid var(--border);">
            <p style="font-size: 11px; color: var(--muted)">Status: <span style="color: var(--green)">Online</span></p>
        </div>
    </aside>

    <main class="main-content">
        <div class="stats-grid">
            <div class="stat-card">
                <div class="stat-label">P&L Acumulado</div>
                <div class="stat-value" id="totalPnl">$0.00</div>
            </div>
            <div class="stat-card">
                <div class="stat-label">Win Rate</div>
                <div class="stat-value" id="winRate">0%</div>
            </div>
            <div class="stat-card">
                <div class="stat-label">Total Trades</div>
                <div class="stat-value" id="totalTrades">0</div>
            </div>
        </div>

        <div class="card">
            <table id="tradesTable">
                <thead>
                    <tr>
                        <th>FECHA</th>
                        <th>ACTIVO</th>
                        <th>TIPO</th>
                        <th>RESULTADO</th>
                        <th>P&L</th>
                        <th>ACCIONES</th>
                    </tr>
                </thead>
                <tbody id="tradesBody">
                    </tbody>
            </table>
        </div>
    </main>
</div>

<div id="tradeModal" class="modal">
    <div class="modal-content">
        <h2 style="margin-bottom: 20px;">Registrar Operación</h2>
        <label>Activo (Ej: BTC/USDT)</label>
        <input type="text" id="symbol" placeholder="BTC/USDT">
        
        <label>Tipo</label>
        <select id="side">
            <option value="Long">Long</option>
            <option value="Short">Short</option>
        </select>

        <label>P&L ($)</label>
        <input type="number" id="pnl" placeholder="Ej: 150 o -50">

        <div style="display: flex; gap: 10px; justify-content: flex-end;">
            <button class="btn btn-secondary" onclick="closeModal()">Cancelar</button>
            <button class="btn btn-primary" onclick="saveTrade()">Guardar</button>
        </div>
    </div>
</div>

<script>
    let trades = JSON.parse(localStorage.getItem('github_trades')) || [];

    function openModal() { document.getElementById('tradeModal').style.display = 'flex'; }
    function closeModal() { document.getElementById('tradeModal').style.display = 'none'; }

    function saveTrade() {
        const symbol = document.getElementById('symbol').value;
        const side = document.getElementById('side').value;
        const pnl = parseFloat(document.getElementById('pnl').value);

        if (!symbol || isNaN(pnl)) return alert("Completa los datos");

        const trade = {
            id: Date.now(),
            date: new Date().toLocaleDateString(),
            symbol,
            side,
            pnl
        };

        trades.unshift(trade);
        updateData();
        closeModal();
        
        // Limpiar campos
        document.getElementById('symbol').value = '';
        document.getElementById('pnl').value = '';
    }

    function deleteTrade(id) {
        trades = trades.filter(t => t.id !== id);
        updateData();
    }

    function updateData() {
        localStorage.setItem('github_trades', JSON.stringify(trades));
        renderTrades();
        calculateStats();
    }

    function renderTrades() {
        const body = document.getElementById('tradesBody');
        body.innerHTML = trades.map(t => `
            <tr>
                <td>${t.date}</td>
                <td style="font-weight: bold;">${t.symbol}</td>
                <td><span style="color: ${t.side === 'Long' ? 'var(--green)' : 'var(--red)'}">${t.side}</span></td>
                <td>${t.pnl >= 0 ? '✅ Win' : '❌ Loss'}</td>
                <td class="${t.pnl >= 0 ? 'pnl-pos' : 'pnl-neg'}">${t.pnl >= 0 ? '+' : ''}$${t.pnl.toFixed(2)}</td>
                <td><button onclick="deleteTrade(${t.id})" style="background:none; border:none; color:var(--muted); cursor:pointer;">Eliminar</button></td>
            </tr>
        `).join('');
    }

    function calculateStats() {
        const total = trades.reduce((acc, t) => acc + t.pnl, 0);
        const wins = trades.filter(t => t.pnl > 0).length;
        const wr = trades.length > 0 ? (wins / trades.length * 100).toFixed(1) : 0;

        document.getElementById('totalPnl').textContent = `${total >= 0 ? '+' : ''}$${total.toFixed(2)}`;
        document.getElementById('totalPnl').className = `stat-value ${total >= 0 ? 'pnl-pos' : 'pnl-neg'}`;
        document.getElementById('winRate').textContent = `${wr}%`;
        document.getElementById('totalTrades').textContent = trades.length;
    }

    // Funciones de Backup para GitHub
    function exportData() {
        const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(trades));
        const downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute("href", dataStr);
        downloadAnchorNode.setAttribute("download", "trading_journal_backup.json");
        document.body.appendChild(downloadAnchorNode);
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
    }

    function importData(event) {
        const reader = new FileReader();
        reader.onload = function(e) {
            trades = JSON.parse(e.target.result);
            updateData();
        };
        reader.readAsText(event.target.files[0]);
    }

    // Inicializar
    renderTrades();
    calculateStats();
</script>

</body>
</html>
