<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>M12 模擬試題 App</title>
    <!-- PWA 支援與 iPhone 畫面優化 -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="M12 刷題">
    
    <style>
        :root {
            --bg-color: #1a1c23;
            --card-bg: #252836;
            --primary: #4e73df;
            --success: #1cc88a;
            --danger: #e74a3b;
            --text-main: #ffffff;
            --text-muted: #b7b9cc;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-user-select: none;
            user-select: none;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        header {
            background-color: var(--card-bg);
            padding: 15px;
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            border-bottom: 1px solid #34384c;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .container {
            flex: 1;
            padding: 20px;
            max-width: 600px;
            margin: 0 auto;
            width: 100%;
        }

        .card {
            background-color: var(--card-bg);
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.15);
        }

        h2 {
            font-size: 18px;
            margin-bottom: 15px;
            color: var(--text-main);
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
            color: var(--text-muted);
        }

        select, button {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid #34384c;
            background-color: #1a1c23;
            color: var(--text-main);
            font-size: 16px;
            margin-bottom: 15px;
            outline: none;
            -webkit-appearance: none;
        }

        button {
            background-color: var(--primary);
            font-weight: bold;
            cursor: pointer;
            border: none;
            transition: background 0.2s;
            margin-top: 10px;
        }

        button:active {
            opacity: 0.8;
        }

        .hidden {
            display: none !important;
        }

        /* 題目樣式 */
        .q-text {
            font-size: 16px;
            line-height: 1.5;
            margin-bottom: 15px;
            word-break: break-word;
            -webkit-user-select: text;
            user-select: text;
        }

        .option-btn {
            background-color: #2d303e;
            text-align: left;
            padding: 15px;
            margin-bottom: 10px;
            border: 1px solid #34384c;
            font-weight: normal;
        }

        .option-btn.correct {
            background-color: rgba(28, 200, 138, 0.2) !important;
            border-color: var(--success) !important;
        }

        .option-btn.wrong {
            background-color: rgba(231, 74, 59, 0.2) !important;
            border-color: var(--danger) !important;
        }

        .explain-box {
            background-color: #1a1c23;
            border-left: 4px solid var(--primary);
            padding: 12px;
            border-radius: 4px;
            margin-top: 10px;
            font-size: 14px;
            color: var(--text-muted);
        }

        .nav-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        .score-banner {
            font-size: 32px;
            font-weight: bold;
            text-align: center;
            margin: 20px 0;
        }

        .pass { color: var(--success); }
        .fail { color: var(--danger); }

        .ios-tip {
            background-color: #2d303e;
            border: 1px dashed var(--primary);
            padding: 12px;
            border-radius: 8px;
            font-size: 13px;
            color: var(--text-muted);
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>

    <header id="app-title">M12- 航空維修試題</header>

    <div class="container">
        <!-- 1. 設定畫面 -->
        <div id="setup-view" class="card">
            <h2>⚙️ 測驗設定與選單</h2>
            
            <label>選擇節次章節</label>
            <select id="chapter-select">
                <option value="all">全部章節隨機混出</option>
                <option value="12.01">12.01 飛行原理-旋翼機空氣動力學</option>
                <option value="12.02">12.02 飛行控制系統</option>
                <option value="12.03">12.03 葉片軌跡及振動分析</option>
                <option value="12.04">12.04 傳動系</option>
                <option value="12.05">12.05 機身結構</option>
                <option value="12.06">12.06 空調系(ATA 21)</option>
                <option value="12.07">12.07 儀表/航電系統</option>
                <option value="12.08">12.08 電力系統(ATA 24)</option>
                <option value="12.09">12.09 裝備與內裝(ATA 25)</option>
                <option value="12.10">12.10 火災防護(ATA 26)</option>
                <option value="12.11">12.11 燃油系統(ATA 28)</option>
                <option value="12.12">12.12 液壓動力(ATA 29)</option>
                <option value="12.13">12.13 冰雪雨防護(ATA 30)</option>
                <option value="12.14">12.14 起落架(ATA 32)</option>
                <option value="12.15">12.15 燈光系統(ATA 33)</option>
                <option value="12.16">12.16 氣動/真空(ATA 36)</option>
                <option value="12.17">12.17 綜合航電與補充題</option>
            </select>

            <label>測驗模式</label>
            <select id="mode-select">
                <option value="study">💡 學習模式 (即時對答案看解析)</option>
                <option value="exam">⏱️ 模擬考模式 (交卷才計分)</option>
            </select>

            <label>題目數量</label>
            <select id="amount-select">
                <option value="10">10 題</option>
                <option value="20">20 題</option>
                <option value="30">30 題</option>
                <option value="50">50 題</option>
                <option value="100">100 題</option>
            </select>

            <label>題目順序</label>
            <select id="order-select">
                <option value="random">🎲 隨機亂序出題</option>
                <option value="seq">🔢 依原始題號順序</option>
            </select>

            <button id="start-btn">🚀 開始刷題</button>

            <div class="ios-tip">
                💡 iPhone 離線密技：點擊 Safari 下方 📤 按鈕，選擇<b>「加入主畫面」</b>，即可隨時在無網路環境下開啟練習！
            </div>
        </div>

        <!-- 2. 作答畫面 -->
        <div id="quiz-view" class="card hidden">
            <div style="display: flex; justify-content: space-between; margin-bottom: 15px; font-size: 14px; color: var(--text-muted);">
                <span id="progress-text">第 1 / 10 題</span>
                <span id="chapter-tag">12.01</span>
            </div>
            
            <div id="question-container" class="q-text">載入中...</div>
            <div id="options-container"></div>
            <div id="explain-container" class="explain-box hidden"></div>

            <div class="nav-actions">
                <button id="exit-btn" style="background-color: #4a4e69; flex: 1;">離開</button>
                <button id="next-btn" style="flex: 2;" class="hidden">下一題 ➡️</button>
            </div>
        </div>

        <!-- 3. 結算畫面 -->
        <div id="result-view" class="card hidden">
            <h2>📊 測驗結果</h2>
            <div id="score-text" class="score-banner">0 分</div>
            <p id="result-comment" style="text-align: center; margin-bottom: 20px; color: var(--text-muted);"></p>
            
            <button id="retry-btn">🔄 再考一次</button>
            <button id="back-home-btn" style="background-color: #4a4e69;">返回首頁</button>
        </div>
    </div>

    <!-- 原始完整安全資料庫庫 -->
    <script>
        var ALL_QUESTIONS = [
            // 12.01
            { id: 1, ch: "12.01", q: "A helicopter is hovering and the pilot applies right pedal. Assuming the main rotor rotates anti clockwise viewed from above, the helicopter will. (直升機滯空且右舵向前時，假設從上方看主旋翼為反時針旋轉，則直升機將會如何？)", a: "descend, unless the pilot applies more collective pitch.", b: "descend, unless the pilot inches the throttle open.", c: "ascend, unless the pilot decreases rotor RPM. (上升，除非駕駛降低旋翼轉速)", ans: "c", exp: "主旋翼反時針旋轉時，右舵會改變尾槳反扭力配平，導致直升機產生上升趨勢，除非降低旋翼轉速或調低集體變距。" },
            { id: 2, ch: "12.01", q: "Forward velocity causes the advancing blade to. (向前速率導致前進旋翼葉片會如何？)", a: "flap down to increase lift.", b: "give increased lift due to blade flapping.", c: "flap up to reduce lift. (向上撲動而減少升力)", ans: "c", exp: "相對前進氣流增加使前進片升力增大，進而向上撲動（Flap up）來減小攻角，自動平衡左右兩側升力。" },
            { id: 3, ch: "12.01", q: "When the rotor blade increases its angle of attack, the centre of pressure. (當旋翼片增加攻角時，其壓力中心如何移動？)", a: "moves forward.", b: "moves rearwards.", c: "does not move. (不會移動)", ans: "c", exp: "對對對稱型航空翼型而言，攻角在正常範圍內改變時，其壓力中心（Centre of Pressure）基本保持不變。" },
