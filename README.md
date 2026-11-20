<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>C++ 程式設計：重複結構 for 迴圈完全解析</title>
    <!-- 引入 Mermaid.js 用於動態視覺化流程圖 -->
    <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
    <style>
        :root {
            --primary-color: #2980b9;
            --secondary-color: #2c3e50;
            --accent-color: #e67e22;
            --success-color: #27ae60;
            --danger-color: #c0392b;
            --bg-color: #f4f7f6;
            --card-bg: #ffffff;
            --text-color: #333333;
            --code-bg: #1e2227;
        }
        * {
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', 'Microsoft JhengHei', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: var(--text-color);
            background-color: var(--bg-color);
            margin: 0;
            padding: 0;
        }
        header {
            background: linear-gradient(135deg, var(--secondary-color), var(--primary-color));
            color: white;
            padding: 28px 20px;
            text-align: center;
        }
        header h1 {
            margin: 0 0 8px 0;
            font-size: 2.1rem;
        }
        header p {
            margin: 0;
            font-size: 1.05rem;
            opacity: 0.92;
        }
        nav {
            background: var(--secondary-color);
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 2px 6px rgba(0,0,0,0.25);
        }
        nav a {
            color: white;
            padding: 14px 18px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.95rem;
            transition: background 0.25s, color 0.25s;
            cursor: pointer;
            user-select: none;
        }
        nav a:hover, nav a.active {
            background: var(--primary-color);
        }
        /* 寬版容器以支援流程圖與程式碼並排顯示 */
        .container {
            max-width: 1280px;
            margin: 25px auto;
            padding: 0 20px;
        }
        .tab-content {
            display: none;
            background: var(--card-bg);
            padding: 28px;
            border-radius: 12px;
            box-shadow: 0 4px 18px rgba(0,0,0,0.06);
        }
        .tab-content.active {
            display: block;
            animation: fadeIn 0.25s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .cover-box {
            text-align: center;
            padding: 35px 20px;
        }
        .cover-box h2 {
            color: var(--secondary-color);
            font-size: 2rem;
            margin-bottom: 16px;
            border-bottom: none;
        }
        .welcome-box {
            background: #eaf2f8;
            border-left: 6px solid var(--primary-color);
            padding: 22px 26px;
            margin: 22px auto 30px auto;
            text-align: left;
            border-radius: 0 10px 10px 0;
            box-shadow: 0 3px 10px rgba(0,0,0,0.04);
            max-width: 850px;
        }
        .welcome-box h3 {
            margin-top: 0;
            color: var(--secondary-color);
            font-size: 1.25rem;
        }
        .start-btn {
            background: var(--accent-color);
            color: white;
            font-size: 1.15rem;
            padding: 14px 36px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 12px rgba(230, 126, 34, 0.4);
            transition: background 0.25s, transform 0.2s;
        }
        .start-btn:hover {
            background: #d35400;
            transform: translateY(-2px);
        }
        h2 {
            color: var(--secondary-color);
            border-bottom: 3px solid var(--primary-color);
            padding-bottom: 8px;
            margin-top: 0;
        }
        h3 {
            color: var(--accent-color);
            margin-top: 24px;
            margin-bottom: 12px;
        }
        /* 四步驟順序卡片 */
        .steps-banner {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin: 18px 0 24px 0;
        }
        .step-card {
            background: #f8fbfd;
            border: 2px solid #d6eaf8;
            border-radius: 8px;
            padding: 12px 14px;
            text-align: center;
            transition: all 0.2s;
        }
        .step-card.highlight-stage {
            border-color: var(--accent-color);
            background: #fef5e7;
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(230, 126, 34, 0.2);
        }
        .step-badge {
            display: inline-block;
            background: var(--primary-color);
            color: white;
            font-size: 0.8rem;
            font-weight: bold;
            padding: 2px 10px;
            border-radius: 12px;
            margin-bottom: 6px;
        }
        .step-card.highlight-stage .step-badge {
            background: var(--accent-color);
        }
        .step-card strong {
            display: block;
            color: var(--secondary-color);
            font-size: 1rem;
        }
        .step-card span {
            font-size: 0.85rem;
            color: #555;
        }

        /* 關鍵版面配置：流程圖與程式碼並排顯示 (Side-by-Side 2-Column Grid) */
        .side-by-side-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            align-items: stretch;
            margin: 18px 0;
        }
        .panel-card {
            background: #fcfdfe;
            border: 1px solid #dce4ec;
            border-radius: 10px;
            padding: 16px;
            display: flex;
            flex-direction: column;
        }
        .panel-title {
            font-weight: bold;
            color: var(--secondary-color);
            font-size: 1.05rem;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 2px solid #ebf0f5;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .flowchart-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: #ffffff;
            padding: 12px;
            border: 1px dashed #bdc3c7;
            border-radius: 8px;
            overflow-x: auto;
        }

        /* 程式碼與逐步除錯器樣式 */
        pre.static-code {
            background: var(--code-bg);
            color: #abb2bf;
            padding: 16px;
            border-radius: 8px;
            overflow-x: auto;
            font-family: 'Consolas', 'Courier New', monospace;
            font-size: 0.95rem;
            margin: 0;
            flex: 1;
            line-height: 1.55;
        }
        .debug-code-box {
            background: var(--code-bg);
            color: #e5e9f0;
            border-radius: 8px;
            padding: 10px 0;
            font-family: 'Consolas', 'Courier New', monospace;
            font-size: 0.95rem;
            overflow-x: auto;
            margin-bottom: 14px;
        }
        .code-line {
            display: flex;
            padding: 3px 14px;
            border-left: 4px solid transparent;
            transition: background 0.15s, border-color 0.15s;
            white-space: pre;
        }
        .code-line .ln {
            color: #636d83;
            width: 28px;
            user-select: none;
            text-align: right;
            margin-right: 14px;
            flex-shrink: 0;
        }
        .code-line .code-text {
            color: #d8dee9;
        }
        /* 目前正在執行的程式碼行高亮 */
        .code-line.active-line {
            background: rgba(230, 126, 34, 0.28);
            border-left-color: #f39c12;
        }
        .code-line.active-line .ln {
            color: #f39c12;
            font-weight: bold;
        }
        .code-line.active-line .code-text {
            color: #ffffff;
            font-weight: bold;
        }
        .pointer-arrow {
            color: #f39c12;
            margin-right: 6px;
            font-weight: bold;
            visibility: hidden;
        }
        .code-line.active-line .pointer-arrow {
            visibility: visible;
        }

        /* 除錯器控制列與變數監控面板 */
        .debugger-controls {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            align-items: center;
            margin-bottom: 12px;
            background: #eef3f7;
            padding: 10px 12px;
            border-radius: 8px;
        }
        .btn-dbg {
            border: none;
            padding: 8px 14px;
            border-radius: 6px;
            font-weight: bold;
            font-size: 0.9rem;
            cursor: pointer;
            color: white;
            transition: opacity 0.2s, transform 0.1s;
        }
        .btn-dbg:hover:not(:disabled) {
            opacity: 0.9;
            transform: translateY(-1px);
        }
        .btn-dbg:disabled {
            opacity: 0.45;
            cursor: not-allowed;
        }
        .btn-reset { background: #7f8c8d; }
        .btn-step { background: var(--primary-color); }
        .btn-auto { background: var(--success-color); }
        .btn-auto.playing { background: var(--accent-color); }

        .watch-panel {
            background: #f8f9fa;
            border: 1px solid #d5dbdb;
            border-radius: 8px;
            padding: 12px;
            margin-bottom: 12px;
        }
        .watch-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            margin-bottom: 10px;
        }
        .watch-item {
            background: white;
            border: 1px solid #e5e8e8;
            border-radius: 6px;
            padding: 8px 10px;
            text-align: center;
        }
        .watch-label {
            font-size: 0.78rem;
            color: #7f8c8d;
            display: block;
        }
        .watch-val {
            font-family: 'Consolas', monospace;
            font-size: 1.15rem;
            font-weight: bold;
            color: var(--secondary-color);
        }
        .watch-val.true-val { color: var(--success-color); }
        .watch-val.false-val { color: var(--danger-color); }

        .step-explanation {
            background: #fff8e7;
            border-left: 4px solid var(--accent-color);
            padding: 8px 12px;
            font-size: 0.92rem;
            color: #4a3b2c;
            border-radius: 0 6px 6px 0;
        }

        .console-box {
            background: #111417;
            color: #2ecc71;
            font-family: 'Consolas', 'Courier New', monospace;
            padding: 10px 14px;
            border-radius: 8px;
            min-height: 62px;
            font-size: 0.92rem;
        }
        .console-header {
            color: #95a5a6;
            font-size: 0.78rem;
            border-bottom: 1px solid #2c3e50;
            padding-bottom: 4px;
            margin-bottom: 6px;
            display: flex;
            justify-content: space-between;
        }

        /* 表格與警告區塊 */
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 16px 0;
        }
        th, td {
            border: 1px solid #dce4ec;
            padding: 11px 14px;
            text-align: left;
        }
        th {
            background-color: var(--primary-color);
            color: white;
            text-align: center;
        }
        .warning-box {
            background: #fdf2e9;
            border-left: 5px solid var(--accent-color);
            padding: 16px 20px;
            margin: 16px 0;
            border-radius: 0 8px 8px 0;
        }
        .warning-box h3 {
            margin-top: 0;
            color: #ba4a00;
        }
        .tool-box {
            background: #f4f8fb;
            border-left: 5px solid var(--primary-color);
            padding: 20px;
            margin: 22px 0;
            border-radius: 0 10px 10px 0;
        }
        .tool-box h3 {
            margin-top: 0;
            color: var(--secondary-color);
        }
        select, input[type="number"] {
            padding: 6px 10px;
            border-radius: 5px;
            border: 1px solid #bdc3c7;
            font-size: 0.9rem;
            font-weight: bold;
        }
        footer {
            text-align: center;
            padding: 22px;
            color: #7f8c8d;
            font-size: 0.9rem;
            margin-top: 30px;
        }

        /* 響應式斷點：平板與手機版自動堆疊 */
        @media (max-width: 960px) {
            .side-by-side-grid {
                grid-template-columns: 1fr;
            }
            .steps-banner {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        @media (max-width: 540px) {
            .steps-banner {
                grid-template-columns: 1fr;
            }
            .watch-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

<header>
    <h1>C++ 程式設計：重複結構 for 迴圈完全解析</h1>
    <p>流程圖與程式碼左右並排對照 × 逐步除錯追蹤器（Step-by-Step Debugger）</p>
</header>

<!-- 頂部導覽列 -->
<nav>
    <a data-target="tab-cover" class="nav-link active">🏠 首頁封面</a>
    <a data-target="tab-basic" class="nav-link">1. for 迴圈基礎與逐步除錯</a>
    <a data-target="tab-advanced" class="nav-link">2. 累加與進階應用</a>
    <a data-target="tab-practice" class="nav-link">3. 動手做練習 (2題)</a>
    <a data-target="tab-mistakes" class="nav-link">4. 易錯觀念與陷阱</a>
</nav>

<div class="container">

    <!-- 0. 網頁封面與引導語分頁 -->
    <div id="tab-cover" class="tab-content active">
        <div class="cover-box">
            <h2>🔄 歡迎來到 C++ 重複結構：for 迴圈互動教室</h2>
            <p>掌握程式設計中最核心的「計數型重複控制結構」，學會讓電腦高效率完成重複性任務！</p>
            
            <div class="welcome-box">
                <h3>👋 老師的學習引導語</h3>
                <p>各位同學好！在生活中我們經常需要重複做某件事（例如：跑操場 5 圈、計算 1 加到 100 的總和）。如果沒有迴圈，我們就必須把相同的程式碼複製貼上幾十次甚至幾千次！</p>
                <p style="margin-bottom:0;"><strong><code>for</code> 迴圈</strong>把「初始設定、條件判斷、變數更新」濃縮在同一行，結構清晰、不易出錯。本網頁特別設計了<strong>「流程圖與程式碼左右並排視圖」</strong>以及<strong>「逐步除錯追蹤器」</strong>，請大家務必多按幾次「下一步」，親眼觀察變數 <code>i</code> 是如何一步一步跳動的！</p>
            </div>

            <button class="start-btn" id="startCourseBtn">👉 點擊開始學習 for 迴圈</button>
        </div>
    </div>

    <!-- 主題一：for 迴圈基礎與逐步除錯 -->
    <div id="tab-basic" class="tab-content">
        <h2>1. for 迴圈語法結構與四步驟執行流程</h2>
        <p>當我們明確知道迴圈需要執行的「次數」時，<code>for</code> 迴圈是最佳選擇。它的執行順序嚴格遵循以下四個階段：</p>

        <!-- 四階段執行順序動態指示燈 -->
        <div class="steps-banner" id="basic-stage-banner">
            <div class="step-card" data-stage="init">
                <span class="step-badge">步驟 ① 初始設定</span>
                <strong>int i = 1;</strong>
                <span>僅在進入迴圈時執行「唯一一次」</span>
            </div>
            <div class="step-card" data-stage="cond">
                <span class="step-badge">步驟 ② 條件判斷</span>
                <strong>i &lt;= 5;</strong>
                <span>成立 (true) 進主體；不成立 (false) 結束</span>
            </div>
            <div class="step-card" data-stage="body">
                <span class="step-badge">步驟 ③ 迴圈主體</span>
                <strong>cout &lt;&lt; i &lt;&lt; " ";</strong>
                <span>執行大括號 { } 內的指令</span>
            </div>
            <div class="step-card" data-stage="update">
                <span class="step-badge">步驟 ④ 更新變數</span>
                <strong>i++</strong>
                <span>調整計數器後，回到「步驟 ②」繼續判斷</span>
            </div>
        </div>

        <!-- 並排區域：左側流程圖 + 右側可逐步除錯程式碼 -->
        <div class="side-by-side-grid">
            <!-- 左側：Mermaid 流程圖 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>📊 執行邏輯流程圖（輸出 1 到 5）</span>
                    <span style="font-size:0.85rem; color:#2980b9;">對照右側程式碼觀察循環路徑</span>
                </div>
                <div class="flowchart-container">
                    <div class="mermaid">
                        graph TD
                            Start([開始]) --> Init["① 初始設定: i = 1"]
                            Init --> Cond{"② 條件判斷: i <= 5 ?"}
                            Cond -- "True (成立)" --> Body["③ 迴圈主體: 輸出 i"]
                            Body --> Update["④ 更新變數: i++ (i = i + 1)"]
                            Update --> Cond
                            Cond -- "False (不成立)" --> End([結束迴圈])
                            
                            style Init fill:#eaf2f8,stroke:#2980b9,stroke-width:2px
                            style Cond fill:#fef9e7,stroke:#f39c12,stroke-width:2px
                            style Body fill:#e8f8f5,stroke:#27ae60,stroke-width:2px
                            style Update fill:#f5eef8,stroke:#8e44ad,stroke-width:2px
                    </div>
                </div>
            </div>

            <!-- 右側：逐步除錯追蹤器 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>🐞 逐步除錯追蹤器 (Step-by-Step Debugger)</span>
                    <span id="basic-step-counter" style="font-size:0.85rem; color:#e67e22;">步驟：0 / 18</span>
                </div>

                <!-- 控制按鈕 -->
                <div class="debugger-controls">
                    <button class="btn-dbg btn-reset" id="basicResetBtn">⏮ 重置 (Reset)</button>
                    <button class="btn-dbg btn-step" id="basicStepBtn">⏭ 下一步 (Step Next)</button>
                    <button class="btn-dbg btn-auto" id="basicAutoBtn">▶ 自動播放</button>
                </div>

                <!-- 程式碼顯示區 (含行號與執行指標) -->
                <div class="debug-code-box" id="basic-code-lines"></div>

                <!-- 變數監控面板 -->
                <div class="watch-panel">
                    <div class="watch-grid">
                        <div class="watch-item">
                            <span class="watch-label">計數變數 i</span>
                            <span class="watch-val" id="basic-var-i">未宣告</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">條件判斷 (i &lt;= 5)</span>
                            <span class="watch-val" id="basic-var-cond">-</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">目前執行階段</span>
                            <span class="watch-val" id="basic-var-stage" style="font-size:0.95rem;">準備開始</span>
                        </div>
                    </div>
                    <div class="step-explanation" id="basic-explanation">
                        點擊 <strong>「⏭ 下一步」</strong> 開始單步追蹤 C++ 程式如何執行 <code>for</code> 迴圈！
                    </div>
                </div>

                <!-- 模擬終端機輸出 -->
                <div class="console-box">
                    <div class="console-header">
                        <span>💻 Console 終端機輸出視窗 (cout)</span>
                        <span>標準輸出</span>
                    </div>
                    <div id="basic-console-output"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- 主題二：累加與進階應用 -->
    <div id="tab-advanced" class="tab-content">
        <h2>2. 累加器模式 (Accumulator) 與進階步進變化</h2>
        <p>在程式設計中，我們常利用 <code>for</code> 迴圈搭配一個「累加變數（如 <code>sum = 0</code>）」來計算總和，也可以透過修改更新運算式實現「間隔跳躍 (<code>i += 2</code>)」或「倒數計時 (<code>i--</code>)」。</p>

        <!-- 並排區域：左側累加流程圖 + 右側可選擇模式的逐步除錯器 -->
        <div class="side-by-side-grid">
            <!-- 左側：累加流程圖與變體對照表 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>📊 累加計算流程圖（sum = sum + i）</span>
                </div>
                <div class="flowchart-container">
                    <div class="mermaid">
                        graph TD
                            A([開始]) --> B["設定 sum = 0, 輸入 N"]
                            B --> C["① 初始: i = 1"]
                            C --> D{"② 判斷: i <= N ?"}
                            D -- "True" --> E["③ 主體: sum = sum + i"]
                            E --> F["④ 更新: i++"]
                            F --> D
                            D -- "False" --> G["輸出總和 sum"]
                            G --> H([結束])
                            
                            style E fill:#e8f8f5,stroke:#27ae60,stroke-width:2px
                            style D fill:#fef9e7,stroke:#f39c12,stroke-width:2px
                    </div>
                </div>

                <table style="margin-top:14px; margin-bottom:0; font-size:0.9rem;">
                    <thead>
                        <tr>
                            <th>進階寫法</th>
                            <th>for 標頭範例</th>
                            <th>產生序列</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><strong>奇數遞增</strong></td>
                            <td><code>for (int i=1; i&lt;=7; i+=2)</code></td>
                            <td>1, 3, 5, 7</td>
                        </tr>
                        <tr>
                            <td><strong>倒數計時</strong></td>
                            <td><code>for (int i=5; i&gt;=1; i--)</code></td>
                            <td>5, 4, 3, 2, 1</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <!-- 右側：累加計算逐步除錯器 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>🐞 累加演算法逐步除錯器</span>
                    <div>
                        <label for="adv-n-select" style="font-size:0.85rem;">設定 N = </label>
                        <select id="adv-n-select">
                            <option value="3">3 (1+2+3)</option>
                            <option value="4" selected>4 (1+2+3+4)</option>
                            <option value="5">5 (1+2+3+4+5)</option>
                        </select>
                    </div>
                </div>

                <div class="debugger-controls">
                    <button class="btn-dbg btn-reset" id="advResetBtn">⏮ 重置 (Reset)</button>
                    <button class="btn-dbg btn-step" id="advStepBtn">⏭ 下一步 (Step Next)</button>
                    <button class="btn-dbg btn-auto" id="advAutoBtn">▶ 自動播放</button>
                    <span id="adv-step-counter" style="margin-left:auto; font-size:0.85rem; font-weight:bold; color:#e67e22;">步驟：0</span>
                </div>

                <div class="debug-code-box" id="adv-code-lines"></div>

                <div class="watch-panel">
                    <div class="watch-grid">
                        <div class="watch-item">
                            <span class="watch-label">變數 i / N</span>
                            <span class="watch-val" id="adv-var-i">- / 4</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">累加器 sum</span>
                            <span class="watch-val" id="adv-var-sum" style="color:#2980b9;">未宣告</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">條件 (i &lt;= N)</span>
                            <span class="watch-val" id="adv-var-cond">-</span>
                        </div>
                    </div>
                    <div class="step-explanation" id="adv-explanation">
                        請按 <strong>「⏭ 下一步」</strong> 觀察 <code>sum</code> 如何每一輪把當下的 <code>i</code> 累加進去！
                    </div>
                </div>

                <div class="console-box">
                    <div class="console-header">
                        <span>💻 Console 終端機輸出視窗 (cout)</span>
                        <span>標準輸出</span>
                    </div>
                    <div id="adv-console-output"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- 主題三：動手做練習 (2題，每題皆為流程圖與程式碼左右並排) -->
    <div id="tab-practice" class="tab-content">
        <h2>3. 動手做綜合練習（流程圖與程式碼並排對照）</h2>
        <p>請對照左側的邏輯流程圖與右側的 C++ 完整程式碼，觀察 <code>for</code> 迴圈如何結合 <code>if</code> 條件判斷與算術運算解決實際問題：</p>

        <!-- 練習題 1 -->
        <div class="tool-box">
            <h3>練習題 1：找出區間內所有 3 的倍數並計算個數</h3>
            <p><strong>題目說明：</strong>輸入一個正整數 <code>N</code>，利用 <code>for</code> 迴圈從 1 檢查到 <code>N</code>，輸出所有 3 的倍數（<code>i % 3 == 0</code>），並統計總共有幾個。</p>
            
            <div class="side-by-side-grid">
                <div class="panel-card">
                    <div class="panel-title"><span>📊 題目 1 邏輯流程圖</span></div>
                    <div class="flowchart-container">
                        <div class="mermaid">
                            graph TD
                                A([開始]) --> B["輸入 N, 設定 count = 0"]
                                B --> C["i = 1"]
                                C --> D{"i <= N ?"}
                                D -- "True" --> E{"i % 3 == 0 ?"}
                                E -- "True" --> F["輸出 i, count++"]
                                E -- "False" --> G["i++"]
                                F --> G
                                G --> D
                                D -- "False" --> H["輸出總個數 count"]
                                H --> I([結束])
                        </div>
                    </div>
                </div>

                <div class="panel-card">
                    <div class="panel-title"><span>💻 題目 1 C++ 參考解答</span></div>
                    <pre class="static-code"><code>#include &lt;iostream&gt;
using namespace std;

int main() {
    int N, count = 0;
    cout &lt;&lt; "請輸入正整數 N：";
    cin &gt;&gt; N;

    for (int i = 1; i &lt;= N; i++) {
        // 判斷 i 是否為 3 的倍數
        if (i % 3 == 0) {
            cout &lt;&lt; i &lt;&lt; " ";
            count++; // 找到一個，計數器加 1
        }
    }

    cout &lt;&lt; endl &lt;&lt; "總共有 " &lt;&lt; count &lt;&lt; " 個 3 的倍數" &lt;&lt; endl;
    return 0;
}</code></pre>
                </div>
            </div>
        </div>

        <!-- 練習題 2 -->
        <div class="tool-box">
            <h3>練習題 2：計算 N 階乘 (N! = 1 × 2 × 3 × ... × N)</h3>
            <p><strong>題目說明：</strong>輸入一個正整數 <code>N</code>（例如 5），計算 <code>1 × 2 × 3 × ... × N</code> 的連乘積。注意：連乘器的初始值必須設為 <strong><code>1</code></strong>（若設為 0，乘任何數都會變成 0）。</p>
            
            <div class="side-by-side-grid">
                <div class="panel-card">
                    <div class="panel-title"><span>📊 題目 2 邏輯流程圖</span></div>
                    <div class="flowchart-container">
                        <div class="mermaid">
                            graph TD
                                A([開始]) --> B["輸入 N, 設定 fact = 1"]
                                B --> C["i = 1"]
                                C --> D{"i <= N ?"}
                                D -- "True" --> E["fact = fact * i"]
                                E --> F["i++"]
                                F --> D
                                D -- "False" --> G["輸出 N! = fact"]
                                G --> H([結束])
                        </div>
                    </div>
                </div>

                <div class="panel-card">
                    <div class="panel-title"><span>💻 題目 2 C++ 參考解答</span></div>
                    <pre class="static-code"><code>#include &lt;iostream&gt;
using namespace std;

int main() {
    int N;
    long long fact = 1; // 連乘初始值必須為 1，且用 long long 避免溢位
    cout &lt;&lt; "請輸入欲計算階乘的正整數 N：";
    cin &gt;&gt; N;

    for (int i = 1; i &lt;= N; i++) {
        fact = fact * i; // 每一輪將 i 乘進 fact 中
    }

    cout &lt;&lt; N &lt;&lt; "! = " &lt;&lt; fact &lt;&lt; endl;
    return 0;
}</code></pre>
                </div>
            </div>
        </div>
    </div>

    <!-- 主題四：易錯觀念與陷阱 -->
    <div id="tab-mistakes" class="tab-content">
        <h2>4. 初學者寫 for 迴圈最常犯的 5 大陷阱</h2>
        <p>寫迴圈時只要差一個符號，就可能導致結果少算一次，甚至變成永遠停不下來的「無窮迴圈」！</p>

        <div class="warning-box">
            <h3>陷阱一：無窮迴圈 (Infinite Loop) —— 條件永遠成立或更新方向寫反</h3>
            <p>例如：<code>for (int i = 1; i &gt; 0; i++)</code>（<code>i</code> 從 1 開始一直往上加，永遠大於 0），或是倒數時寫成 <code>for (int i = 10; i &gt;= 1; i++)</code>（越加越大，永遠無法小於 1），程式會卡死不斷執行。</p>
        </div>

        <div class="warning-box">
            <h3>陷阱二：差一錯誤 (Off-by-One Error) —— <code>&lt;</code> 與 <code>&lt;=</code> 混淆</h3>
            <p>若要從 1 跑到 5，寫成 <code>for (int i = 1; i &lt; 5; i++)</code> 只會執行 1, 2, 3, 4（共 4 次，漏掉 5）；必須寫成 <code>i &lt;= 5</code> 才會包含 5。</p>
        </div>

        <div class="warning-box">
            <h3>陷阱三：在 <code>for (...)</code> 小括號後面誤加分號 <code>;</code></h3>
            <p>寫成 <code>for (int i = 1; i &lt;= 5; i++); { cout &lt;&lt; i; }</code> 時，分號代表一個「空敘述」，迴圈會自己空轉 5 次結束，大括號裡的內容只會執行 1 次（甚至產生變數未宣告的編譯錯誤）。</p>
        </div>

        <div class="warning-box">
            <h3>陷阱四：在迴圈主體內隨意修改計數變數 <code>i</code></h3>
            <p>在 <code>for</code> 迴圈的大括號內部又寫了 <code>i++</code> 或 <code>i = 10</code>，會破壞原本設計好的步進節奏，導致跳過某些次數或提前結束，難以除錯。</p>
        </div>

        <div class="warning-box">
            <h3>陷阱五：變數生命週期 (Scope) 錯誤 —— 在迴圈外部存取 <code>i</code></h3>
            <p>若在標頭宣告 <code>for (int i = 1; i &lt;= 5; i++)</code>，則變數 <code>i</code> 只存在於該 <code>for</code> 迴圈的大括號範圍內；迴圈結束後在外面寫 <code>cout &lt;&lt; i;</code> 會發生編譯錯誤（<code>'i' was not declared in this scope</code>）。</p>
        </div>
    </div>

</div>

<footer>
    &copy; 2026 C++ 程式設計：重複結構 for 迴圈互動教學網頁
</footer>

<script>
    // 初始化 Mermaid（關閉自動載入，改由分頁切換時精準渲染）
    mermaid.initialize({ startOnLoad: false, theme: 'default' });

    // 渲染指定容器內的 Mermaid 圖表
    function renderMermaidInContainer(containerId) {
        const container = document.getElementById(containerId);
        if (!container) return;
        const nodes = container.querySelectorAll('.mermaid');
        nodes.forEach(node => {
            if (!node.getAttribute('data-processed')) {
                mermaid.init(undefined, node);
            }
        });
    }

    // 分頁切換函式
    function switchTab(targetId) {
        // 切換分頁時停止所有自動播放計時器
        stopBasicAuto();
        stopAdvAuto();

        document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));

        const targetContent = document.getElementById(targetId);
        if (targetContent) targetContent.classList.add('active');

        const targetLink = document.querySelector(`.nav-link[data-target="${targetId}"]`);
        if (targetLink) targetLink.classList.add('active');

        setTimeout(() => renderMermaidInContainer(targetId), 50);
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    document.querySelectorAll('.nav-link').forEach(link => {
        link.addEventListener('click', function() {
            switchTab(this.getAttribute('data-target'));
        });
    });

    document.getElementById('startCourseBtn').addEventListener('click', () => {
        switchTab('tab-basic');
    });

    // =========================================================================
    // 除錯器 1：主題一（輸出 1 到 5 基礎 for 迴圈逐步追蹤器）
    // =========================================================================
    const basicCodeArray = [
        '#include <iostream>',
        'using namespace std;',
        'int main() {',
        '    for (int i = 1; i <= 5; i++) {',
        '        cout << i << " ";',
        '    }',
        '    return 0;',
        '}'
    ];

    // 預先建立完整的單步執行軌跡 (Trace Steps)
    function buildBasicTrace() {
        const steps = [];
        steps.push({
            line: 3, i: '未宣告', cond: '-', stage: '準備進入 main', stageKey: '',
            output: '',
            msg: '程式從 <code>main()</code> 函式開始執行，即將進入 <code>for</code> 迴圈。'
        });
        steps.push({
            line: 4, i: '1', cond: '-', stage: '① 初始設定', stageKey: 'init',
            output: '',
            msg: '<strong>步驟 ① 初始設定：</strong>宣告整數變數 <code>i = 1</code>（此動作整個迴圈只會執行這 1 次！）。'
        });

        let outStr = '';
        for (let val = 1; val <= 5; val++) {
            steps.push({
                line: 4, i: String(val), cond: `true (${val} <= 5)`, stage: '② 條件判斷', stageKey: 'cond',
                output: outStr,
                msg: `<strong>步驟 ② 條件判斷：</strong>檢查 <code>${val} &lt;= 5</code>，結果為 <strong style="color:#27ae60;">true (成立)</strong>，進入迴圈大括號執行主體。`
            });
            outStr += val + ' ';
            steps.push({
                line: 5, i: String(val), cond: 'true', stage: '③ 迴圈主體', stageKey: 'body',
                output: outStr,
                msg: `<strong>步驟 ③ 迴圈主體：</strong>執行 <code>cout &lt;&lt; i &lt;&lt; " ";</code>，在終端機印出數字 <strong>${val}</strong>。`
            });
            steps.push({
                line: 4, i: String(val + 1), cond: '-', stage: '④ 更新變數', stageKey: 'update',
                output: outStr,
                msg: `<strong>步驟 ④ 更新變數：</strong>執行 <code>i++</code>，變數 <code>i</code> 由 ${val} 增加為 <strong>${val + 1}</strong>，接著跳回條件判斷。`
            });
        }

        // 第 6 次條件判斷失敗
        steps.push({
            line: 4, i: '6', cond: 'false (6 <= 5)', stage: '② 條件判斷 (結束)', stageKey: 'cond',
            output: outStr,
            msg: '<strong>步驟 ② 條件判斷：</strong>檢查 <code>6 &lt;= 5</code>，結果為 <strong style="color:#c0392b;">false (不成立)</strong>！立即跳出 <code>for</code> 迴圈。'
        });
        steps.push({
            line: 7, i: '已釋放', cond: 'false', stage: '迴圈結束', stageKey: '',
            output: outStr,
            msg: '迴圈完全結束，執行 <code>return 0;</code> 正常結束程式！共印出 1 2 3 4 5。'
        });
        return steps;
    }

    const basicTrace = buildBasicTrace();
    let basicStepIdx = 0;
    let basicAutoTimer = null;

    function initBasicCodeUI() {
        const box = document.getElementById('basic-code-lines');
        box.innerHTML = basicCodeArray.map((code, idx) => `
            <div class="code-line" id="basic-line-${idx + 1}">
                <span class="ln">${idx + 1}</span>
                <span class="pointer-arrow">➤</span>
                <span class="code-text">${escapeHtml(code)}</span>
            </div>
        `).join('');
        updateBasicUI();
    }

    function updateBasicUI() {
        const state = basicTrace[basicStepIdx];
        document.querySelectorAll('#basic-code-lines .code-line').forEach(el => el.classList.remove('active-line'));
        const activeEl = document.getElementById(`basic-line-${state.line}`);
        if (activeEl) activeEl.classList.add('active-line');

        document.getElementById('basic-var-i').textContent = state.i;
        const condEl = document.getElementById('basic-var-cond');
        condEl.textContent = state.cond;
        condEl.className = 'watch-val ' + (state.cond.startsWith('true') ? 'true-val' : (state.cond.startsWith('false') ? 'false-val' : ''));

        document.getElementById('basic-var-stage').textContent = state.stage;
        document.getElementById('basic-explanation').innerHTML = state.msg;
        document.getElementById('basic-console-output').textContent = state.output || '(尚未輸出)';
        document.getElementById('basic-step-counter').textContent = `步驟：${basicStepIdx} / ${basicTrace.length - 1}`;

        // 同步高亮上方四階段卡片
        document.querySelectorAll('#basic-stage-banner .step-card').forEach(card => {
            card.classList.toggle('highlight-stage', card.getAttribute('data-stage') === state.stageKey);
        });

        document.getElementById('basicStepBtn').disabled = (basicStepIdx >= basicTrace.length - 1);
    }

    function stopBasicAuto() {
        if (basicAutoTimer) {
            clearInterval(basicAutoTimer);
            basicAutoTimer = null;
        }
        const btn = document.getElementById('basicAutoBtn');
        btn.textContent = '▶ 自動播放';
        btn.classList.remove('playing');
    }

    document.getElementById('basicStepBtn').addEventListener('click', () => {
        if (basicStepIdx < basicTrace.length - 1) {
            basicStepIdx++;
            updateBasicUI();
        }
    });

    document.getElementById('basicResetBtn').addEventListener('click', () => {
        stopBasicAuto();
        basicStepIdx = 0;
        updateBasicUI();
    });

    document.getElementById('basicAutoBtn').addEventListener('click', function() {
        if (basicAutoTimer) {
            stopBasicAuto();
        } else {
            if (basicStepIdx >= basicTrace.length - 1) basicStepIdx = 0;
            this.textContent = '⏸ 暫停播放';
            this.classList.add('playing');
            basicAutoTimer = setInterval(() => {
                if (basicStepIdx < basicTrace.length - 1) {
                    basicStepIdx++;
                    updateBasicUI();
                } else {
                    stopBasicAuto();
                }
            }, 950);
        }
    });

    // =========================================================================
    // 除錯器 2：主題二（累加演算法 sum += i 逐步追蹤器）
    // =========================================================================
    const advCodeArray = [
        '#include <iostream>',
        'using namespace std;',
        'int main() {',
        '    int N = 4, sum = 0;',
        '    for (int i = 1; i <= N; i++) {',
        '        sum = sum + i;',
        '    }',
        '    cout << "總和 = " << sum << endl;',
        '    return 0;',
        '}'
    ];

    function buildAdvTrace(nVal) {
        const steps = [];
        steps.push({
            line: 3, i: '-', sum: '未宣告', cond: '-', output: '',
            msg: '開始執行 <code>main()</code> 函式。'
        });
        steps.push({
            line: 4, i: '-', sum: '0', cond: '-', output: '',
            msg: `宣告上限 <code>N = ${nVal}</code>，並將累加器 <code>sum</code> 初始化為 <strong>0</strong>。`
        });
        steps.push({
            line: 5, i: '1', sum: '0', cond: '-', output: '',
            msg: '<strong>① 初始設定：</strong>計數器 <code>i = 1</code>。'
        });

        let currentSum = 0;
        for (let val = 1; val <= nVal; val++) {
            steps.push({
                line: 5, i: String(val), sum: String(currentSum), cond: `true (${val} <= ${nVal})`, output: '',
                msg: `<strong>② 條件判斷：</strong><code>${val} &lt;= ${nVal}</code> 為 <strong style="color:#27ae60;">true</strong>，進入迴圈進行累加。`
            });
            const prevSum = currentSum;
            currentSum += val;
            steps.push({
                line: 6, i: String(val), sum: String(currentSum), cond: 'true', output: '',
                msg: `<strong>③ 累加主體：</strong><code>sum = ${prevSum} + ${val}</code>，<code>sum</code> 更新為 <strong>${currentSum}</strong>。`
            });
            steps.push({
                line: 5, i: String(val + 1), sum: String(currentSum), cond: '-', output: '',
                msg: `<strong>④ 更新變數：</strong><code>i++</code> 使 <code>i</code> 變為 <strong>${val + 1}</strong>。`
            });
        }

        steps.push({
            line: 5, i: String(nVal + 1), sum: String(currentSum), cond: `false (${nVal + 1} <= ${nVal})`, output: '',
            msg: `<strong>② 條件判斷：</strong><code>${nVal + 1} &lt;= ${nVal}</code> 為 <strong style="color:#c0392b;">false</strong>，跳出迴圈！`
        });
        steps.push({
            line: 8, i: '已釋放', sum: String(currentSum), cond: 'false', output: `總和 = ${currentSum}`,
            msg: `執行 <code>cout</code> 輸出最終累加結果：<strong>總和 = ${currentSum}</strong>。`
        });
        return steps;
    }

    let currentN = 4;
    let advTrace = buildAdvTrace(currentN);
    let advStepIdx = 0;
    let advAutoTimer = null;

    function renderAdvCodeLines() {
        const dynamicCode = [...advCodeArray];
        dynamicCode[3] = `    int N = ${currentN}, sum = 0;`;
        const box = document.getElementById('adv-code-lines');
        box.innerHTML = dynamicCode.map((code, idx) => `
            <div class="code-line" id="adv-line-${idx + 1}">
                <span class="ln">${idx + 1}</span>
                <span class="pointer-arrow">➤</span>
                <span class="code-text">${escapeHtml(code)}</span>
            </div>
        `).join('');
    }

    function updateAdvUI() {
        const state = advTrace[advStepIdx];
        document.querySelectorAll('#adv-code-lines .code-line').forEach(el => el.classList.remove('active-line'));
        const activeEl = document.getElementById(`adv-line-${state.line}`);
        if (activeEl) activeEl.classList.add('active-line');

        document.getElementById('adv-var-i').textContent = `${state.i} / ${currentN}`;
        document.getElementById('adv-var-sum').textContent = state.sum;
        const condEl = document.getElementById('adv-var-cond');
        condEl.textContent = state.cond;
        condEl.className = 'watch-val ' + (state.cond.startsWith('true') ? 'true-val' : (state.cond.startsWith('false') ? 'false-val' : ''));

        document.getElementById('adv-explanation').innerHTML = state.msg;
        document.getElementById('adv-console-output').textContent = state.output || '(尚未輸出)';
        document.getElementById('adv-step-counter').textContent = `步驟：${advStepIdx} / ${advTrace.length - 1}`;
        document.getElementById('advStepBtn').disabled = (advStepIdx >= advTrace.length - 1);
    }

    function stopAdvAuto() {
        if (advAutoTimer) {
            clearInterval(advAutoTimer);
            advAutoTimer = null;
        }
        const btn = document.getElementById('advAutoBtn');
        btn.textContent = '▶ 自動播放';
        btn.classList.remove('playing');
    }

    document.getElementById('adv-n-select').addEventListener('change', function() {
        stopAdvAuto();
        currentN = Number(this.value);
        advTrace = buildAdvTrace(currentN);
        advStepIdx = 0;
        renderAdvCodeLines();
        updateAdvUI();
    });

    document.getElementById('advStepBtn').addEventListener('click', () => {
        if (advStepIdx < advTrace.length - 1) {
            advStepIdx++;
            updateAdvUI();
        }
    });

    document.getElementById('advResetBtn').addEventListener('click', () => {
        stopAdvAuto();
        advStepIdx = 0;
        updateAdvUI();
    });

    document.getElementById('advAutoBtn').addEventListener('click', function() {
        if (advAutoTimer) {
            stopAdvAuto();
        } else {
            if (advStepIdx >= advTrace.length - 1) advStepIdx = 0;
            this.textContent = '⏸ 暫停播放';
            this.classList.add('playing');
            advAutoTimer = setInterval(() => {
                if (advStepIdx < advTrace.length - 1) {
                    advStepIdx++;
                    updateAdvUI();
                } else {
                    stopAdvAuto();
                }
            }, 950);
        }
    });

    function escapeHtml(str) {
        return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
    }

    // 頁面載入完成初始化兩個除錯器
    window.addEventListener('DOMContentLoaded', () => {
        initBasicCodeUI();
        renderAdvCodeLines();
        updateAdvUI();
    });
</script>

</body>
</html>
