<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🦙 可愛水豚君天氣預報</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* CSS 樣式開始 */
        :root {
            /* 水豚君主題色 */
            --capybara-brown: #a0522d; /* 棕色 */
            --capybara-light: #f5deb3; /* 淺米色/卡其色 */
            --sky-blue-light: #87ceeb; /* 天空藍 */
            --sunny-yellow: #ffd700; /* 太陽黃 */
            --rainy-gray: #b0c4de; /* 雨天藍灰 */
            --current-bg: #87ceeb; /* 當前時段背景 */
            --text-color: #4b4b4b; /* 深灰文字 */
            --card-shadow: 0 6px 15px rgba(160, 82, 45, 0.2);
            --border-radius-large: 25px;
            --border-radius-small: 15px;
        }

        /* 全局設置與可愛字體 */
        body {
            font-family: 'Fredoka', sans-serif; /* 使用可愛圓潤的字體 */
            background-color: var(--capybara-light);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            line-height: 1.6;
        }

        .container {
            width: 100%;
            max-width: 900px;
            padding: 20px;
            background: white;
            border-radius: var(--border-radius-large);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        h1 {
            text-align: center;
            color: var(--capybara-brown);
            margin-bottom: 30px;
            font-size: 2.2em;
            text-shadow: 2px 2px 0 var(--sunny-yellow);
        }

        /* 主時段 (當前/最近) 區塊 */
        #current-forecast {
            background-color: var(--current-bg);
            padding: 30px;
            margin-bottom: 20px;
            border-radius: var(--border-radius-large);
            box-shadow: var(--card-shadow);
            color: white; /* 主要文字顏色改為白色，更醒目 */
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            position: relative;
            overflow: hidden;
            border: 4px solid var(--capybara-brown);
        }

        #current-forecast:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(160, 82, 45, 0.4);
        }

        .current-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .current-time-period {
            font-size: 1.8em;
            font-weight: 700;
        }

        .current-city {
            font-size: 1.1em;
            font-weight: 600;
        }

        .current-main {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
        }

        .weather-icon-lg {
            font-size: 6em; /* 超大 Icon */
            margin-bottom: 10px;
            animation: pulse 2s infinite alternate; /* 脈衝動畫 */
        }

        .current-temp {
            font-size: 4em;
            font-weight: 700;
            line-height: 1;
        }

        .current-weather-text {
            font-size: 1.5em;
            font-weight: 600;
            margin-top: -10px;
            margin-bottom: 15px;
        }

        .current-details {
            display: flex;
            justify-content: space-around;
            width: 100%;
            max-width: 400px;
            margin-top: 15px;
            padding: 10px 0;
            background-color: rgba(255, 255, 255, 0.2); /* 輕微透明背景 */
            border-radius: var(--border-radius-small);
        }

        .detail-item {
            text-align: center;
            font-size: 1em;
            font-weight: 600;
        }
        
        .detail-item span {
            display: block;
            font-size: 1.5em;
            margin-bottom: 5px;
        }


        /* 未來時段區塊 */
        .future-forecasts {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        .forecast-card {
            background-color: var(--capybara-light);
            padding: 20px;
            border-radius: var(--border-radius-small);
            box-shadow: var(--card-shadow);
            text-align: center;
            transition: transform 0.3s ease, background-color 0.3s ease;
            border: 2px solid var(--capybara-brown);
        }

        .forecast-card:hover {
            transform: scale(1.05);
            background-color: var(--sunny-yellow);
        }

        .forecast-card h3 {
            color: var(--capybara-brown);
            font-size: 1.3em;
            margin-top: 0;
            margin-bottom: 10px;
            border-bottom: 2px dashed var(--capybara-brown);
            padding-bottom: 5px;
        }

        .weather-icon-md {
            font-size: 3em;
            margin: 5px 0 10px 0;
            display: inline-block;
            animation: bounce 1s ease-in-out infinite alternate; /* 彈跳動畫 */
        }

        .forecast-card p {
            margin: 5px 0;
            font-size: 0.95em;
        }

        .temp-range {
            font-size: 1.2em;
            font-weight: 700;
            color: var(--capybara-brown);
        }

        .rain-chance {
            color: #1e90ff; /* 藍色降雨機率 */
            font-weight: 600;
        }

        /* 載入中/錯誤訊息 */
        #loading, #error {
            text-align: center;
            font-size: 1.5em;
            color: var(--capybara-brown);
            padding: 50px;
        }

        /* 響應式設計 (RWD) */
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            h1 {
                font-size: 1.8em;
            }
            #current-forecast {
                padding: 20px;
            }
            .weather-icon-lg {
                font-size: 5em;
            }
            .current-temp {
                font-size: 3em;
            }
            .current-weather-text {
                font-size: 1.3em;
            }
            .current-details {
                flex-direction: column;
                align-items: center;
            }
            .detail-item {
                margin: 5px 0;
            }
            .future-forecasts {
                grid-template-columns: 1fr; /* 單列顯示 */
            }
        }

        /* Icon 動畫 */
        @keyframes pulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.05); }
        }

        @keyframes bounce {
            0% { transform: translateY(0); }
            100% { transform: translateY(-5px); }
        }

        /* 滾動動畫 */
        @keyframes flow {
            from {
                background-position: 0 0;
            }
            to {
                background-position: 200% 0;
            }
        }

        /* JavaScript 根據天氣自動變換背景顏色 */
        .weather-sunny {
            background-color: var(--sunny-yellow);
        }
        .weather-cloudy {
            background-color: var(--rainy-gray);
        }
        .weather-rainy {
            background-color: #6a5acd; /* 較深的藍紫色 */
        }
        .weather-thunder {
            background-color: #708090; /* 較深的灰 */
        }
        /* CSS 樣式結束 */
    </style>
