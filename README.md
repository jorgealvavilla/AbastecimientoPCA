# AbastecimientoPCA
html
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Dashboard de Abastecimiento - Gobiernos Locales</title>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <!-- html2pdf Library for PDF Export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js" integrity="sha512-GsLlZN/3F2ErC5ifS5QtgpiJtWd43JWSuIgh7mbzZ8zBps+dvLusV+eNQATqgA/HdeKFVgA5v3S/cIrLF7QnIg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <!-- Font Awesome (opcional, para iconos) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f0f2f5;
            padding: 20px;
            color: #1a1a2e;
        }

        .dashboard-container {
            max-width: 1600px;
            margin: 0 auto;
        }

        /* Header */
        .header {
            background: linear-gradient(135deg, #0b2b3b, #1a4b6e);
            color: white;
            padding: 20px 25px;
            border-radius: 20px;
            margin-bottom: 25px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        .header h1 {
            font-weight: 600;
            font-size: 1.8rem;
            letter-spacing: -0.5px;
        }
        .header p {
            opacity: 0.9;
            font-size: 0.9rem;
            margin-top: 5px;
        }
        .last-update {
            background: rgba(255,255,255,0.2);
            padding: 8px 15px;
            border-radius: 40px;
            font-size: 0.85rem;
            font-weight: 500;
        }

        /* Filtros */
        .filters-card {
            background: white;
            border-radius: 20px;
            padding: 15px 20px;
            margin-bottom: 25px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            align-items: flex-end;
        }
        .filter-group {
            flex: 1;
            min-width: 180px;
        }
        .filter-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 6px;
            color: #2c3e66;
            font-size: 0.85rem;
        }
        .filter-group select, .filter-group input {
            width: 100%;
            padding: 10px 12px;
            border-radius: 12px;
            border: 1px solid #ccdbe8;
            background: #fefefe;
            font-size: 0.9rem;
            transition: all 0.2s;
        }
        .filter-group select:focus, .filter-group input:focus {
            outline: none;
            border-color: #1a4b6e;
            box-shadow: 0 0 0 2px rgba(26,75,110,0.2);
        }
        .btn-reset {
            background: #e9ecef;
            border: none;
            padding: 10px 20px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            color: #2c3e66;
        }
        .btn-reset:hover {
            background: #cdd5df;
        }

        /* Grid de gráficos y tablas */
        .grid-2col {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 24px;
            margin-bottom: 24px;
        }
        .card {
            background: white;
            border-radius: 24px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            padding: 18px 20px 20px 20px;
            transition: all 0.2s;
            overflow: hidden;
        }
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 2px solid #eef2f8;
            padding-bottom: 12px;
            flex-wrap: wrap;
        }
        .card-header h3 {
            font-size: 1.3rem;
            font-weight: 600;
            color: #1f3a4b;
        }
        .btn-pdf {
            background: none;
            border: none;
            font-size: 0.8rem;
            color: #1a4b6e;
            cursor: pointer;
            font-weight: 500;
            padding: 6px 12px;
            border-radius: 30px;
            transition: 0.2s;
        }
        .btn-pdf i {
            margin-right: 5px;
        }
        .btn-pdf:hover {
            background: #eef2fa;
        }
        .full-width {
            grid-column: span 2;
        }
        .table-responsive {
            overflow-x: auto;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.8rem;
        }
        th, td {
            text-align: left;
            padding: 12px 8px;
            border-bottom: 1px solid #e2e8f0;
        }
        th {
            background: #f8fafc;
            font-weight: 600;
            color: #1e3a5f;
        }
        tr:hover {
            background: #f9fcff;
        }
        .badge {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.7rem;
            font-weight: 600;
        }
        .badge-abast {
            background: #d1fae5;
            color: #065f46;
        }
        .badge-desab {
            background: #ffe4e2;
            color: #9b2c1d;
        }
        .badge-sd {
            background: #f1f3f4;
            color: #4b5563;
        }
        canvas {
            max-height: 320px;
            width: 100%;
        }
        footer {
            text-align: center;
            margin-top: 35px;
            font-size: 0.75rem;
            color: #5c6f87;
        }
        @media (max-width: 800px) {
            .grid-2col {
                grid-template-columns: 1fr;
            }
            .full-width {
                grid-column: span 1;
            }
            body {
                padding: 12px;
            }
        }
        .loading {
            text-align: center;
            padding: 40px;
            color: #1f6392;
        }
    </style>
