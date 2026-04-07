<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Monitoreo - Prensa</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-50 p-4 md:p-8">
    <div id="app" class="max-w-6xl mx-auto">
        <header class="mb-8">
            <h1 class="text-3xl font-bold text-slate-800">📊 Monitoreo de Medios</h1>
            <p class="text-slate-500">Datos en tiempo real desde Google Sheets</p>
        </header>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                <p class="text-sm font-medium text-slate-500">Menciones Mes Actual</p>
                <div class="flex items-baseline gap-4">
                    <h2 class="text-4xl font-bold text-slate-900">{{ stats.currentMonth }}</h2>
                    <span :class="stats.diff >= 0 ? 'text-green-600' : 'text-red-600'" class="text-sm font-semibold">
                        {{ stats.diff >= 0 ? '↑' : '↓' }} {{ Math.abs(stats.percentage).toFixed(1) }}%
                    </span>
                </div>
                <p class="text-xs text-slate-400 mt-2">vs. mes anterior ({{ stats.lastMonth }} menciones)</p>
            </div>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
            <h3 class="text-lg font-semibold mb-4 text-slate-700">Línea de Tiempo (Menciones Diarias)</h3>
            <canvas id="timeChart" height="100"></canvas>
        </div>
    </div>

    <script>
        const CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vT2khLH7C3lCFCIV3ss1U5AV4SOL922Sld2rnPhSayp39VqgtlBV8ey63dxgCbur1nHiK-6CfNKB2B-/pub?gid=817460174&single=true&output=csv';

        const { createApp } = Vue;

        createApp({
            data() {
                return {
                    stats: { currentMonth: 0, lastMonth: 0, diff: 0, percentage: 0 }
                }
            },
            mounted() {
                this.fetchData();
            },
            methods: {
                fetchData() {
                    Papa.parse(CSV_URL, {
                        download: true,
                        header: true,
                        complete: (results) => {
                            this.processData(results.data);
                        }
                    });
                },
                processData(data) {
                    const now = new Date();
                    const currentMonth = now.getMonth();
                    const currentYear = now.getFullYear();
                    
                    const dailyCounts = {};
                    let countThisMonth = 0;
                    let countLastMonth = 0;

                    data.forEach(row => {
                        // Ajustar según el nombre de tu columna de fecha
                        const dateStr = row['FECHA DE PUBLICACIÓN'];
                        if (!dateStr) return;

                        // Formato esperado: DD/MM/YYYY
                        const parts = dateStr.split('/');
                        const date = new Date(parts[2], parts[1] - 1, parts[0]);
                        
                        if (isNaN(date)) return;

                        // Agrupar para el gráfico
                        const dayKey = date.toISOString().split('T')[0];
                        dailyCounts[dayKey] = (dailyCounts[dayKey] || 0) + 1;

                        // Lógica de comparación mensual
                        if (date.getFullYear() === currentYear && date.getMonth() === currentMonth) {
                            countThisMonth++;
                        } else if (date.getFullYear() === currentYear && date.getMonth() === currentMonth - 1) {
                            countLastMonth++;
                        }
                    });

                    // Actualizar KPIs
                    this.stats.currentMonth = countThisMonth;
                    this.stats.lastMonth = countLastMonth;
                    this.stats.diff = countThisMonth - countLastMonth;
                    this.stats.percentage = countLastMonth ? (this.stats.diff / countLastMonth) * 100 : 0;

                    this.renderChart(dailyCounts);
                },
                renderChart(dailyData) {
                    const sortedLabels = Object.keys(dailyData).sort();
                    const values = sortedLabels.map(label => dailyData[label]);

                    new Chart(document.getElementById('timeChart'), {
                        type: 'line',
                        data: {
                            labels: sortedLabels,
                            datasets: [{
                                label: 'Menciones',
                                data: values,
                                borderColor: '#3b82f6',
                                backgroundColor: 'rgba(59, 130, 246, 0.1)',
                                fill: true,
                                tension: 0.3
                            }]
                        },
                        options: {
                            responsive: true,
                            plugins: { legend: { display: false } },
                            scales: { y: { beginAtZero: true } }
                        }
                    });
                }
            }
        }).mount('#app');
    </script>
</body>
</html>
