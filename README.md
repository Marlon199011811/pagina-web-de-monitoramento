<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#0a0e27">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Monitor Profissional - Caixa D'água</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        :root {
            --bg-dark: #0a0e27;
            --bg-card: #1a1f3a;
            --bg-secondary: #141832;
            --accent-blue: #00d4ff;
            --accent-cyan: #00fff7;
            --text-primary: #ffffff;
            --text-secondary: #8892b0;
            --warning: #ffaa00;
            --danger: #ff4444;
            --success: #00ff88;
            --purple: #a78bfa;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: 'Poppins', sans-serif;
            background: var(--bg-dark);
            color: var(--text-primary);
            min-height: 100vh;
            overflow-x: hidden;
            background-image: 
                radial-gradient(circle at 20% 20%, rgba(102, 126, 234, 0.15) 0%, transparent 40%),
                radial-gradient(circle at 80% 80%, rgba(0, 212, 255, 0.1) 0%, transparent 40%);
        }
        
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            padding-bottom: 40px;
        }
        
        /* Header */
        .header {
            background: linear-gradient(135deg, #1a1f3a 0%, #0a0e27 100%);
            padding: 25px 20px 35px;
            text-align: center;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, var(--accent-cyan), var(--accent-blue), var(--accent-cyan));
            animation: shimmer 3s ease-in-out infinite;
        }
        
        @keyframes shimmer {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }
        
        .header h1 {
            font-size: clamp(1.5rem, 4vw, 2rem);
            font-weight: 800;
            margin-bottom: 8px;
            background: linear-gradient(135deg, var(--accent-cyan) 0%, var(--accent-blue) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .header p {
            color: var(--text-secondary);
            font-size: clamp(0.85rem, 2vw, 1rem);
            font-weight: 400;
        }
        
        /* Alert Section */
        .alert-section {
            padding: 15px 20px 0;
        }
        
        .alert {
            padding: 15px 18px;
            border-radius: 15px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 12px;
            font-weight: 500;
            font-size: 0.9rem;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .alert-warning {
            background: linear-gradient(135deg, rgba(255, 170, 0, 0.15), rgba(255, 170, 0, 0.05));
            border: 2px solid var(--warning);
            color: var(--warning);
            animation: pulse 2s ease-in-out infinite;
        }
        
        .alert-danger {
            background: linear-gradient(135deg, rgba(255, 68, 68, 0.15), rgba(255, 68, 68, 0.05));
            border: 2px solid var(--danger);
            color: var(--danger);
            animation: pulse 2s ease-in-out infinite;
        }
        
        .alert-success {
            background: linear-gradient(135deg, rgba(0, 255, 136, 0.15), rgba(0, 255, 136, 0.05));
            border: 2px solid var(--success);
            color: var(--success);
        }
        
        .alert-info {
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.15), rgba(0, 212, 255, 0.05));
            border: 2px solid var(--accent-blue);
            color: var(--accent-cyan);
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.9; transform: scale(0.99); }
        }
        
        .alert-icon {
            font-size: 1.5rem;
            flex-shrink: 0;
        }
        
        /* Main Content Grid */
        .content-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            padding: 20px;
        }
        
        /* Card Base */
        .card {
            background: var(--bg-card);
            border-radius: 25px;
            padding: 25px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .card-title {
            font-size: 1.1rem;
            font-weight: 700;
            color: var(--accent-cyan);
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 1px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .card-title-icon {
            font-size: 1.4rem;
        }
        
        /* Water Tank Card */
        .tank-card {
            text-align: center;
        }
        
        .water-tank-container {
            position: relative;
            width: 100%;
            max-width: 280px;
            height: 320px;
            margin: 0 auto 25px;
        }
        
        .water-tank {
            width: 180px;
            height: 280px;
            border: 4px solid var(--accent-blue);
            border-radius: 0 0 20px 20px;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            overflow: hidden;
            background: rgba(0, 0, 0, 0.5);
            box-shadow: 
                inset 0 0 35px rgba(0, 212, 255, 0.3),
                0 0 35px rgba(0, 212, 255, 0.2);
        }
        
        .tank-top {
            position: absolute;
            top: -8px;
            left: 50%;
            transform: translateX(-50%);
            width: 200px;
            height: 12px;
            background: linear-gradient(135deg, var(--accent-blue), var(--accent-cyan));
            border-radius: 6px;
            box-shadow: 0 4px 15px rgba(0, 212, 255, 0.4);
        }
        
        .water-level {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(180deg, 
                rgba(0, 255, 247, 0.9) 0%, 
                rgba(0, 212, 255, 0.95) 30%,
                rgba(0, 153, 255, 1) 60%,
                rgba(0, 102, 204, 1) 100%
            );
            transition: height 1.5s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 -10px 30px rgba(0, 212, 255, 0.6);
        }
        
        .water-surface {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 40px;
            overflow: hidden;
        }
        
        .water-wave {
            position: absolute;
            top: -20px;
            left: -100%;
            width: 400%;
            height: 40px;
            background: 
                radial-gradient(ellipse at center, rgba(255,255,255,0.4) 0%, transparent 50%);
            animation: wave 4s ease-in-out infinite;
        }
        
        .water-wave:nth-child(2) {
            animation: wave 5s ease-in-out infinite reverse;
            opacity: 0.6;
        }
        
        @keyframes wave {
            0%, 100% { transform: translateX(0) translateY(0); }
            25% { transform: translateX(-10%) translateY(-4px); }
            50% { transform: translateX(-20%) translateY(0); }
            75% { transform: translateX(-10%) translateY(4px); }
        }
        
        .bubbles {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 100%;
            overflow: hidden;
        }
        
        .bubble {
            position: absolute;
            bottom: -20px;
            width: 10px;
            height: 10px;
            background: radial-gradient(circle, rgba(255,255,255,0.6), rgba(255,255,255,0.1));
            border-radius: 50%;
            animation: rise 4s ease-in infinite;
            opacity: 0;
        }
        
        .bubble:nth-child(1) { left: 20%; animation-delay: 0s; width: 8px; height: 8px; }
        .bubble:nth-child(2) { left: 40%; animation-delay: 1.5s; width: 12px; height: 12px; }
        .bubble:nth-child(3) { left: 60%; animation-delay: 0.8s; width: 6px; height: 6px; }
        .bubble:nth-child(4) { left: 80%; animation-delay: 2.2s; width: 10px; height: 10px; }
        .bubble:nth-child(5) { left: 50%; animation-delay: 3s; width: 7px; height: 7px; }
        
        @keyframes rise {
            0% { bottom: -20px; opacity: 0; }
            10% { opacity: 0.8; }
            90% { opacity: 0.3; }
            100% { bottom: 100%; opacity: 0; transform: translateX(20px); }
        }
        
        .level-display {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
            text-align: center;
            background: rgba(10, 14, 39, 0.85);
            padding: 15px 25px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            border: 2px solid rgba(0, 212, 255, 0.3);
        }
        
        .level-text {
            font-size: 3rem;
            font-weight: 800;
            background: linear-gradient(135deg, var(--accent-cyan), var(--accent-blue));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            line-height: 1;
        }
        
        .level-label {
            font-size: 0.75rem;
            color: var(--text-secondary);
            margin-top: 5px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        /* Statistics Grid */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .stat-card {
            background: var(--bg-secondary);
            padding: 18px;
            border-radius: 15px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 212, 255, 0.15);
        }
        
        .stat-icon {
            font-size: 2rem;
            margin-bottom: 8px;
        }
        
        .stat-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--accent-cyan);
            margin-bottom: 5px;
        }
        
        .stat-label {
            font-size: 0.85rem;
            color: var(--text-secondary);
            font-weight: 500;
        }
        
        /* Time Statistics Cards */
        .time-stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 15px;
        }
        
        .time-stat-card {
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(167, 139, 250, 0.1));
            padding: 20px;
            border-radius: 18px;
            border: 2px solid rgba(0, 212, 255, 0.2);
            transition: all 0.3s ease;
        }
        
        .time-stat-card:hover {
            border-color: var(--accent-cyan);
            box-shadow: 0 10px 30px rgba(0, 212, 255, 0.2);
            transform: translateY(-2px);
        }
        
        .time-stat-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 15px;
        }
        
        .time-stat-icon {
            font-size: 2.2rem;
        }
        
        .time-stat-title {
            font-size: 0.9rem;
            font-weight: 600;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        
        .time-stat-value {
            font-size: 2.2rem;
            font-weight: 800;
            background: linear-gradient(135deg, var(--accent-cyan), var(--purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 5px;
        }
        
        .time-stat-description {
            font-size: 0.8rem;
            color: var(--text-secondary);
            line-height: 1.4;
        }
        
        /* Chart Card */
        .chart-card {
            min-height: 400px;
        }
        
        .chart-container {
            position: relative;
            height: 350px;
            margin-top: 20px;
        }
        
        /* Refresh Button */
        .refresh-section {
            padding: 0 20px 20px;
        }
        
        .refresh-btn {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, var(--accent-blue), var(--accent-cyan));
            border: none;
            border-radius: 15px;
            color: var(--bg-dark);
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(0, 212, 255, 0.3);
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .refresh-btn:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(0, 212, 255, 0.4);
        }
        
        .refresh-btn:active:not(:disabled) {
            transform: translateY(0);
        }
        
        .refresh-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }
        
        .spinner {
            width: 18px;
            height: 18px;
            border: 3px solid rgba(10, 14, 39, 0.3);
            border-top-color: var(--bg-dark);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* Loading Screen */
        .loading-screen {
            position: fixed;
            inset: 0;
            background: var(--bg-dark);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            transition: opacity 0.5s ease, visibility 0.5s ease;
        }
        
        .loading-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }
        
        .loading-spinner {
            width: 60px;
            height: 60px;
            border: 4px solid rgba(0, 212, 255, 0.2);
            border-top-color: var(--accent-cyan);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }
        
        .loading-text {
            color: var(--text-secondary);
            font-size: 1rem;
        }
        
        /* Last Update */
        .last-update {
            text-align: center;
            padding: 15px 20px;
            color: var(--text-secondary);
            font-size: 0.85rem;
        }
        
        .last-update strong {
            color: var(--accent-cyan);
        }
        
        /* Desktop Layout */
        @media (min-width: 768px) {
            .content-grid {
                grid-template-columns: 1fr 1fr;
            }
            
            .tank-card {
                grid-row: 1 / 3;
            }
            
            .chart-card {
                grid-column: 1 / -1;
            }
            
            .time-stats-grid {
                grid-column: 1 / -1;
            }
            
            .chart-container {
                height: 400px;
            }
        }
        
        @media (min-width: 1200px) {
            .content-grid {
                grid-template-columns: 1fr 2fr;
            }
            
            .stats-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <!-- Loading Screen -->
    <div class="loading-screen" id="loadingScreen">
        <div class="loading-spinner"></div>
        <div class="loading-text">Carregando dados...</div>
    </div>

    <div class="app-container">
        <!-- Header -->
        <header class="header">
            <h1>💧 Monitor Caixa D'água</h1>
            <p>Sistema Profissional de Monitoramento</p>
        </header>

        <!-- Alert Section -->
        <section class="alert-section">
            <div id="alertContainer"></div>
        </section>

        <!-- Main Content -->
        <main class="content-grid">
            <!-- Water Tank Card -->
            <div class="card tank-card">
                <div class="card-title">
                    <span class="card-title-icon">💧</span>
                    Nível Atual
                </div>
                
                <div class="water-tank-container">
                    <div class="tank-top"></div>
                    <div class="water-tank">
                        <div class="water-level" id="waterLevel">
                            <div class="water-surface">
                                <div class="water-wave"></div>
                                <div class="water-wave"></div>
                            </div>
                            <div class="bubbles">
                                <div class="bubble"></div>
                                <div class="bubble"></div>
                                <div class="bubble"></div>
                                <div class="bubble"></div>
                                <div class="bubble"></div>
                            </div>
                        </div>
                        <div class="level-display">
                            <div class="level-text" id="levelText">--</div>
                            <div class="level-label">Porcentagem</div>
                        </div>
                    </div>
                </div>

                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-icon">📊</div>
                        <div class="stat-value" id="avgLevel">--</div>
                        <div class="stat-label">Média</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-icon">⬆️</div>
                        <div class="stat-value" id="maxLevel">--</div>
                        <div class="stat-label">Máximo</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-icon">⬇️</div>
                        <div class="stat-value" id="minLevel">--</div>
                        <div class="stat-label">Mínimo</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-icon">📝</div>
                        <div class="stat-value" id="totalReadings">--</div>
                        <div class="stat-label">Leituras</div>
                    </div>
                </div>
            </div>

            <!-- Time Statistics -->
            <div class="card">
                <div class="card-title">
                    <span class="card-title-icon">⏱️</span>
                    Estatísticas de Tempo
                </div>
                
                <div class="time-stats-grid">
                    <div class="time-stat-card">
                        <div class="time-stat-header">
                            <span class="time-stat-icon">⬆️</span>
                            <div class="time-stat-title">Último Enchimento</div>
                        </div>
                        <div class="time-stat-value" id="fillTime">--</div>
                        <div class="time-stat-description">Tempo que levou para encher do último nível mínimo ao máximo</div>
                    </div>
                    
                    <div class="time-stat-card">
                        <div class="time-stat-header">
                            <span class="time-stat-icon">⬇️</span>
                            <div class="time-stat-title">Último Esvaziamento</div>
                        </div>
                        <div class="time-stat-value" id="drainTime">--</div>
                        <div class="time-stat-description">Tempo que levou para esvaziar do último nível máximo ao mínimo</div>
                    </div>
                </div>
            </div>

            <!-- Chart Card -->
            <div class="card chart-card">
                <div class="card-title">
                    <span class="card-title-icon">📈</span>
                    Histórico de Níveis
                </div>
                <div class="chart-container">
                    <canvas id="levelChart"></canvas>
                </div>
            </div>
        </main>

        <!-- Refresh Button -->
        <section class="refresh-section">
            <button class="refresh-btn" id="refreshBtn" onclick="loadData()">
                <span id="btnText">Atualizar Dados</span>
                <div class="spinner" id="spinner" style="display: none;"></div>
            </button>
        </section>

        <!-- Last Update -->
        <div class="last-update">
            Última atualização: <strong id="lastUpdate">--:--</strong>
        </div>
    </div>

    <script>
        const API_URL = 'https://script.google.com/macros/s/AKfycbx7kaNvASekDf5duusiRwjq163R9KDs6NEuTqlieUW3f_7hPJo3MhWIS8NURT_tA04/exec';
        
        let chart = null;
        let isLoading = false;
        
        // Carregar dados na inicialização
        window.addEventListener('load', () => {
            loadData();
        });
        
        async function loadData() {
            if (isLoading) return;
            
            isLoading = true;
            const refreshBtn = document.getElementById('refreshBtn');
            const spinner = document.getElementById('spinner');
            const btnText = document.getElementById('btnText');
            const alertContainer = document.getElementById('alertContainer');
            const loadingScreen = document.getElementById('loadingScreen');
            
            refreshBtn.disabled = true;
            spinner.style.display = 'block';
            btnText.textContent = 'Carregando...';
            
            alertContainer.innerHTML = `
                <div class="alert alert-info">
                    <span class="alert-icon">🔄</span>
                    <div class="alert-content">
                        <strong>Sincronizando...</strong> Buscando dados atualizados.
                    </div>
                </div>
            `;
            
            try {
                console.log('Fazendo requisição para:', API_URL);
                
                const response = await fetch(API_URL, {
                    method: 'GET',
                    redirect: 'follow'
                });
                
                console.log('Resposta recebida, status:', response.status);
                
                if (!response.ok) {
                    throw new Error(`Erro HTTP: ${response.status}`);
                }
                
                const text = await response.text();
                console.log('Texto da resposta:', text.substring(0, 200));
                
                let result;
                try {
                    result = JSON.parse(text);
                } catch (e) {
                    console.error('Erro ao parsear JSON:', e);
                    throw new Error('Resposta inválida do servidor');
                }
                
                if (result.error) {
                    throw new Error(result.error);
                }
                
                if (!result.success || !result.data || result.data.length === 0) {
                    throw new Error('Nenhum dado encontrado na resposta');
                }
                
                console.log(`${result.data.length} registros carregados com sucesso`);
                
                const data = result.data;
                updateDashboard(data);
                updateChart(data);
                calculateTimingStats(data);
                
                const currentLevel = data[data.length - 1].level;
                updateAlerts(currentLevel);
                
                setTimeout(() => {
                    const alertSuccess = alertContainer.querySelector('.alert-success');
                    if (!alertSuccess) {
                        alertContainer.innerHTML = `
                            <div class="alert alert-success">
                                <span class="alert-icon">✅</span>
                                <div class="alert-content">
                                    <strong>Conectado!</strong> ${data.length} leituras carregadas com sucesso.
                                </div>
                            </div>
                        `;
                        
                        setTimeout(() => {
                            const currentAlert = alertContainer.querySelector('.alert-success');
                            if (currentAlert) {
                                currentAlert.style.transition = 'opacity 0.5s ease';
                                currentAlert.style.opacity = '0';
                                setTimeout(() => {
                                    if (currentAlert.parentNode) {
                                        currentAlert.remove();
                                        updateAlerts(currentLevel);
                                    }
                                }, 500);
                            }
                        }, 3000);
                    }
                }, 500);
                
                document.getElementById('lastUpdate').textContent = new Date().toLocaleTimeString('pt-BR', {
                    hour: '2-digit',
                    minute: '2-digit'
                });
                
                loadingScreen.classList.add('hidden');
                
            } catch (error) {
                console.error('Erro completo:', error);
                
                let errorMsg = error.message;
                let errorDetails = '';
                
                if (error.name === 'AbortError') {
                    errorMsg = 'Tempo de conexão esgotado';
                    errorDetails = 'A requisição demorou muito. Verifique sua internet.';
                } else if (error.message.includes('Failed to fetch')) {
                    errorMsg = 'Falha na conexão';
                    errorDetails = 'Verifique se está conectado à internet ou se o Apps Script está ativo.';
                } else if (error.message.includes('HTTP')) {
                    errorDetails = 'Problema no servidor do Google Apps Script.';
                }
                
                alertContainer.innerHTML = `
                    <div class="alert alert-danger">
                        <span class="alert-icon">❌</span>
                        <div class="alert-content">
                            <strong>Erro!</strong> ${errorMsg}
                            ${errorDetails ? `<small>${errorDetails}</small>` : ''}
                        </div>
                    </div>
                `;
                
                loadingScreen.classList.add('hidden');
            } finally {
                spinner.style.display = 'none';
                btnText.textContent = 'Atualizar Dados';
                refreshBtn.disabled = false;
                isLoading = false;
            }
        }
        
        
        function updateDashboard(data) {
            const levels = data.map(d => d.level);
            const currentLevel = levels[levels.length - 1];
            const avgLevel = Math.round(levels.reduce((a, b) => a + b, 0) / levels.length);
            const maxLevel = Math.max(...levels);
            const minLevel = Math.min(...levels);
            
            document.getElementById('levelText').textContent = currentLevel + '%';
            document.getElementById('waterLevel').style.height = currentLevel + '%';
            
            document.getElementById('avgLevel').textContent = avgLevel + '%';
            document.getElementById('maxLevel').textContent = maxLevel + '%';
            document.getElementById('minLevel').textContent = minLevel + '%';
            document.getElementById('totalReadings').textContent = data.length;
        }
        
        function calculateTimingStats(data) {
            if (data.length < 2) {
                document.getElementById('fillTime').textContent = 'N/A';
                document.getElementById('drainTime').textContent = 'N/A';
                return;
            }
            
            // Encontrar último registro de nível mínimo e máximo
            let lastMinIndex = -1;
            let lastMaxIndex = -1;
            let minLevel = 100;
            let maxLevel = 0;
            
            // Primeiro, encontrar os valores mínimo e máximo reais
            for (let i = 0; i < data.length; i++) {
                if (data[i].level < minLevel) minLevel = data[i].level;
                if (data[i].level > maxLevel) maxLevel = data[i].level;
            }
            
            // Agora encontrar os últimos índices de min e max (com margem de ±5%)
            for (let i = data.length - 1; i >= 0; i--) {
                if (lastMinIndex === -1 && data[i].level <= minLevel + 5) {
                    lastMinIndex = i;
                }
                if (lastMaxIndex === -1 && data[i].level >= maxLevel - 5) {
                    lastMaxIndex = i;
                }
                if (lastMinIndex !== -1 && lastMaxIndex !== -1) break;
            }
            
            // Calcular tempo de enchimento (do último mínimo ao último máximo)
            let fillTime = 'N/A';
            if (lastMinIndex !== -1 && lastMaxIndex !== -1 && lastMaxIndex > lastMinIndex) {
                const minTime = new Date(data[lastMinIndex].dateTime);
                const maxTime = new Date(data[lastMaxIndex].dateTime);
                const timeDiff = (maxTime - minTime) / (1000 * 60); // em minutos
                fillTime = formatTime(timeDiff);
            }
            
            // Calcular tempo de esvaziamento (do último máximo ao último mínimo)
            let drainTime = 'N/A';
            if (lastMinIndex !== -1 && lastMaxIndex !== -1 && lastMinIndex > lastMaxIndex) {
                const maxTime = new Date(data[lastMaxIndex].dateTime);
                const minTime = new Date(data[lastMinIndex].dateTime);
                const timeDiff = (minTime - maxTime) / (1000 * 60); // em minutos
                drainTime = formatTime(timeDiff);
            }
            
            document.getElementById('fillTime').textContent = fillTime;
            document.getElementById('drainTime').textContent = drainTime;
        }
        
        function formatTime(minutes) {
            if (minutes < 60) {
                return Math.round(minutes) + ' min';
            } else if (minutes < 1440) {
                const hours = Math.floor(minutes / 60);
                const mins = Math.round(minutes % 60);
                return hours + 'h ' + (mins > 0 ? mins + 'min' : '');
            } else {
                const days = Math.floor(minutes / 1440);
                const hours = Math.floor((minutes % 1440) / 60);
                return days + 'd ' + (hours > 0 ? hours + 'h' : '');
            }
        }
        
        function updateChart(data) {
            const ctx = document.getElementById('levelChart').getContext('2d');
            
            // Mostrar apenas os últimos 4 registros
            const recentData = data.slice(-4);
            
            const labels = recentData.map(d => {
                const date = new Date(d.dateTime);
                const day = date.getDate().toString().padStart(2, '0');
                const month = (date.getMonth() + 1).toString().padStart(2, '0');
                const hours = date.getHours().toString().padStart(2, '0');
                const minutes = date.getMinutes().toString().padStart(2, '0');
                return `${day}/${month} ${hours}:${minutes}`;
            });
            
            const levels = recentData.map(d => d.level);
            
            if (chart) {
                chart.destroy();
            }
            
            chart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Nível da Caixa (%)',
                        data: levels,
                        borderColor: '#00d4ff',
                        backgroundColor: 'rgba(0, 212, 255, 0.1)',
                        borderWidth: 3,
                        fill: true,
                        tension: 0.4,
                        pointBackgroundColor: '#00d4ff',
                        pointBorderColor: '#fff',
                        pointBorderWidth: 2,
                        pointRadius: 4,
                        pointHoverRadius: 6,
                        pointHoverBackgroundColor: '#00fff7',
                        pointHoverBorderWidth: 3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: true,
                            labels: {
                                color: '#8892b0',
                                font: {
                                    size: 13,
                                    weight: '600'
                                },
                                padding: 15
                            }
                        },
                        tooltip: {
                            backgroundColor: 'rgba(26, 31, 58, 0.95)',
                            titleColor: '#00fff7',
                            bodyColor: '#fff',
                            borderColor: '#00d4ff',
                            borderWidth: 2,
                            padding: 12,
                            displayColors: false,
                            titleFont: {
                                size: 13,
                                weight: '700'
                            },
                            bodyFont: {
                                size: 14,
                                weight: '600'
                            },
                            callbacks: {
                                label: function(context) {
                                    return 'Nível: ' + context.parsed.y + '%';
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            grid: {
                                color: 'rgba(255, 255, 255, 0.08)',
                                drawBorder: false
                            },
                            ticks: {
                                color: '#8892b0',
                                font: {
                                    size: 12,
                                    weight: '600'
                                },
                                callback: function(value) {
                                    return value + '%';
                                },
                                padding: 10
                            },
                            title: {
                                display: true,
                                text: 'Nível (%)',
                                color: '#00d4ff',
                                font: {
                                    size: 13,
                                    weight: '700'
                                },
                                padding: 10
                            }
                        },
                        x: {
                            grid: {
                                color: 'rgba(255, 255, 255, 0.05)',
                                drawBorder: false
                            },
                            ticks: {
                                color: '#8892b0',
                                font: {
                                    size: 11,
                                    weight: '500'
                                },
                                maxRotation: 45,
                                minRotation: 45,
                                autoSkip: false,
                                maxTicksLimit: 4
                            },
                            title: {
                                display: true,
                                text: 'Data e Hora',
                                color: '#00d4ff',
                                font: {
                                    size: 13,
                                    weight: '700'
                                },
                                padding: 10
                            }
                        }
                    },
                    interaction: {
                        intersect: false,
                        mode: 'index'
                    }
                }
            });
        }
        
        function updateAlerts(currentLevel) {
            const alertContainer = document.getElementById('alertContainer');
            
            if (alertContainer.querySelector('.alert-success')) {
                return;
            }
            
            let alert = '';
            
            if (currentLevel <= 25) {
                alert = `
                    <div class="alert alert-danger">
                        <span class="alert-icon">⚠️</span>
                        <div class="alert-content">
                            <strong>Nível Crítico!</strong> Apenas ${currentLevel}% de água restante.
                        </div>
                    </div>
                `;
            } else if (currentLevel <= 50) {
                alert = `
                    <div class="alert alert-warning">
                        <span class="alert-icon">⚡</span>
                        <div class="alert-content">
                            <strong>Atenção!</strong> Nível baixo detectado (${currentLevel}%).
                        </div>
                    </div>
                `;
            } else if (currentLevel >= 75) {
                alert = `
                    <div class="alert alert-info">
                        <span class="alert-icon">💧</span>
                        <div class="alert-content">
                            <strong>Ótimo!</strong> Nível adequado de água (${currentLevel}%).
                        </div>
                    </div>
                `;
            }
            
            alertContainer.innerHTML = alert;
        }
        
        // Atualizar automaticamente a cada 300 segundos
        setInterval(() => {
            if (!isLoading) {
                loadData();
            }
        }, 300000); // 300000ms = 300 segundos
        
        // Pull to refresh
        let touchStartY = 0;
        document.addEventListener('touchstart', (e) => {
            touchStartY = e.touches[0].clientY;
        });
        
        document.addEventListener('touchmove', (e) => {
            const touchY = e.touches[0].clientY;
            const touchDiff = touchY - touchStartY;
            
            if (touchDiff > 100 && window.scrollY === 0 && !isLoading) {
                loadData();
            }
        });
    </script>
</body>
</html>