</head>
<body>
<div class="dashboard-container" id="dashboardApp">
    <div class="header">
        <div>
            <h1><i class="fas fa-chart-line"></i> Monitoreo de Abastecimiento</h1>
            <p>Gobiernos locales · Departamentos · Estado de compras</p>
        </div>
        <div class="last-update" id="lastUpdateText">
            <i class="fas fa-sync-alt"></i> Cargando datos...
        </div>
    </div>

    <!-- Filtros -->
    <div class="filters-card">
        <div class="filter-group">
            <label><i class="fas fa-building"></i> Departamento</label>
            <select id="filterDept">
                <option value="all">Todos los departamentos</option>
            </select>
        </div>
        <div class="filter-group">
            <label><i class="fas fa-city"></i> Gobierno Local</label>
            <input type="text" id="filterMunicipio" placeholder="Escribir nombre..." autocomplete="off">
        </div>
        <div class="filter-group">
            <button class="btn-reset" id="resetFiltersBtn"><i class="fas fa-undo-alt"></i> Limpiar filtros</button>
        </div>
    </div>

    <!-- Gráficos principales -->
    <div class="grid-2col">
        <div class="card" id="card-phaseChart">
            <div class="card-header">
                <h3><i class="fas fa-chart-pie"></i> Fase actual del proceso de compra</h3>
                <button class="btn-pdf pdf-export-btn" data-target="phaseChart"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            </div>
            <canvas id="phaseChart" style="width:100%; max-height:300px"></canvas>
        </div>
        <div class="card" id="card-startChart">
            <div class="card-header">
                <h3><i class="fas fa-calendar-alt"></i> Inicio de proceso de compra</h3>
                <button class="btn-pdf pdf-export-btn" data-target="startChart"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            </div>
            <canvas id="startChart" style="width:100%; max-height:300px"></canvas>
        </div>
    </div>

    <!-- Gráfico de barras por mes: abastecido / desabastecido -->
    <div class="card full-width" id="card-monthlyBar">
        <div class="card-header">
            <h3><i class="fas fa-chart-bar"></i> Evolución mensual: Gobiernos abastecidos vs desabastecidos</h3>
            <button class="btn-pdf pdf-export-btn" data-target="monthlyBar"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
        </div>
        <canvas id="monthlyBarChart" style="width:100%; max-height:380px"></canvas>
        <div class="table-responsive" style="margin-top: 15px;">
            <small style="color:#4b6f8e;">Detalle mensual: cantidad de gobiernos locales por estado</small>
            <div id="monthlyDetailTable"></div>
        </div>
    </div>

    <!-- Listas Top & tabla completa -->
    <div class="grid-2col">
        <div class="card" id="cardTopAbast">
            <div class="card-header">
                <h3><i class="fas fa-trophy"></i> Top 10 Gobiernos Abastecidos (últimos 3 meses)</h3>
                <button class="btn-pdf pdf-export-btn" data-target="topAbastList"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            </div>
            <div id="topAbastList" class="table-responsive"></div>
        </div>
        <div class="card" id="cardTopDesab">
            <div class="card-header">
                <h3><i class="fas fa-exclamation-triangle"></i> Top 10 Desabastecidos (últimos 2 meses)</h3>
                <button class="btn-pdf pdf-export-btn" data-target="topDesabList"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            </div>
            <div id="topDesabList" class="table-responsive"></div>
        </div>
    </div>

    <!-- Listado completo de gobiernos locales con detalle mensual -->
    <div class="card full-width" id="cardFullList">
        <div class="card-header">
            <h3><i class="fas fa-table-list"></i> Reporte completo: Gobiernos locales · Inicio de proceso · Estado último mes y compras mensuales</h3>
            <button class="btn-pdf pdf-export-btn" data-target="fullTable"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
        </div>
        <div id="fullTable" class="table-responsive"></div>
    </div>
    <footer>Datos actualizados en tiempo real desde hoja pública de Google Sheets. Dashboard interactivo con filtros dinámicos.</footer>
</div>

