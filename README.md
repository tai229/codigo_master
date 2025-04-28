codigo do conversor de moeda
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Conversor de Moedas Pro</title>
  <!-- Adicionando Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --primary: #00ffcc;
      --primary-dark: #00d6ad;
      --background: #121212;
      --card-bg: #1f1f1f;
      --card-hover: #2a2a2a;
      --text: #ffffff;
      --text-secondary: #bbbbbb;
      --shadow: 0 0 15px rgba(0, 255, 204, 0.3);
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      transition: all 0.3s ease;
    }
    
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background-color: var(--background);
      color: var(--text);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }
    
    .container {
      background-color: var(--card-bg);
      padding: 30px;
      border-radius: 16px;
      box-shadow: var(--shadow);
      width: 90%;
      max-width: 480px;
      text-align: center;
    }
    
    h1 {
      margin-bottom: 20px;
      color: var(--primary);
      font-size: 2rem;
    }
    
    .converter {
      display: flex;
      flex-direction: column;
      gap: 15px;
    }
    
    .input-group {
      display: flex;
      align-items: center;
      background-color: #2a2a2a;
      border-radius: 12px;
      overflow: hidden;
    }
    
    .input-group input {
      flex: 1;
      border: none;
      padding: 15px;
      background-color: transparent;
      color: var(--text);
      font-size: 1.1em;
      outline: none;
    }
    
    .input-group select {
      border: none;
      padding: 15px;
      background-color: #333;
      color: var(--text);
      font-size: 1.1em;
      cursor: pointer;
      outline: none;
      min-width: 120px;
    }
    
    .swap-btn {
      background-color: #333;
      color: var(--primary);
      border: none;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      font-size: 1.2em;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      align-self: center;
      margin: 5px 0;
    }
    
    .swap-btn:hover {
      background-color: #444;
    }
    
    button {
      padding: 15px;
      border: none;
      border-radius: 12px;
      font-size: 1.1em;
      font-weight: 600;
      cursor: pointer;
    }
    
    .convert-btn {
      background-color: var(--primary);
      color: #000;
      margin-top: 10px;
    }
    
    .convert-btn:hover {
      background-color: var(--primary-dark);
    }
    
    .result {
      margin-top: 25px;
      padding: 20px;
      background-color: #2a2a2a;
      border-radius: 12px;
      font-size: 1.2em;
      word-break: break-word;
    }
    
    .result-amount {
      font-size: 1.8em;
      font-weight: 600;
      color: var(--primary);
      margin: 10px 0;
    }
    
    .copy-btn {
      background-color: #333;
      color: var(--text);
      padding: 8px 15px;
      font-size: 0.9em;
      margin-top: 15px;
    }
    
    .copy-btn:hover {
      background-color: #444;
    }
    
    .history {
      margin-top: 30px;
      text-align: left;
    }
    
    .history h3 {
      color: var(--primary);
      margin-bottom: 15px;
      font-size: 1.2em;
    }
    
    .history-list {
      max-height: 150px;
      overflow-y: auto;
      background-color: #2a2a2a;
      border-radius: 12px;
      padding: 10px;
    }
    
    .history-item {
      padding: 8px;
      border-bottom: 1px solid #333;
    }
    
    .history-item:last-child {
      border-bottom: none;
    }
    
    .info {
      margin-top: 20px;
      color: var(--text-secondary);
      font-size: 0.9em;
    }
    
    .info a {
      color: var(--primary);
      text-decoration: none;
    }
    
    .info a:hover {
      text-decoration: underline;
    }
    
    .tabs {
      display: flex;
      margin-bottom: 20px;
      background-color: #2a2a2a;
      border-radius: 12px;
      overflow: hidden;
    }
    
    .tab {
      flex: 1;
      padding: 12px;
      background: none;
      border: none;
      color: var(--text);
      cursor: pointer;
    }
    
    .tab.active {
      background-color: var(--primary);
      color: #000;
    }
    
    .tab-content {
      display: none;
    }
    
    .tab-content.active {
      display: block;
    }
    
    @media (max-width: 480px) {
      .container {
        padding: 20px;
      }
      
      h1 {
        font-size: 1.7rem;
      }
      
      .input-group select {
        min-width: 90px;
        font-size: 0.9em;
        padding: 15px 10px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Conversor de Moedas Pro</h1>
    
    <div class="tabs">
      <button class="tab active" data-tab="converter">Conversor</button>
      <button class="tab" data-tab="charts">Gráficos</button>
      <button class="tab" data-tab="settings">Configurações</button>
    </div>
    
    <div id="converter" class="tab-content active">
      <div class="converter">
        <div class="input-group">
          <input type="number" id="amount" placeholder="Digite um valor" />
          <select id="fromCurrency"></select>
        </div>
        
        <button class="swap-btn" id="swapBtn">⇅</button>
        
        <div class="input-group">
          <input type="number" id="resultAmount" placeholder="Resultado" readonly />
          <select id="toCurrency"></select>
        </div>
        
        <button class="convert-btn" id="convertBtn">Converter</button>
      </div>
      
      <div class="result" id="result">
        <div>Digite um valor para converter</div>
      </div>
      
      <div class="history">
        <h3>Histórico de conversões</h3>
        <div class="history-list" id="historyList"></div>
      </div>
    </div>
    
    <div id="charts" class="tab-content">
      <div style="padding: 20px; background: #2a2a2a; border-radius: 12px; margin-bottom: 20px;">
        <p>Gráfico de variação das últimas 7 dias</p>
        <div id="chartContainer" style="height: 200px; margin-top: 20px;">
          <canvas id="currencyChart"></canvas>
        </div>
      </div>
      
      <div class="input-group" style="margin-bottom: 15px;">
        <select id="chartCurrency1">
          <!-- Preenchido via JavaScript -->
        </select>
        <select id="chartCurrency2">
          <!-- Preenchido via JavaScript -->
        </select>
      </div>
      
      <button class="convert-btn" id="updateChartBtn">Atualizar Gráfico</button>
    </div>
    
    <div id="settings" class="tab-content">
      <div style="padding: 20px; background: #2a2a2a; border-radius: 12px; margin-bottom: 20px;">
        <h3 style="margin-bottom: 15px; color: var(--primary);">Moedas favoritas</h3>
        <div id="favoriteCurrencies" style="display: flex; flex-wrap: wrap; gap: 10px;"></div>
      </div>
      
      <div style="padding: 20px; background: #2a2a2a; border-radius: 12px; margin-bottom: 20px;">
        <h3 style="margin-bottom: 15px; color: var(--primary);">Tema</h3>
        <button class="tab" style="width: 48%; margin-right: 2%;" id="lightTheme">Claro</button>
        <button class="tab active" style="width: 48%;" id="darkTheme">Escuro</button>
      </div>
      
      <div style="padding: 20px; background: #2a2a2a; border-radius: 12px;">
        <h3 style="margin-bottom: 15px; color: var(--primary);">Formato de exibição</h3>
        <div style="text-align: left; display: flex; flex-direction: column; gap: 10px;">
          <label>
            <input type="checkbox" id="showDecimals" checked> Mostrar 2 casas decimais
          </label>
          <label>
            <input type="checkbox" id="showSymbol" checked> Mostrar símbolo da moeda
          </label>
        </div>
      </div>
    </div>
    
    <div class="info">
      Dados atualizados em <span id="lastUpdate">carregando...</span>
      <br>
      Taxas fornecidas por <a href="https://www.exchangerate-api.com" target="_blank">ExchangeRate-API</a>
    </div>
  </div>

  <script>
    // Lista expandida de moedas
    const currencies = {
      USD: { name: "Dólar Americano", symbol: "$" },
      EUR: { name: "Euro", symbol: "€" },
      BRL: { name: "Real Brasileiro", symbol: "R$" },
      GBP: { name: "Libra Esterlina", symbol: "£" },
      JPY: { name: "Iene Japonês", symbol: "¥" },
      CNY: { name: "Yuan Chinês", symbol: "¥" },
      AUD: { name: "Dólar Australiano", symbol: "A$" },
      CAD: { name: "Dólar Canadense", symbol: "C$" },
      CHF: { name: "Franco Suíço", symbol: "Fr" },
      AOA: { name: "Kwanza Angolano", symbol: "Kz" },
      MXN: { name: "Peso Mexicano", symbol: "$" },
      ARS: { name: "Peso Argentino", symbol: "$" },
      CLP: { name: "Peso Chileno", symbol: "$" },
      COP: { name: "Peso Colombiano", symbol: "$" },
      PEN: { name: "Sol Peruano", symbol: "S/" },
      UYU: { name: "Peso Uruguaio", symbol: "$" },
      ZAR: { name: "Rand Sul-Africano", symbol: "R" },
      TRY: { name: "Lira Turca", symbol: "₺" },
      RUB: { name: "Rublo Russo", symbol: "₽" },
      INR: { name: "Rupia Indiana", symbol: "₹" },
      KRW: { name: "Won Sul-Coreano", symbol: "₩" },
      SGD: { name: "Dólar de Singapura", symbol: "S$" },
      HKD: { name: "Dólar de Hong Kong", symbol: "HK$" },
      BTC: { name: "Bitcoin", symbol: "₿" },
      ETH: { name: "Ethereum", symbol: "Ξ" },
      LTC: { name: "Litecoin", symbol: "Ł" },
      XRP: { name: "Ripple", symbol: "XRP" },
      DOGE: { name: "Dogecoin", symbol: "Ð" },
      ADA: { name: "Cardano", symbol: "₳" }
    };

    // Estado da aplicação
    let exchangeRates = {};
    let favorites = JSON.parse(localStorage.getItem('favoriteCurrencies')) || ["USD", "EUR", "BRL", "GBP", "BTC"];
    let conversionHistory = JSON.parse(localStorage.getItem('conversionHistory')) || [];
    let darkMode = localStorage.getItem('darkMode') !== 'false';
    let currencyChart = null;

    // Elementos DOM
    const amountInput = document.getElementById('amount');
    const resultAmountInput = document.getElementById('resultAmount');
    const fromCurrencySelect = document.getElementById('fromCurrency');
    const toCurrencySelect = document.getElementById('toCurrency');
    const resultDiv = document.getElementById('result');
    const historyList = document.getElementById('historyList');
    const lastUpdateSpan = document.getElementById('lastUpdate');
    const swapBtn = document.getElementById('swapBtn');
    const convertBtn = document.getElementById('convertBtn');
    const tabs = document.querySelectorAll('.tab');
    const tabContents = document.querySelectorAll('.tab-content');
    const chartCurrency1 = document.getElementById('chartCurrency1');
    const chartCurrency2 = document.getElementById('chartCurrency2');
    const favoriteCurrenciesDiv = document.getElementById('favoriteCurrencies');
    
    // Inicialização
    window.onload = function() {
      populateCurrencySelects();
      fetchExchangeRates();
      renderConversionHistory();
      renderFavoriteCurrencies();
      initTabsSystem();
      
      // Configurando eventos
      swapBtn.addEventListener('click', swapCurrencies);
      convertBtn.addEventListener('click', convert);
      amountInput.addEventListener('input', autoConvert);
      fromCurrencySelect.addEventListener('change', autoConvert);
      toCurrencySelect.addEventListener('change', autoConvert);
      document.getElementById('updateChartBtn').addEventListener('click', updateChart);
      document.getElementById('lightTheme').addEventListener('click', () => setTheme(false));
      document.getElementById('darkTheme').addEventListener('click', () => setTheme(true));
      
      // Aplicar tema salvo
      setTheme(darkMode);
    };
    
    // Funções principais
    function populateCurrencySelects() {
      // Ordena as moedas por nome
      const sortedCurrencies = Object.entries(currencies).sort((a, b) => {
        // Colocar BRL no topo para usuários brasileiros
        if (a[0] === 'BRL') return -1;
        if (b[0] === 'BRL') return 1;
        // Depois moedas mais populares
        if (favorites.includes(a[0]) && !favorites.includes(b[0])) return -1;
        if (!favorites.includes(a[0]) && favorites.includes(b[0])) return 1;
        // Depois ordem alfabética por nome
        return a[1].name.localeCompare(b[1].name);
      });
      
      // Limpar selects
      [fromCurrencySelect, toCurrencySelect, chartCurrency1, chartCurrency2].forEach(select => {
        if (select) select.innerHTML = '';
      });
      
      // Preencher selects
      sortedCurrencies.forEach(([code, info]) => {
        const option = `<option value="${code}">${info.name} (${code})</option>`;
        
        if (fromCurrencySelect) fromCurrencySelect.innerHTML += option;
        if (toCurrencySelect) toCurrencySelect.innerHTML += option;
        if (chartCurrency1) chartCurrency1.innerHTML += option;
        if (chartCurrency2) chartCurrency2.innerHTML += option;
      });
      
      // Definir valores padrão
      if (fromCurrencySelect) fromCurrencySelect.value = 'BRL';
      if (toCurrencySelect) toCurrencySelect.value = 'USD';
      if (chartCurrency1) chartCurrency1.value = 'BRL';
      if (chartCurrency2) chartCurrency2.value = 'USD';
    }
    
    async function fetchExchangeRates() {
      try {
        // Primeiro tentamos obter as taxas de câmbio tradicionais
        const response = await fetch('https://api.exchangerate-api.com/v4/latest/USD');
        const data = await response.json();
        
        if (data && data.rates) {
          exchangeRates = data.rates;
          exchangeRates.USD = 1; // Garantir que USD sempre seja 1
          
          // Agora buscamos as cotações das criptomoedas
          await fetchCryptoRates();
          
          lastUpdateSpan.textContent = new Date().toLocaleString();
          autoConvert();
          
          // Inicializar o gráfico
          updateChart();
        }
      } catch (error) {
        console.error('Erro ao buscar taxas de câmbio:', error);
        resultDiv.innerHTML = `<div>Erro ao buscar taxas de câmbio. Tente novamente.</div>`;
        
        // Usar taxas de fallback se a API falhar
        setFallbackRates();
      }
    }
    
    async function fetchCryptoRates() {
      try {
        // API para criptomoedas (simplificada)
        const cryptoResponse = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,litecoin,ripple,dogecoin,cardano&vs_currencies=usd');
        const cryptoData = await cryptoResponse.json();
        
        if (cryptoData) {
          // Atualizar taxas de criptomoedas
          if (cryptoData.bitcoin) exchangeRates.BTC = 1 / cryptoData.bitcoin.usd;
          if (cryptoData.ethereum) exchangeRates.ETH = 1 / cryptoData.ethereum.usd;
          if (cryptoData.litecoin) exchangeRates.LTC = 1 / cryptoData.litecoin.usd;
          if (cryptoData.ripple) exchangeRates.XRP = 1 / cryptoData.ripple.usd;
          if (cryptoData.dogecoin) exchangeRates.DOGE = 1 / cryptoData.dogecoin.usd;
          if (cryptoData.cardano) exchangeRates.ADA = 1 / cryptoData.cardano.usd;
        }
      } catch (error) {
        console.error('Erro ao buscar cotações de criptomoedas:', error);
        // Usar valores padrão para criptomoedas se a API falhar
        exchangeRates.BTC = 0.000037;
        exchangeRates.ETH = 0.00052;
        exchangeRates.LTC = 0.008;
        exchangeRates.XRP = 1.42;
        exchangeRates.DOGE = 12.45;
        exchangeRates.ADA = 1.82;
      }
    }
    
    function setFallbackRates() {
      // Taxas de fallback (atualizadas manualmente)
      exchangeRates = {
        USD: 1,
        EUR: 0.93,
        BRL: 5.45,
        GBP: 0.79,
        JPY: 151.50,
        CNY: 7.24,
        AUD: 1.52,
        CAD: 1.36,
        CHF: 0.91,
        AOA: 832.50,
        MXN: 16.70,
        ARS: 864.50,
        CLP: 960,
        COP: 3900,
        PEN: 3.72,
        UYU: 39.20,
        ZAR: 18.70,
        TRY: 32.30,
        RUB: 92.50,
        INR: 83.30,
        KRW: 1345,
        SGD: 1.35,
        HKD: 7.83,
        BTC: 0.000037,
        ETH: 0.00052,
        LTC: 0.008,
        XRP: 1.42,
        DOGE: 12.45,
        ADA: 1.82
      };
      
      lastUpdateSpan.textContent = new Date().toLocaleString() + " (offline)";
    }
    
    function convertAmount(amount, fromCurrency, toCurrency) {
      if (!exchangeRates[fromCurrency] || !exchangeRates[toCurrency]) {
        return null;
      }
      
      // Converter para USD primeiro (base comum)
      const amountInUSD = amount / exchangeRates[fromCurrency];
      // Converter de USD para moeda de destino
      return amountInUSD * exchangeRates[toCurrency];
    }
    
    function autoConvert() {
      if (amountInput.value) {
        convert();
      }
    }
    
    async function convert() {
      const amount = parseFloat(amountInput.value);
      const from = fromCurrencySelect.value;
      const to = toCurrencySelect.value;
      
      if (isNaN(amount) || amount <= 0) {
        resultDiv.innerHTML = `<div>Por favor, insira um valor válido.</div>`;
        return;
      }
      
      try {
        const result = convertAmount(amount, from, to);
        
        if (result !== null) {
          const showSymbol = document.getElementById('showSymbol').checked;
          const showDecimals = document.getElementById('showDecimals').checked;
          
          // Formatar o resultado
          const formattedResult = new Intl.NumberFormat('pt-BR', {
            minimumFractionDigits: showDecimals ? 2 : 0,
            maximumFractionDigits: showDecimals ? 2 : 0
          }).format(result);
          
          // Mostrar o resultado
          resultDiv.innerHTML = `
            <div>
              <div>${amount} ${from} =</div>
              <div class="result-amount">${showSymbol ? currencies[to].symbol : ''} ${formattedResult} ${to}</div>
              <div>1 ${from} = ${showSymbol ? currencies[to].symbol : ''} ${(result / amount).toFixed(6)} ${to}</div>
              <button class="copy-btn" onclick="copyToClipboard('${formattedResult}')">Copiar valor</button>
            </div>
          `;
          
          // Atualizar o campo de resultado
          resultAmountInput.value = showDecimals ? result.toFixed(2) : result.toFixed(0);
          
          // Adicionar ao histórico
          addToHistory(amount, from, result, to);
        } else {
          throw new Error('Moeda não encontrada');
        }
      } catch (error) {
        console.error('Erro na conversão:', error);
        resultDiv.innerHTML = `<div>Erro na conversão. Tente novamente.</div>`;
      }
    }
    
    function swapCurrencies() {
      const fromValue = fromCurrencySelect.value;
      const toValue = toCurrencySelect.value;
      
      fromCurrencySelect.value = toValue;
      toCurrencySelect.value = fromValue;
      
      // Trocar também os valores, se houver
      if (amountInput.value && resultAmountInput.value) {
        const tempValue = amountInput.value;
        amountInput.value = resultAmountInput.value;
        resultAmountInput.value = tempValue;
      }
      
      autoConvert();
    }
    
    function addToHistory(amount, fromCurrency, result, toCurrency) {
      const timestamp = new Date();
      const historyItem = {
        amount,
        fromCurrency,
        result,
        toCurrency,
        timestamp: timestamp.toISOString()
      };
      
      // Adicionar ao início para os itens mais recentes aparecerem primeiro
      conversionHistory.unshift(historyItem);
      
      // Limitar a 10 itens
      if (conversionHistory.length > 10) {
        conversionHistory.pop();
      }
      
      // Salvar no localStorage
      localStorage.setItem('conversionHistory', JSON.stringify(conversionHistory));
      
      // Atualizar a exibição
      renderConversionHistory();
    }
    
    function renderConversionHistory() {
      if (!historyList) return;
      
      historyList.innerHTML = '';
      
      if (conversionHistory.length === 0) {
        historyList.innerHTML = '<div class="history-item">Nenhuma conversão realizada</div>';
        return;
      }
      
      conversionHistory.forEach(item => {
        const date = new Date(item.timestamp);
        const formattedDate = date.toLocaleString('pt-BR', { 
          day: '2-digit', 
          month: '2-digit',
          hour: '2-digit', 
          minute: '2-digit'
        });
        
        const historyItem = document.createElement('div');
        historyItem.className = 'history-item';
        historyItem.innerHTML = `
          <div>${item.amount} ${item.fromCurrency} → ${item.result.toFixed(2)} ${item.toCurrency}</div>
          <div style="font-size: 0.8em; color: var(--text-secondary);">${formattedDate}</div>
        `;
        
        // Adicionar evento de clique para repetir a conversão
        historyItem.addEventListener('click', () => {
          amountInput.value = item.amount;
          fromCurrencySelect.value = item.fromCurrency;
          toCurrencySelect.value = item.toCurrency;
          convert();
        });
        
        historyList.appendChild(historyItem);
      });
    }
    
    function renderFavoriteCurrencies() {
      if (!favoriteCurrenciesDiv) return;
      
      favoriteCurrenciesDiv.innerHTML = '';
      
      Object.entries(currencies).forEach(([code, info]) => {
        const isFavorite = favorites.includes(code);
        
        const currencyBtn = document.createElement('button');
        currencyBtn.className = `tab ${isFavorite ? 'active' : ''}`;
        currencyBtn.textContent = code;
        currencyBtn.style.margin = '5px';
        currencyBtn.style.width = 'calc(25% - 10px)';
        
        currencyBtn.addEventListener('click', () => {
          if (isFavorite) {
            // Remover dos favoritos se já estiver lá
            favorites = favorites.filter(c => c !== code);
            currencyBtn.classList.remove('active');
          } else {
            // Adicionar aos favoritos se não estiver
            favorites.push(code);
            currencyBtn.classList.add('active');
          }
          
          // Salvar no localStorage
          localStorage.setItem('favoriteCurrencies', JSON.stringify(favorites));
          
          // Reordenar e repopular os selects
          populateCurrencySelects();
        });
        
        favoriteCurrenciesDiv.appendChild(currencyBtn);
      });
    }
    
    async function updateChart() {
      const currency1 = chartCurrency1.value;
      const currency2 = chartCurrency2.value;
      
      try {
        // Obter dados históricos (últimos 7 dias)
        const endDate = new Date();
        const startDate = new Date();
        startDate.setDate(endDate.getDate() - 7);
        
        const formatDate = (date) => date.toISOString().split('T')[0];
        
        // Para criptomoedas, usamos a API CoinGecko
        if (['BTC', 'ETH', 'LTC', 'XRP', 'DOGE', 'ADA'].includes(currency1) || 
            ['BTC', 'ETH', 'LTC', 'XRP', 'DOGE', 'ADA'].includes(currency2)) {
          
          // Simplificação: vamos mostrar apenas a variação de uma criptomoeda em USD
          if (currency2 !== 'USD' && currency1 !== 'USD') {
            alert('Para criptomoedas, mostramos apenas a variação em relação ao USD');
            return;
          }
          
          const cryptoId = {
            BTC: 'bitcoin',
            ETH: 'ethereum',
            LTC: 'litecoin',
            XRP: 'ripple',
            DOGE: 'dogecoin',
            ADA: 'cardano'
          }[currency1 === 'USD' ? currency2 : currency1];
          
          const response = await fetch(`https://api.coingecko.com/api/v3/coins/${cryptoId}/market_chart?vs_currency=usd&days=7`);
          const data = await response.json();
          
          const labels = [];
          const values = [];
          
          // Processar dados para obter valores diários
          const dailyData = {};
          data.prices.forEach(([timestamp, price]) => {
            const date = new Date(timestamp).toISOString().split('T')[0];
            if (!dailyData[date]) {
              dailyData[date] = price;
            }
          });
          
          // Criar arrays ordenados
          Object.keys(dailyData).sort().forEach(date => {
            labels.push(new Date(date).toLocaleDateString('pt-BR', {day: 'numeric', month: 'short'}));
            values.push(dailyData[date]);
          });
          
          renderChart(labels, values, `${currency1 === 'USD' ? currency2 : currency1}/USD`);
          
        } else {
          // Para moedas tradicionais, usamos a API ExchangeRate-API
          const response = await fetch(`https://api.exchangerate-api.com/v4/historical/USD?start_date=${formatDate(startDate)}&end_date=${formatDate(endDate)}`);
          const data = await response.json();
          
          const labels = [];
          const values = [];
          
          // Processar dados
          Object.keys(data.rates).sort().forEach(date => {
            const rateData = data.rates[date];
            if (rateData[currency1] && rateData[currency2]) {
              labels.push(new Date(date).toLocaleDateString('pt-BR', {day: 'numeric', month: 'short'}));
              values.push(rateData[currency2] / rateData[currency1]);
            }
          });
          
          renderChart(labels, values, `${currency1}/${currency2}`);
        }
      } catch (error) {
        console.error('Erro ao atualizar gráfico:', error);
        alert('Erro ao carregar dados do gráfico. Tente novamente mais tarde.');
        
        // Renderizar gráfico de exemplo em caso de erro
        const labels = ['Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb', 'Dom'];
        const values = [1, 1.02, 1.05, 1.03, 1.07, 1.06, 1.04];
        renderChart(labels, values, `${currency1}/${currency2}`);
      }
    }
    
    function renderChart(labels, values, pair) {
      const ctx = document.getElementById('currencyChart').getContext('2d');
      
      // Destruir gráfico anterior se existir
      if (currencyChart) {
        currencyChart.destroy();
      }
      
      currencyChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [{
            label: `Variação ${pair}`,
            data: values,
            borderColor: '#00ffcc',
            backgroundColor: 'rgba(0, 255, 204, 0.1)',
            borderWidth: 2,
            fill: true,
            tension: 0.4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              display: false
            }
          },
          scales: {
            y: {
              beginAtZero: false,
              grid: {
                color: 'rgba(255, 255, 255, 0.1)'
              },
              ticks: {
                color: '#ffffff'
              }
            },
            x: {
              grid: {
                color: 'rgba(255, 255, 255, 0.1)'
              },
              ticks: {
                color: '#ffffff'
              }
            }
          }
        }
      });
    }
    
    function initTabsSystem() {
      tabs.forEach(tab => {
        tab.addEventListener('click', () => {
          // Remover classe ativa de todas as abas
          tabs.forEach(t => t.classList.remove('active'));
          tabContents.forEach(c => c.classList.remove('active'));
          
          // Adicionar classe ativa à aba clicada
          tab.classList.add('active');
          
          // Mostrar o conteúdo correspondente
          const tabId = tab.getAttribute('data-tab');
          document.getElementById(tabId).classList.add('active');
          
          // Se for a aba de gráficos, atualizar o gráfico
          if (tabId === 'charts') {
            updateChart();
          }
        });
      });
    }
    
    function setTheme(isDark) {
      const root = document.documentElement;
      
      if (isDark) {
        root.style.setProperty('--background', '#121212');
        root.style.setProperty('--card-bg', '#1f1f1f');
        root.style.setProperty('--card-hover', '#2a2a2a');
        root.style.setProperty('--text', '#ffffff');
        root.style.setProperty('--text-secondary', '#bbbbbb');
        
        document.getElementById('darkTheme').classList.add('active');
        document.getElementById('lightTheme').classList.remove('active');
      } else {
        root.style.setProperty('--background', '#f5f5f5');
        root.style.setProperty('--card-bg', '#ffffff');
        root.style.setProperty('--card-hover', '#f0f0f0');
        root.style.setProperty('--text', '#333333');
        root.style.setProperty('--text-secondary', '#666666');
        
        document.getElementById('lightTheme').classList.add('active');
        document.getElementById('darkTheme').classList.remove('active');
      }
      
      // Salvar preferência
      localStorage.setItem('darkMode', isDark);
      darkMode = isDark;
      
      // Atualizar o gráfico se existir
      if (currencyChart) {
        currencyChart.update();
      }
    }
    
    function copyToClipboard(text) {
      navigator.clipboard.writeText(text)
        .then(() => {
          const originalText = document.querySelector('.copy-btn').textContent;
          document.querySelector('.copy-btn').textContent = 'Copiado!';
          
          setTimeout(() => {
            document.querySelector('.copy-btn').textContent = originalText;
          }, 2000);
        })
        .catch(err => {
          console.error('Erro ao copiar: ', err);
          alert('Não foi possível copiar o texto');
        });
    }
    
    // Expor funções globalmente
    window.copyToClipboard = copyToClipboard;
  </script>
</body>
</html>