</head>
<body>

    <div class="container">
        <h1>🦙 臺中市水豚君天氣預報 ☀️</h1>
        
        <div id="loading">... 正在呼叫可愛的水豚君氣象站中 ...</div>
        <div id="error" style="display: none;"></div>

        <div id="current-forecast" style="display: none;">
            <div class="current-header">
                <div class="current-time-period" id="current-time-period"></div>
                <div class="current-city" id="current-city"></div>
            </div>
            <div class="current-main">
                <div class="weather-icon-lg" id="current-icon"></div>
                <div class="current-temp-range">
                    <span class="current-temp" id="current-max-temp"></span> / <span id="current-min-temp"></span>
                </div>
                <div class="current-weather-text" id="current-weather"></div>
            </div>
            <div class="current-details">
                <div class="detail-item">
                    <span id="current-rain"></span>
                    降雨機率
                </div>
                <div class="detail-item">
                    <span id="current-comfort"></span>
                    舒適度
                </div>
            </div>
        </div>

        <div class="future-forecasts" id="future-forecasts">
            </div>

    </div>

    <script>
        // JavaScript 開始
        const API_URL = 'https://nutc-web-vic-peng.zeabur.app/api/weather/kaohsiung';

        const loadingElement = document.getElementById('loading');
        const errorElement = document.getElementById('error');
        const currentForecastElement = document.getElementById('current-forecast');
        const futureForecastsElement = document.getElementById('future-forecasts');

        /**
         * 根據天氣狀況文字，返回對應的可愛 Emoji Icon 和 CSS Class
         * @param {string} weatherText - 天氣狀況文字 (如: 晴時多雲, 陰天, 陣雨)
         * @returns {{icon: string, className: string}}
         */
        function getWeatherIconAndClass(weatherText) {
            let icon = '❓'; // 預設
            let className = 'weather-default';
            
            if (weatherText.includes('晴') || weatherText.includes('多雲')) {
                icon = '☀️';
                className = 'weather-sunny';
            } else if (weatherText.includes('陰') || weatherText.includes('多雲')) {
                icon = '☁️';
                className = 'weather-cloudy';
            } else if (weatherText.includes('雨') || weatherText.includes('陣雨')) {
                icon = '🌧️';
                className = 'weather-rainy';
            } else if (weatherText.includes('雷')) {
                icon = '⛈️';
                className = 'weather-thunder';
            } else if (weatherText.includes('雪')) {
                icon = '🌨️';
                className = 'weather-snowy';
            }
            return { icon, className };
        }

        /**
         * 根據時間戳 (ISO 格式) 判斷時段描述 (早晨, 上午, 下午, 晚上)
         * @param {string} startTime - ISO 8601 格式的時間字串
         * @returns {string} 時段描述
         */
        function getTimePeriod(startTime) {
            const date = new Date(startTime);
            const hour = date.getHours();

            if (hour >= 5 && hour < 9) {
                return '🌄 早晨';
            } else if (hour >= 9 && hour < 12) {
                return '🌤️ 上午';
            } else if (hour >= 12 && hour < 18) {
                return '☀️ 下午';
            } else if (hour >= 18 || hour < 5) {
                return '🌃 晚上';
            }
            return '時段預報';
        }

        /**
         * 創建未來時段的天氣預報卡片
         * @param {Object} forecast - 單一時段的預報資料
         * @returns {string} HTML 字串
         */
        function createForecastCard(forecast) {
            const { icon, className } = getWeatherIconAndClass(forecast.weather);
            const timePeriod = getTimePeriod(forecast.startTime);

            return `
                <div class="forecast-card ${className}">
                    <h3>${timePeriod}</h3>
                    <div class="weather-icon-md">${icon}</div>
                    <p>${forecast.weather}</p>
                    <p class="temp-range">${forecast.minTemp} ~ ${forecast.maxTemp}</p>
                    <p class="rain-chance">💧 降雨機率: ${forecast.rain}</p>
                    <p>😊 ${forecast.comfort}</p>
                </div>
            `;
        }


        /**
         * 載入並顯示天氣資料
         */
        async function loadWeatherData() {
            try {
                // 1. 抓取資料
                const response = await fetch(API_URL);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const json = await response.json();

                if (!json.success || !json.data || !json.data.forecasts || json.data.forecasts.length === 0) {
                    throw new Error('API 回傳資料結構錯誤或無資料');
                }

                const forecasts = json.data.forecasts;
                const city = json.data.city;

                // 2. 處理資料
                const current = forecasts[0];
                const future = forecasts.slice(1);

                // 3. 顯示主時段/當前時段 (第一個預報)
                const { icon, className } = getWeatherIconAndClass(current.weather);
                const timePeriod = getTimePeriod(current.startTime);

                document.getElementById('current-city').textContent = city;
                document.getElementById('current-time-period').textContent = timePeriod;
                document.getElementById('current-icon').textContent = icon;
                document.getElementById('current-max-temp').textContent = current.maxTemp;
                document.getElementById('current-min-temp').textContent = current.minTemp;
                document.getElementById('current-weather').textContent = current.weather;
                
                // 這裡的舒適度描述文字較長，拆成兩行顯示，第一行只顯示 icon + 簡潔標題
                document.getElementById('current-comfort').innerHTML = `🥰 <br> ${current.comfort}`; 
                document.getElementById('current-rain').innerHTML = `💦 <br> ${current.rain}`;

                // 根據天氣類別變更背景顏色
                currentForecastElement.className = `weather-card ${className}`;
                
                // 4. 顯示未來時段
                futureForecastsElement.innerHTML = future.map(createForecastCard).join('');

                // 5. 隱藏載入中，顯示內容
                loadingElement.style.display = 'none';
                currentForecastElement.style.display = 'block';
                
            } catch (error) {
                console.error('天氣資料載入失敗:', error);
                loadingElement.style.display = 'none';
                errorElement.textContent = `🚨 資料載入失敗: ${error.message} (請檢查 API 網址是否正確)`;
                errorElement.style.display = 'block';
            }
        }

        // 頁面載入完成後執行
        document.addEventListener('DOMContentLoaded', loadWeatherData);
        // JavaScript 結束
    </script>

</body>
</html>