<script>
    // 1. CONFIGURACIÓN URL PÚBLICA CSV
    const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRFoAeenC2uzJj9fz5tRLof0PP6pJX7qGlz6K1KmFhj6BROoShKV5sCVBQgeDpBNwmxtmmUQ15eFDjj/pub?gid=1704897024&single=true&output=csv";
    // Usamos proxy CORS gratuito para evitar bloqueos (puede cambiarse si la URL original funciona sin CORS)
    const PROXY_URL = "https://api.allorigins.win/raw?url=";
    const FETCH_URL = PROXY_URL + encodeURIComponent(CSV_URL);

    let rawData = [];        // Array de objetos con todos los campos
    let filteredData = [];

    // Chart instances
    let phaseChartObj, startChartObj, monthlyBarChartObj;

    // Helper: parse CSV básico
    async function fetchData() {
        try {
            const response = await fetch(FETCH_URL);
            if (!response.ok) throw new Error(`HTTP ${response.status}`);
            const csvText = await response.text();
            const rows = csvText.trim().split(/\r?\n/);
            if(rows.length < 2) throw new Error("CSV vacío");
            const headers = rows[0].split(",").map(h => h.replace(/["']/g, '').trim());
            // Mapeamos índices esperados según estructura: Departamento, Gobierno Local, Fase Actual del Proceso de Compra, Inicio de Proceso de Compra (yyyy-mm-dd), Fecha Actualización, Estado
            // Ajuste dinámico en base a los títulos encontrados
            const idxDept = headers.findIndex(h => h.toLowerCase().includes("departamento"));
            const idxMunicipio = headers.findIndex(h => h.toLowerCase().includes("gobierno local") || h.toLowerCase().includes("municipio"));
            const idxFase = headers.findIndex(h => h.toLowerCase().includes("fase actual"));
            const idxInicio = headers.findIndex(h => h.toLowerCase().includes("inicio de proceso"));
            const idxFechaAct = headers.findIndex(h => h.toLowerCase().includes("fecha actualización"));
            const idxEstado = headers.findIndex(h => h.toLowerCase().includes("estado"));
            const dataRows = [];
            for(let i=1; i<rows.length; i++) {
                // parse CSV simple respetando comillas
                let row = rows[i];
                let cols = [];
                let inQuote = false;
                let current = "";
                for(let ch of row) {
                    if(ch === '"') inQuote = !inQuote;
                    else if(ch === ',' && !inQuote) {
                        cols.push(current.trim());
                        current = "";
                    } else current += ch;
                }
                cols.push(current.trim());
                if(cols.length < headers.length) continue;
                const dept = (idxDept>=0 ? cols[idxDept] : "").replace(/["']/g, '').trim();
                const municipio = (idxMunicipio>=0 ? cols[idxMunicipio] : "").replace(/["']/g, '').trim();
                const fase = (idxFase>=0 ? cols[idxFase] : "").replace(/["']/g, '').trim();
                let inicioProceso = (idxInicio>=0 ? cols[idxInicio] : "").replace(/["']/g, '').trim();
                let fechaAct = (idxFechaAct>=0 ? cols[idxFechaAct] : "").replace(/["']/g, '').trim();
                let estadoRaw = (idxEstado>=0 ? cols[idxEstado] : "").replace(/["']/g, '').trim();
                // estandarizar estado
                let estado = "";
                if(estadoRaw.toLowerCase().includes("abastecido")) estado = "Abastecido";
                else if(estadoRaw.toLowerCase().includes("desabastecido")) estado = "Desabastecido";
                else estado = "Sin dato";
                // validar fila mínima
                if(municipio === "" && dept === "") continue;
                dataRows.push({
                    departamento: dept || "Sin especificar",
                    gobiernoLocal: municipio || "No especificado",
                    faseActual: fase || "No registrada",
                    inicioProceso: inicioProceso,
                    fechaActualizacion: fechaAct,
                    estado: estado,
                });
            }
            rawData = dataRows;
            document.getElementById("lastUpdateText").innerHTML = `<i class="fas fa-check-circle"></i> Actualizado: ${new Date().toLocaleString()}`;
            applyFilters();
        } catch(error) {
            console.error("Error cargando datos", error);
            document.getElementById("lastUpdateText").innerHTML = `<i class="fas fa-exclamation-triangle"></i> Error al cargar datos, reintentando...`;
            setTimeout(() => fetchData(), 5000);
        }
    }

    // Filtros
    function applyFilters() {
        let deptValue = document.getElementById("filterDept").value;
        let municipioText = document.getElementById("filterMunicipio").value.toLowerCase().trim();
        filteredData = rawData.filter(row => {
            if(deptValue !== "all" && row.departamento !== deptValue) return false;
            if(municipioText !== "" && !row.gobiernoLocal.toLowerCase().includes(municipioText)) return false;
            return true;
        });
        actualizarDashboards();
    }

    // actualizar todos los componentes
    function actualizarDashboards() {
        // 1. Fase actual (pie/doughnut)
        const faseMap = new Map();
        filteredData.forEach(d => { faseMap.set(d.faseActual, (faseMap.get(d.faseActual)||0)+1); });
        const fasesLabels = Array.from(faseMap.keys());
        const fasesCounts = Array.from(faseMap.values());
        if(phaseChartObj) phaseChartObj.destroy();
        const ctxPhase = document.getElementById('phaseChart').getContext('2d');
        phaseChartObj = new Chart(ctxPhase, {
            type: 'doughnut',
            data: { labels: fasesLabels, datasets: [{ data: fasesCounts, backgroundColor: ['#2c6e9e','#4d9b76','#e9a23b','#b56576','#6c91b2'] }] },
            options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom' }, tooltip: { callbacks: { label: (ctx) => `${ctx.label}: ${ctx.raw} (${((ctx.raw/filteredData.length)*100).toFixed(1)}%)` } } } }
        });

        // 2. Inicio de proceso de compra: agrupar por año? (mostramos años principales)
        const inicioYearMap = new Map();
        filteredData.forEach(d => {
            let year = "Sin fecha";
            if(d.inicioProceso && d.inicioProceso.match(/\d{4}/)) year = d.inicioProceso.match(/\d{4}/)[0];
            inicioYearMap.set(year, (inicioYearMap.get(year)||0)+1);
        });
        const startLabels = Array.from(inicioYearMap.keys());
        const startCounts = Array.from(inicioYearMap.values());
        if(startChartObj) startChartObj.destroy();
        const ctxStart = document.getElementById('startChart').getContext('2d');
        startChartObj = new Chart(ctxStart, { type: 'bar', data: { labels: startLabels, datasets: [{ label: 'Procesos iniciados', data: startCounts, backgroundColor: '#1f7a8c' }] }, options: { responsive: true, maintainAspectRatio: true } });

        // 3. Gráfico de barras por mes y estado (abastecido/desabastecido/sin dato)
        const monthStatus = new Map(); // key: "YYYY-MM" , value: {abast, desab, nodata}
        filteredData.forEach(d => {
            let dateRaw = d.fechaActualizacion;
            let monthKey = "Sin fecha";
            if(dateRaw && dateRaw.match(/\d{1,2}\/\d{1,2}\/\d{4}/)) {
                let parts = dateRaw.split('/');
                if(parts.length===3) monthKey = `${parts[2]}-${parts[1].padStart(2,'0')}`;
            }
            if(!monthStatus.has(monthKey)) monthStatus.set(monthKey, {abast:0, desab:0, nodata:0});
            let stat = monthStatus.get(monthKey);
            if(d.estado === "Abastecido") stat.abast++;
            else if(d.estado === "Desabastecido") stat.desab++;
            else stat.nodata++;
        });
        const sortedMonths = Array.from(monthStatus.keys()).sort().slice(-12);
        const abastArr = [], desabArr = [], nodataArr = [];
        sortedMonths.forEach(m => { let s = monthStatus.get(m); abastArr.push(s.abast); desabArr.push(s.desab); nodataArr.push(s.nodata); });
        if(monthlyBarChartObj) monthlyBarChartObj.destroy();
        const ctxBar = document.getElementById('monthlyBarChart').getContext('2d');
        monthlyBarChartObj = new Chart(ctxBar, { type: 'bar', data: { labels: sortedMonths, datasets: [{ label: 'Abastecidos', data: abastArr, backgroundColor: '#2b8c5e' },{ label: 'Desabastecidos', data: desabArr, backgroundColor: '#cd5c5c' },{ label: 'Sin dato', data: nodataArr, backgroundColor: '#b0b0b0' }] }, options: { responsive: true, maintainAspectRatio: true, scales: { x: { title: { display: true, text: 'Mes - Año' } }, y: { title: { display: true, text: 'N° Gobiernos Locales' } } } } });
        // Detalle tabla mensual pequeña
        let htmlDetail = `<table style="font-size:0.75rem"><thead><tr><th>Mes/Año</th><th>Abastecidos</th><th>Desabastecidos</th><th>Sin dato</th></tr></thead><tbody>`;
        sortedMonths.forEach(m => { let s = monthStatus.get(m); htmlDetail+=`<tr><td>${m}</td><td>${s.abast}</td><td>${s.desab}</td><td>${s.nodata}</td></tr>`; });
        htmlDetail+=`</tbody></table>`;
        document.getElementById("monthlyDetailTable").innerHTML = htmlDetail;

        // 4. Top 10 abastecidos últimos 3 meses
        // Obtener últimos 3 meses únicos presentes en datos
        let allMonths = Array.from(monthStatus.keys()).sort().filter(m=> m!="Sin fecha");
        let last3Months = allMonths.slice(-3);
        const countAbastUlt3 = new Map();
        filteredData.forEach(d => {
            let dateRaw = d.fechaActualizacion;
            let monthKey = "Sin fecha";
            if(dateRaw && dateRaw.match(/\d{1,2}\/\d{1,2}\/\d{4}/)) {
                let parts = dateRaw.split('/');
                monthKey = `${parts[2]}-${parts[1].padStart(2,'0')}`;
            }
            if(last3Months.includes(monthKey) && d.estado === "Abastecido") {
                let gl = d.gobiernoLocal;
                countAbastUlt3.set(gl, (countAbastUlt3.get(gl)||0)+1);
            }
        });
        let topAbastSorted = Array.from(countAbastUlt3.entries()).sort((a,b)=>b[1]-a[1]).slice(0,10);
        let htmlTopAbast = `<table><thead><tr><th>Gobierno Local</th><th>Veces abastecido (últimos 3 meses)</th></tr></thead><tbody>${topAbastSorted.map(([g,c]) => `<tr><td>${g}</td><td>${c}</td></tr>`).join('')||'<tr><td colspan="2">No hay datos</td></tr>'}</tbody></table>`;
        document.getElementById("topAbastList").innerHTML = htmlTopAbast;

        // Top 10 desabastecidos últimos 2 meses
        let last2Months = allMonths.slice(-2);
        const countDesabUlt2 = new Map();
        filteredData.forEach(d => {
            let dateRaw = d.fechaActualizacion;
            let monthKey = "Sin fecha";
            if(dateRaw && dateRaw.match(/\d{1,2}\/\d{1,2}\/\d{4}/)) {
                let parts = dateRaw.split('/');
                monthKey = `${parts[2]}-${parts[1].padStart(2,'0')}`;
            }
            if(last2Months.includes(monthKey) && d.estado === "Desabastecido") {
                let gl = d.gobiernoLocal;
                countDesabUlt2.set(gl, (countDesabUlt2.get(gl)||0)+1);
            }
        });
        let topDesabSorted = Array.from(countDesabUlt2.entries()).sort((a,b)=>b[1]-a[1]).slice(0,10);
        let htmlTopDesab = `<table><thead><tr><th>Gobierno Local</th><th>Veces desabastecido (últimos 2 meses)</th></tr></thead><tbody>${topDesabSorted.map(([g,c]) => `<tr><td>${g}</td><td>${c}</td></tr>`).join('')||'<tr><td colspan="2">No hay datos</td></tr>'}</tbody></table>`;
        document.getElementById("topDesabList").innerHTML = htmlTopDesab;

        // 5. Tabla completa: mostrar gobierno local, inicio de proceso, estado último mes y compras mensuales
        // Estado último mes: tomamos el último mes disponible del dataset, para cada gobierno local mostramos estado en ese mes
        let lastMonthAvailable = allMonths.filter(m=>m!="Sin fecha").slice(-1)[0] || null;
        let monthlyStatusMap = new Map(); // key: gobiernoLocal -> objeto con map mes->estado
        filteredData.forEach(d => {
            let dateRaw = d.fechaActualizacion;
            let monthKey = "Sin fecha";
            if(dateRaw && dateRaw.match(/\d{1,2}\/\d{1,2}\/\d{4}/)) {
                let parts = dateRaw.split('/');
                monthKey = `${parts[2]}-${parts[1].padStart(2,'0')}`;
            }
            let gl = d.gobiernoLocal;
            if(!monthlyStatusMap.has(gl)) monthlyStatusMap.set(gl, new Map());
            monthlyStatusMap.get(gl).set(monthKey, d.estado);
        });
        let fullRows = [];
        let uniqueGL = [...new Set(filteredData.map(d=>d.gobiernoLocal))];
        for(let gl of uniqueGL) {
            let primerInicio = filteredData.find(d=>d.gobiernoLocal===gl)?.inicioProceso || "-";
            let lastMonthState = lastMonthAvailable ? (monthlyStatusMap.get(gl)?.get(lastMonthAvailable) || "Sin dato") : "Sin dato";
            let mesesCompra = Array.from(monthlyStatusMap.get(gl)?.entries() || []).map(([m,est]) => `${m} (${est})`).join(", ");
            fullRows.push({ gl, inicio: primerInicio, estadoUltimoMes: lastMonthState, resumenMensual: mesesCompra || "Sin registros" });
        }
        let htmlFull = `<table><thead><tr><th>Gobierno Local</th><th>Inicio Proceso Compra</th><th>Estado último mes (${lastMonthAvailable||"N/D"})</th><th>Compras por mes (estado)</th></tr></thead><tbody>${fullRows.map(r => `<tr><td>${r.gl}</td><td>${r.inicio}</td><td><span class="badge ${r.estadoUltimoMes==='Abastecido'?'badge-abast':r.estadoUltimoMes==='Desabastecido'?'badge-desab':'badge-sd'}">${r.estadoUltimoMes}</span></td><td style="max-width:280px; word-break:break-word;">${r.resumenMensual.substring(0,180)}</td></tr>`).join('')}</tbody></table>`;
        document.getElementById("fullTable").innerHTML = htmlFull;
    }

    // poblar filtro departamentos
    function populateDeptFilter() {
        let depts = [...new Set(rawData.map(d=>d.departamento))].sort();
        let select = document.getElementById("filterDept");
        select.innerHTML = '<option value="all">Todos los departamentos</option>';
        depts.forEach(d => { let opt = document.createElement("option"); opt.value=d; opt.textContent=d; select.appendChild(opt); });
    }

    // Event Listeners
    document.getElementById("filterDept").addEventListener("change", applyFilters);
    document.getElementById("filterMunicipio").addEventListener("input", applyFilters);
    document.getElementById("resetFiltersBtn").addEventListener("click", () => {
        document.getElementById("filterDept").value = "all";
        document.getElementById("filterMunicipio").value = "";
        applyFilters();
    });

    // Exports PDF individual para cada bloque: usando html2pdf
    function exportCardAsPDF(elementId, filename) {
        const element = document.getElementById(elementId);
        if(!element) return;
        const clone = element.cloneNode(true);
        clone.style.padding = "15px";
        clone.style.backgroundColor = "#fff";
        const opt = { margin: [0.5,0.5,0.5,0.5], filename: `${filename}.pdf`, image: { type: 'jpeg', quality: 0.98 }, html2canvas: { scale: 2, useCORS: true, letterRendering: true }, jsPDF: { unit: 'in', format: 'a4', orientation: 'landscape' } };
        html2pdf().set(opt).from(clone).save();
    }
    document.querySelectorAll('.pdf-export-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
            let target = btn.getAttribute('data-target');
            if(target === 'phaseChart') exportCardAsPDF('card-phaseChart', 'Fase_Proceso');
            else if(target === 'startChart') exportCardAsPDF('card-startChart', 'Inicio_Proceso');
            else if(target === 'monthlyBar') exportCardAsPDF('card-monthlyBar', 'Evolucion_Mensual');
            else if(target === 'topAbastList') exportCardAsPDF('cardTopAbast', 'Top_Abastecidos');
            else if(target === 'topDesabList') exportCardAsPDF('cardTopDesab', 'Top_Desabastecidos');
            else if(target === 'fullTable') exportCardAsPDF('cardFullList', 'Reporte_Completo');
        });
    });

    // Carga inicial
    fetchData().then(() => { populateDeptFilter(); applyFilters(); });
</script>
</body>
</html>
