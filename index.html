<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Monitoreo - Lima</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="bg-gray-900 text-gray-100 min-h-screen font-sans">

    <div class="max-w-7xl mx-auto px-4 py-8">
        <div class="flex justify-between items-center mb-10">
            <div>
                <h1 class="text-3xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-emerald-400">
                    Panel de Monitoreo de Medios
                </h1>
                <p class="text-gray-400 mt-1">Sincronizado con Google Sheets</p>
            </div>
            <div id="status" class="px-3 py-1 rounded-full bg-emerald-500/10 text-emerald-400 text-xs border border-emerald-500/20 flex items-center gap-2">
                <span class="relative flex h-2 w-2">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                </span>
                En vivo
            </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-10">
            <div class="bg-gray-800 p-6 rounded-2xl border border-gray-700 shadow-xl">
                <p class="text-gray-400 text-sm font-medium uppercase tracking-wider">Menciones este mes</p>
                <div class="flex items-end justify-between mt-2">
                    <h2 id="total-mes" class="text-5xl font-bold italic tracking-tighter">--</h2>
                    <div id="comparativo-container" class="mb-1">
                        <span id="comparativo-val" class="px-2 py-1 rounded text-sm font-bold tracking-tight">--</span>
                    </div>
                </div>
                <p class="text-gray-500 text-xs mt-4 italic">vs. mes anterior: <span id="total-pasado">--</span></p>
            </div>

            <div class="bg-gray-800 p-6 rounded-2xl border border-gray-700 shadow-xl">
                <p class="text-gray-400 text-sm font-medium uppercase tracking-wider">Sentimiento Hoy</p>
                <div class="mt-2 flex items-center gap-4">
                    <div id="emoji-sentimiento" class="text-5xl italic font-black">--</div>
                    <div>
                        <h2 id="status-texto" class="text-xl font-bold">Analizando...</h2>
                    </div>
                </div>
            </div>

            <div class="bg-gray-800 p-6 rounded-2xl border border-gray-700 shadow-xl overflow-hidden relative">
                <div class="relative z-10">
                    <p class="text-gray-400 text-sm font-medium uppercase tracking-wider">Casos de Urgencia "ALTA"</p>
                    <h2 id="urgencia-total" class="text-5xl font-bold mt-2">--</h2>
                </div>
                <div class="absolute right-0 bottom-0 opacity-10">
                    <i data-lucide="alert-triangle" class="w-24 h-24 text-red-500"></i>
                </div>
            </div>
        </div>

        <div class="bg-gray-800 p-8 rounded-3xl border border-gray-700 shadow-2xl">
            <h3 class="text-lg font-semibold mb-6 flex items-center gap-2 italic">
                <i data-lucide="trending-up" class="w-5 h-5 text-blue-400"></i>
                Línea de Tiempo: Menciones Diarias
            </h3>
            <div class="h-[400px]">
                <canvas id="mencionesChart"></canvas>
            </div>
        </div>
    </div>

    <script>
        const CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vT2khLH7C3lCFCIV3ss1U5AV4SOL922Sld2rnPhSayp39VqgtlBV8ey63dxgCbur1nHiK-6CfNKB2B-/pub?gid=817460174&single=true&output=csv';

        async function init() {
            lucide.createIcons();
            
            Papa.parse(CSV_URL, {
                download: true,
                header: true,
                complete: function(results) {
                    processData(results.data);
                }
            });
        }

        function processData(data) {
            const today = new Date();
            const currentMonth = today.getMonth();
            const currentYear = today.getFullYear();
            
            let countMesActual = 0;
            let countMesPasado = 0;
            let urgenciaAlta = 0;
            let timeline = {};

            data.forEach(row => {
                const fechaStr = row['FECHA DE PUBLICACIÓN'];
                if(!fechaStr) return;

                // Parseo de fecha DD/MM/YYYY
                const parts = fechaStr.split('/');
                const date = new Date(parts[2], parts[1] - 1, parts[0]);
                
                if(isNaN(date.getTime())) return;

                const rowMonth = date.getMonth();
                const rowYear = date.getFullYear();

                // Conteo mensual
                if(rowMonth === currentMonth && rowYear === currentYear) countMesActual++;
                if(rowMonth === (currentMonth - 1 === -1 ? 11 : currentMonth - 1)) {
                   if(rowYear === (currentMonth === 0 ? currentYear - 1 : currentYear)) countMesPasado++;
                }

                // Urgencia
                if(row['URGENCIA'] === 'ALTA') urgenciaAlta++;

                // Agrupar para gráfico
                const key = date.toISOString().split('T')[0];
                timeline[key] = (timeline[key] || 0) + 1;
            });

            // Actualizar UI
            document.getElementById('total-mes').innerText = countMesActual;
            document.getElementById('total-pasado').innerText = countMesPasado;
            document.getElementById('urgencia-total').innerText = urgenciaAlta;

            // Comparativo
            const diff = countMesActual - countMesPasado;
            const perc = countMesPasado > 0 ? (diff / countMesPasado * 100).toFixed(1) : 100;
            const compEl = document.getElementById('comparativo-val');
            compEl.innerText = `${diff >= 0 ? '+' : ''}${perc}%`;
            compEl.className = diff >= 0 ? 'bg-emerald-500/20 text-emerald-400 px-2 py-1 rounded text-sm font-bold' : 'bg-red-500/20 text-red-400 px-2 py-1 rounded text-sm font-bold';

            renderChart(timeline);
        }

        function renderChart(timelineData) {
            const labels = Object.keys(timelineData).sort();
            const values = labels.map(l => timelineData[l]);

            const ctx = document.getElementById('mencionesChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Menciones Diarias',
                        data: values,
                        borderColor: '#60a5fa',
                        borderWidth: 4,
                        pointBackgroundColor: '#60a5fa',
                        pointRadius: 4,
                        tension: 0.4,
                        fill: true,
                        backgroundColor: 'rgba(96, 165, 250, 0.1)'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        y: { 
                            grid: { color: '#334155' },
                            ticks: { color: '#94a3b8' }
                        },
                        x: { 
                            grid: { display: false },
                            ticks: { color: '#94a3b8' }
                        }
                    }
                }
            });
        }

        init();
    </script>
</body>
</html>
