<!DOCTYPE html>
<html lang="zh-Hant" translate="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="google" content="notranslate">
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
        *, *::before, *::after {
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
        .watch-val.continue-val { color: var(--accent-color); }
        .watch-val.break-val { color: var(--danger-color); }
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
        .badge-flow {
            display: inline-block;
            padding: 2px 8px;
            border-radius: 10px;
            font-size: 0.8rem;
            font-weight: bold;
            color: white;
            background: var(--primary-color);
        }
        .badge-flow.is-continue {
            background: var(--accent-color);
        }
        .badge-flow.is-break {
            background: var(--danger-color);
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

<nav>
    <a data-target="tab-cover" class="nav-link active">🏠 首頁封面</a>
    <a data-target="tab-basic" class="nav-link">1. for 迴圈基礎與逐步除錯</a>
    <a data-target="tab-advanced" class="nav-link">2. 累加與進階應用</a>
    <a data-target="tab-practice" class="nav-link">3. 動手做練習 (2題)</a>
    <a data-target="tab-mistakes" class="nav-link">4. 易錯觀念與陷阱</a>
    <a data-target="tab-control" class="nav-link">5. continue 與 break 除錯</a>
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

        <div class="side-by-side-grid">
            <!-- 左側：Mermaid 流程圖 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>📊 執行邏輯流程圖（輸出 1 到 5）</span>
                    <span style="font-size:0.85rem; color:#2980b9;">對照右側程式碼觀察循環路徑</span>
                </div>
                <div class="flowchart-container">
                    <div class="mermaid notranslate">
flowchart TD
    Start(["開始"]) --> Init["① 初始設定 i = 1"]
    Init --> Cond{"② 條件判斷 i &lt;= 5"}
    Cond -- "True 成立" --> Body["③ 迴圈主體 輸出 i"]
    Body --> Update["④ 更新變數 i++"]
    Update --> Cond
    Cond -- "False 不成立" --> End(["結束迴圈"])

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
                    <span id="basic-step-counter" style="font-size:0.85rem; color:#e67e22;">步驟：0 / 22</span>
                </div>

                <div class="debugger-controls">
                    <button class="btn-dbg btn-reset" id="basicResetBtn">⏮ 重置 (Reset)</button>
                    <button class="btn-dbg btn-step" id="basicStepBtn">⏭ 下一步 (Step Next)</button>
                    <button class="btn-dbg btn-auto" id="basicAutoBtn">▶ 自動播放</button>
                </div>

                <div class="debug-code-box notranslate" id="basic-code-lines"></div>

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

                <div class="console-box">
                    <div class="console-header">
                        <span>💻 Console 終端機輸出視窗 (cout)</span>
                        <span>標準輸出</span>
                    </div>
                    <div id="basic-console-output" class="notranslate"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- 主題二：累加與進階應用 -->
    <div id="tab-advanced" class="tab-content">
        <h2>2. 累加器模式 (Accumulator) 與進階步進變化</h2>
        <p>在程式設計中，我們常利用 <code>for</code> 迴圈搭配一個「累加變數（如 <code>sum = 0</code>）」來計算總和，也可以透過修改更新運算式實現「間隔跳躍 (<code>i += 2</code>)」或「倒數計時 (<code>i--</code>)」。</p>

        <div class="side-by-side-grid">
            <!-- 左側：累加流程圖與變體對照表 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>📊 累加計算流程圖（sum = sum + i）</span>
                </div>
                <div class="flowchart-container">
                    <div class="mermaid notranslate">
flowchart TD
    A(["開始"]) --> B["設定 sum = 0 並輸入 N"]
    B --> C["① 初始設定 i = 1"]
    C --> D{"② 條件判斷 i &lt;= N"}
    D -- "True" --> E["③ 迴圈主體 sum = sum + i"]
    E --> F["④ 更新變數 i++"]
    F --> D
    D -- "False" --> G["輸出總和 sum"]
    G --> H(["結束"])

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

                <div class="debug-code-box notranslate" id="adv-code-lines"></div>

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
                    <div id="adv-console-output" class="notranslate"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- 主題三：動手做練習 (2題) -->
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
                        <div class="mermaid notranslate">
flowchart TD
    A(["開始"]) --> B["輸入 N 並設定 count = 0"]
    B --> C["i = 1"]
    C --> D{"i &lt;= N"}
    D -- "True" --> E{"i % 3 == 0"}
    E -- "True" --> F["輸出 i 且 count++"]
    E -- "False" --> G["i++"]
    F --> G
    G --> D
    D -- "False" --> H["輸出總個數 count"]
    H --> I(["結束"])
                        </div>
                    </div>
                </div>

                <div class="panel-card">
                    <div class="panel-title"><span>💻 題目 1 C++ 參考解答</span></div>
                    <pre class="static-code notranslate"><code>#include &lt;iostream&gt;
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
                        <div class="mermaid notranslate">
flowchart TD
    A(["開始"]) --> B["輸入 N 並設定 fact = 1"]
    B --> C["i = 1"]
    C --> D{"i &lt;= N"}
    D -- "True" --> E["fact = fact * i"]
    E --> F["i++"]
    F --> D
    D -- "False" --> G["輸出 N! = fact"]
    G --> H(["結束"])
                        </div>
                    </div>
                </div>

                <div class="panel-card">
                    <div class="panel-title"><span>💻 題目 2 C++ 參考解答</span></div>
                    <pre class="static-code notranslate"><code>#include &lt;iostream&gt;
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

    <!-- 主題五：continue 與 break 流程控制與逐步除錯 -->
    <div id="tab-control" class="tab-content">
        <h2>5. 迴圈控制指令：<code>continue</code> 與 <code>break</code> 逐步除錯</h2>
        <p>
            以下範例讓 <code>i</code> 從 <code>1</code> 跑到 <code>5</code>：當 <code>i == 2</code> 時執行
            <strong><code>continue</code></strong>（跳過印出 2，直接執行 <code>i++</code> 進入下一輪）；當 <code>i == 4</code> 時執行
            <strong><code>break</code></strong>（直接跳出整個 <code>for</code> 迴圈，<code>5</code> 也不會執行）。
        </p>

        <table>
            <thead>
                <tr>
                    <th>控制指令</th>
                    <th>遇到時的行為</th>
                    <th>對 <code>for</code> 更新變數（<code>i++</code>）的影響</th>
                    <th>常見應用情境</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong><code>continue</code></strong></td>
                    <td>跳過本次迴圈剩餘敘述，直接進入下一輪</td>
                    <td><strong>會執行</strong>更新變數（如 <code>i++</code>），再重新判斷條件</td>
                    <td>過濾特定條件（例如：跳過偶數、跳過不合法輸入）</td>
                </tr>
                <tr>
                    <td><strong><code>break</code></strong></td>
                    <td>立刻強制跳出並結束整個 <code>for</code> 迴圈</td>
                    <td><strong>不再執行</strong>後續更新與條件判斷，直接前往迴圈後敘述</td>
                    <td>提前達標或找到目標（例如：找到第一個符合條件的值就停止）</td>
                </tr>
            </tbody>
        </table>

        <div class="side-by-side-grid">
            <!-- 左側：Mermaid 流程圖 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>📊 continue 與 break 執行流程圖</span>
                    <span class="badge-flow" id="ctrl-flow-badge">一般循環</span>
                </div>
                <div class="flowchart-container">
                    <div class="mermaid notranslate">
flowchart TD
    Start(["開始"]) --> Init["i = 1"]
    Init --> Cond{"i &lt;= 5"}
    Cond -- "False" --> End(["迴圈結束，執行迴圈後程式"])
    Cond -- "True" --> IfBreak{"i == 4 ?"}
    IfBreak -- "True" --> BreakStmt["break：立刻跳出迴圈"]
    BreakStmt --> End
    IfBreak -- "False" --> IfCont{"i == 2 ?"}
    IfCont -- "True" --> ContStmt["continue：跳過下方輸出"]
    ContStmt --> Update["i++"]
    IfCont -- "False" --> Print["輸出 i"]
    Print --> Update
    Update --> Cond

    style ContStmt fill:#fef5e7,stroke:#e67e22,stroke-width:2px
    style BreakStmt fill:#fdedec,stroke:#c0392b,stroke-width:2px
                    </div>
                </div>
            </div>

            <!-- 右側：逐步除錯追蹤器 -->
            <div class="panel-card">
                <div class="panel-title">
                    <span>🐞 continue &amp; break 逐步除錯追蹤器</span>
                    <span id="ctrl-step-counter" style="font-size:0.85rem; color:#e67e22;">步驟：0 / 15</span>
                </div>

                <div class="debugger-controls">
                    <button class="btn-dbg btn-reset" id="ctrlResetBtn">⏮ 重置 (Reset)</button>
                    <button class="btn-dbg btn-step" id="ctrlStepBtn">⏭ 下一步 (Step Next)</button>
                    <button class="btn-dbg btn-auto" id="ctrlAutoBtn">▶ 自動播放</button>
                </div>

                <div class="debug-code-box notranslate" id="ctrl-code-lines"></div>

                <div class="watch-panel">
                    <div class="watch-grid">
                        <div class="watch-item">
                            <span class="watch-label">計數變數 i</span>
                            <span class="watch-val" id="ctrl-var-i">未宣告</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">控制狀態</span>
                            <span class="watch-val" id="ctrl-var-status">準備開始</span>
                        </div>
                        <div class="watch-item">
                            <span class="watch-label">條件 (i &lt;= 5)</span>
                            <span class="watch-val" id="ctrl-var-cond">-</span>
                        </div>
                    </div>
                    <div class="step-explanation" id="ctrl-explanation">
                        點擊 <strong>「⏭ 下一步」</strong> 觀察 <code>i = 2</code> 遇到 <code>continue</code> 與 <code>i = 4</code> 遇到 <code>break</code> 時的跳轉路徑！
                    </div>
                </div>

                <div class="console-box">
                    <div class="console-header">
                        <span>💻 Console 終端機輸出視窗 (cout)</span>
                        <span>標準輸出</span>
                    </div>
                    <div id="ctrl-console-output" class="notranslate"></div>
                </div>
            </div>
        </div>
    </div>

</div>

<footer>
    &copy; 2026 C++ 程式設計：重複結構 for 迴圈互動教學網頁
</footer>

<script>
    mermaid.initialize({ startOnLoad: false, theme: 'default' });

    // 先保存每個 .mermaid 元素的原始文字，避免重複渲染或意外修改後無法重繪
    document.querySelectorAll('.mermaid').forEach(node => {
        if (!node.dataset.rawDefinition) {
            node.dataset.rawDefinition = node.textContent.trim();
        }
    });

    async function renderMermaidInContainer(containerId) {
        const container = document.getElementById(containerId);
        if (!container) return;
        const nodes = Array.from(container.querySelectorAll('.mermaid'));
        const pendingNodes = [];

        for (const node of nodes) {
            if (node.getAttribute('data-processed') === 'true' && node.querySelector('svg')) {
                continue;
            }
            if (node.dataset.rawDefinition) {
                node.removeAttribute('data-processed');
                node.textContent = node.dataset.rawDefinition;
                pendingNodes.push(node);
            }
        }

        if (pendingNodes.length > 0) {
            try {
                await mermaid.run({ nodes: pendingNodes });
            } catch (err) {
                console.error('Mermaid render error:', err);
                pendingNodes.forEach(node => {
                    node.innerHTML = `<pre style="color:#c0392b;text-align:left;white-space:pre-wrap;">流程圖渲染失敗：${err.message || err}</pre>`;
                });
            }
        }
    }

    function switchTab(targetId) {
        stopBasicAuto();
        stopAdvAuto();
        if (typeof window.stopCtrlAuto === 'function') {
            window.stopCtrlAuto();
        }

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

    function escapeHtml(str) {
        return str
            .replace(/&/g, '&amp;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;');
    }

    function renderCodeLines(containerId, lines, activeLineIndex) {
        const box = document.getElementById(containerId);
        if (!box) return;
        box.innerHTML = lines.map((line, idx) => {
            const lineNum = idx + 1;
            const isActive = idx === activeLineIndex;
            return `
                <div class="code-line ${isActive ? 'active-line' : ''}">
                    <span class="ln">${lineNum}</span>
                    <span class="pointer-arrow">▸</span>
                    <span class="code-text">${escapeHtml(line)}</span>
                </div>
            `;
        }).join('');
    }

    function highlightStageBanner(stage) {
        document.querySelectorAll('#basic-stage-banner .step-card').forEach(card => {
            card.classList.toggle('highlight-stage', card.dataset.stage === stage);
        });
    }

    // ==================== 1. 基礎 for 迴圈逐步除錯器 ====================
    const basicCodeLines = [
        "#include <iostream>",
        "using namespace std;",
        "",
        "int main() {",
        "    for (int i = 1; i <= 5; i++) {",
        "        cout << i << \" \";",
        "    }",
        "    return 0;",
        "}"
    ];

    const basicSteps = [
        { line: 4, stage: "init", i: "1", cond: "尚未判斷", condCls: "", stageText: "① 初始設定", out: "", desc: "執行 for 迴圈初始設定：宣告整數變數 i 並設為 1（僅執行一次）。" },
        { line: 4, stage: "cond", i: "1", cond: "1 <= 5 (true)", condCls: "true-val", stageText: "② 條件判斷", out: "", desc: "判斷 1 <= 5 是否成立：結果為 true，進入迴圈主體。" },
        { line: 5, stage: "body", i: "1", cond: "1 <= 5 (true)", condCls: "true-val", stageText: "③ 迴圈主體", out: "1 ", desc: "執行迴圈主體：輸出目前 i 的值（1）。" },
        { line: 4, stage: "update", i: "2", cond: "2 <= 5 (true)", condCls: "true-val", stageText: "④ 更新變數", out: "1 ", desc: "執行更新運算式 i++，i 變成 2，接著判斷 2 <= 5 成立（true）。" },
        { line: 5, stage: "body", i: "2", cond: "2 <= 5 (true)", condCls: "true-val", stageText: "③ 迴圈主體", out: "1 2 ", desc: "執行迴圈主體：輸出 2。" },
        { line: 4, stage: "update", i: "3", cond: "3 <= 5 (true)", condCls: "true-val", stageText: "④ 更新變數", out: "1 2 ", desc: "執行 i++，i 變成 3，接著判斷 3 <= 5 成立（true）。" },
        { line: 5, stage: "body", i: "3", cond: "3 <= 5 (true)", condCls: "true-val", stageText: "③ 迴圈主體", out: "1 2 3 ", desc: "執行迴圈主體：輸出 3。" },
        { line: 4, stage: "update", i: "4", cond: "4 <= 5 (true)", condCls: "true-val", stageText: "④ 更新變數", out: "1 2 3 ", desc: "執行 i++，i 變成 4，接著判斷 4 <= 5 成立（true）。" },
        { line: 5, stage: "body", i: "4", cond: "4 <= 5 (true)", condCls: "true-val", stageText: "③ 迴圈主體", out: "1 2 3 4 ", desc: "執行迴圈主體：輸出 4。" },
        { line: 4, stage: "update", i: "5", cond: "5 <= 5 (true)", condCls: "true-val", stageText: "④ 更新變數", out: "1 2 3 4 ", desc: "執行 i++，i 變成 5，接著判斷 5 <= 5 成立（true）。" },
        { line: 5, stage: "body", i: "5", cond: "5 <= 5 (true)", condCls: "true-val", stageText: "③ 迴圈主體", out: "1 2 3 4 5 ", desc: "執行迴圈主體：輸出 5。" },
        { line: 4, stage: "update", i: "6", cond: "6 <= 5 (false)", condCls: "false-val", stageText: "② 條件不成立", out: "1 2 3 4 5 ", desc: "執行 i++，i 變成 6；判斷 6 <= 5 為 false，跳出 for 迴圈！" },
        { line: 7, stage: "", i: "6", cond: "6 <= 5 (false)", condCls: "false-val", stageText: "迴圈已結束", out: "1 2 3 4 5 ", desc: "迴圈結束，準備執行 return 0 結束 main 函式。" }
    ];

    let basicIdx = -1;
    let basicTimer = null;
    const basicResetBtn = document.getElementById("basicResetBtn");
    const basicStepBtn = document.getElementById("basicStepBtn");
    const basicAutoBtn = document.getElementById("basicAutoBtn");

    function stopBasicAuto() {
        if (basicTimer) {
            clearInterval(basicTimer);
            basicTimer = null;
        }
        if (basicAutoBtn) {
            basicAutoBtn.textContent = "▶ 自動播放";
            basicAutoBtn.classList.remove("playing");
        }
    }

    function renderBasicState() {
        const total = basicSteps.length;
        const counterEl = document.getElementById("basic-step-counter");
        const iEl = document.getElementById("basic-var-i");
        const condEl = document.getElementById("basic-var-cond");
        const stageEl = document.getElementById("basic-var-stage");
        const descEl = document.getElementById("basic-explanation");
        const outEl = document.getElementById("basic-console-output");

        if (!counterEl) return;

        if (basicIdx < 0) {
            counterEl.textContent = `步驟：0 / ${total}`;
            renderCodeLines("basic-code-lines", basicCodeLines, -1);
            highlightStageBanner("");
            iEl.textContent = "未宣告";
            condEl.textContent = "-";
            condEl.className = "watch-val";
            stageEl.textContent = "準備開始";
            descEl.innerHTML = '點擊 <strong>「⏭ 下一步」</strong> 開始單步追蹤 C++ 程式如何執行 <code>for</code> 迴圈！';
            outEl.textContent = "";
            basicStepBtn.disabled = false;
            return;
        }

        const s = basicSteps[basicIdx];
        counterEl.textContent = `步驟：${basicIdx + 1} / ${total}`;
        renderCodeLines("basic-code-lines", basicCodeLines, s.line);
        highlightStageBanner(s.stage);
        iEl.textContent = s.i;
        condEl.textContent = s.cond;
        condEl.className = `watch-val ${s.condCls || ""}`;
        stageEl.textContent = s.stageText;
        descEl.innerHTML = s.desc;
        outEl.textContent = s.out;
        basicStepBtn.disabled = basicIdx >= total - 1;
        if (basicIdx >= total - 1) stopBasicAuto();
    }

    function stepBasicNext() {
        if (basicIdx < basicSteps.length - 1) {
            basicIdx++;
            renderBasicState();
        }
    }

    basicResetBtn.addEventListener("click", () => { stopBasicAuto(); basicIdx = -1; renderBasicState(); });
    basicStepBtn.addEventListener("click", () => { stopBasicAuto(); stepBasicNext(); });
    basicAutoBtn.addEventListener("click", () => {
        if (basicTimer) { stopBasicAuto(); return; }
        if (basicIdx >= basicSteps.length - 1) { basicIdx = -1; renderBasicState(); }
        basicAutoBtn.textContent = "⏸ 暫停播放";
        basicAutoBtn.classList.add("playing");
        basicTimer = setInterval(() => {
            if (basicIdx < basicSteps.length - 1) stepBasicNext();
            else stopBasicAuto();
        }, 850);
    });
    renderBasicState();

    // ==================== 2. 累加演算法逐步除錯器 ====================
    const advCodeLines = [
        "#include <iostream>",
        "using namespace std;",
        "",
        "int main() {",
        "    int N = 4, sum = 0;",
        "    for (int i = 1; i <= N; i++) {",
        "        sum = sum + i;",
        "    }",
        "    cout << \"Sum = \" << sum << endl;",
        "    return 0;",
        "}"
    ];

    let advN = 4;
    let advSteps = [];
    let advIdx = -1;
    let advTimer = null;

    const advNSelect = document.getElementById("adv-n-select");
    const advResetBtn = document.getElementById("advResetBtn");
    const advStepBtn = document.getElementById("advStepBtn");
    const advAutoBtn = document.getElementById("advAutoBtn");

    function buildAdvSteps(nVal) {
        const steps = [];
        let sum = 0;
        steps.push({ line: 4, iText: `- / ${nVal}`, sumText: "0", condText: "-", condCls: "", out: "", desc: `宣告變數 N = ${nVal}，並將累加器 sum 初始化為 0。` });
        for (let i = 1; i <= nVal; i++) {
            steps.push({
                line: 5,
                iText: `${i} / ${nVal}`,
                sumText: `${sum}`,
                condText: `${i} <= ${nVal} (true)`,
                condCls: "true-val",
                out: "",
                desc: i === 1
                    ? `初始設定 i = 1，並判斷 ${i} <= ${nVal} 成立（true），進入迴圈。`
                    : `更新 i 變為 ${i}，並判斷 ${i} <= ${nVal} 成立（true），進入迴圈。`
            });
            const prevSum = sum;
            sum += i;
            steps.push({
                line: 6,
                iText: `${i} / ${nVal}`,
                sumText: `${sum}`,
                condText: `${i} <= ${nVal} (true)`,
                condCls: "true-val",
                out: "",
                desc: `執行 sum = sum + i：${prevSum} + ${i} = ${sum}，sum 更新為 ${sum}。`
            });
        }
        const endI = nVal + 1;
        steps.push({
            line: 5,
            iText: `${endI} / ${nVal}`,
            sumText: `${sum}`,
            condText: `${endI} <= ${nVal} (false)`,
            condCls: "false-val",
            out: "",
            desc: `i 更新為 ${endI}，判斷 ${endI} <= ${nVal} 不成立（false），結束迴圈。`
        });
        steps.push({
            line: 8,
            iText: `${endI} / ${nVal}`,
            sumText: `${sum}`,
            condText: `${endI} <= ${nVal} (false)`,
            condCls: "false-val",
            out: `Sum = ${sum}`,
            desc: `輸出最終累加結果：Sum = ${sum}。`
        });
        return steps;
    }

    function stopAdvAuto() {
        if (advTimer) {
            clearInterval(advTimer);
            advTimer = null;
        }
        if (advAutoBtn) {
            advAutoBtn.textContent = "▶ 自動播放";
            advAutoBtn.classList.remove("playing");
        }
    }

    function renderAdvState() {
        const total = advSteps.length;
        const counterEl = document.getElementById("adv-step-counter");
        const iEl = document.getElementById("adv-var-i");
        const sumEl = document.getElementById("adv-var-sum");
        const condEl = document.getElementById("adv-var-cond");
        const descEl = document.getElementById("adv-explanation");
        const outEl = document.getElementById("adv-console-output");

        if (!counterEl) return;

        if (advIdx < 0) {
            counterEl.textContent = `步驟：0 / ${total}`;
            renderCodeLines("adv-code-lines", advCodeLines.map(l => l.replace("N = 4", `N = ${advN}`)), -1);
            iEl.textContent = `- / ${advN}`;
            sumEl.textContent = "未宣告";
            condEl.textContent = "-";
            condEl.className = "watch-val";
            descEl.innerHTML = '請按 <strong>「⏭ 下一步」</strong> 觀察 <code>sum</code> 如何每一輪把當下的 <code>i</code> 累加進去！';
            outEl.textContent = "";
            advStepBtn.disabled = false;
            return;
        }

        const s = advSteps[advIdx];
        counterEl.textContent = `步驟：${advIdx + 1} / ${total}`;
        renderCodeLines("adv-code-lines", advCodeLines.map(l => l.replace("N = 4", `N = ${advN}`)), s.line);
        iEl.textContent = s.iText;
        sumEl.textContent = s.sumText;
        condEl.textContent = s.condText;
        condEl.className = `watch-val ${s.condCls || ""}`;
        descEl.innerHTML = s.desc;
        outEl.textContent = s.out;
        advStepBtn.disabled = advIdx >= total - 1;
        if (advIdx >= total - 1) stopAdvAuto();
    }

    function stepAdvNext() {
        if (advIdx < advSteps.length - 1) {
            advIdx++;
            renderAdvState();
        }
    }

    advNSelect.addEventListener("change", (e) => {
        stopAdvAuto();
        advN = parseInt(e.target.value, 10);
        advSteps = buildAdvSteps(advN);
        advIdx = -1;
        renderAdvState();
    });
    advResetBtn.addEventListener("click", () => { stopAdvAuto(); advIdx = -1; renderAdvState(); });
    advStepBtn.addEventListener("click", () => { stopAdvAuto(); stepAdvNext(); });
    advAutoBtn.addEventListener("click", () => {
        if (advTimer) { stopAdvAuto(); return; }
        if (advIdx >= advSteps.length - 1) { advIdx = -1; renderAdvState(); }
        advAutoBtn.textContent = "⏸ 暫停播放";
        advAutoBtn.classList.add("playing");
        advTimer = setInterval(() => {
            if (advIdx < advSteps.length - 1) stepAdvNext();
            else stopAdvAuto();
        }, 850);
    });

    advSteps = buildAdvSteps(advN);
    renderAdvState();

    // ==================== 3. continue 與 break 逐步除錯器 ====================
    (function() {
        const ctrlCodeLines = [
            "#include <iostream>",
            "using namespace std;",
            "",
            "int main() {",
            "    for (int i = 1; i <= 5; i++) {",
            "        if (i == 4) break;",
            "        if (i == 2) continue;",
            "        cout << i << \" \";",
            "    }",
            "    cout << \"[Done]\";",
            "    return 0;",
            "}"
        ];

        const ctrlSteps = [
            { line: 4, i: "1", cond: "1 <= 5 (true)", condCls: "true-val", status: "① 初始化 i=1", statusCls: "", badge: "初始化", badgeCls: "", out: "", desc: "執行 for 迴圈初始設定：宣告並設定 i = 1。" },
            { line: 4, i: "1", cond: "1 <= 5 (true)", condCls: "true-val", status: "② 條件成立", statusCls: "true-val", badge: "進入迴圈", badgeCls: "", out: "", desc: "判斷 1 <= 5 成立（true），進入迴圈主體。" },
            { line: 5, i: "1", cond: "1 <= 5 (true)", condCls: "true-val", status: "檢查 i == 4", statusCls: "", badge: "條件檢查", badgeCls: "", out: "", desc: "判斷 i == 4：目前 i 為 1，不成立，繼續往下執行。" },
            { line: 6, i: "1", cond: "1 <= 5 (true)", condCls: "true-val", status: "檢查 i == 2", statusCls: "", badge: "條件檢查", badgeCls: "", out: "", desc: "判斷 i == 2：目前 i 為 1，不成立，繼續往下執行。" },
            { line: 7, i: "1", cond: "1 <= 5 (true)", condCls: "true-val", status: "③ 執行輸出", statusCls: "true-val", badge: "正常輸出", badgeCls: "", out: "1 ", desc: "執行 cout << i << \" \"，終端機印出 1。" },
            { line: 4, i: "2", cond: "2 <= 5 (true)", condCls: "true-val", status: "④ 更新 i++ (變為2)", statusCls: "", badge: "變數更新", badgeCls: "", out: "1 ", desc: "第一輪結束，執行 i++ 使 i 變成 2，再判斷 2 <= 5 成立。" },
            { line: 5, i: "2", cond: "2 <= 5 (true)", condCls: "true-val", status: "檢查 i == 4", statusCls: "", badge: "條件檢查", badgeCls: "", out: "1 ", desc: "判斷 i == 4：i 為 2，不成立。" },
            { line: 6, i: "2", cond: "2 <= 5 (true)", condCls: "true-val", status: "觸發 continue！", statusCls: "continue-val", badge: "continue 跳過", badgeCls: "is-continue", out: "1 ", desc: "i == 2 成立！執行 <code>continue</code>：跳過第 8 行的輸出，直接準備進入下一輪更新。" },
            { line: 4, i: "3", cond: "3 <= 5 (true)", condCls: "true-val", status: "④ 更新 i++ (變為3)", statusCls: "continue-val", badge: "變數更新", badgeCls: "", out: "1 ", desc: "因為剛才執行了 <code>continue</code>，直接跳回 for 標頭執行 i++（i 變為 3），並判斷 3 <= 5 成立。" },
            { line: 7, i: "3", cond: "3 <= 5 (true)", condCls: "true-val", status: "③ 執行輸出", statusCls: "true-val", badge: "正常輸出", badgeCls: "", out: "1 3 ", desc: "i = 3 未觸發 break 或 continue，執行 cout 印出 3。" },
            { line: 4, i: "4", cond: "4 <= 5 (true)", condCls: "true-val", status: "④ 更新 i++ (變為4)", statusCls: "", badge: "變數更新", badgeCls: "", out: "1 3 ", desc: "第三輪結束，執行 i++ 使 i 變成 4，判斷 4 <= 5 成立。" },
            { line: 5, i: "4", cond: "4 <= 5 (true)", condCls: "true-val", status: "觸發 break！", statusCls: "break-val", badge: "break 跳出", badgeCls: "is-break", out: "1 3 ", desc: "i == 4 成立！執行 <code>break</code>：立刻強制跳出整個 for 迴圈（第 7、8 行與 i=5 都不會執行）。" },
            { line: 9, i: "4", cond: "已強制跳出", condCls: "false-val", status: "離開迴圈", statusCls: "break-val", badge: "迴圈已結束", badgeCls: "is-break", out: "1 3 ", desc: "控制權來到 for 迴圈結束後的第一行（第 10 行）。" },
            { line: 9, i: "4", cond: "已強制跳出", condCls: "false-val", status: "輸出 [Done]", statusCls: "", badge: "迴圈外敘述", badgeCls: "", out: "1 3 [Done]", desc: "執行 cout << \"[Done]\"，終端機輸出 [Done]。" },
            { line: 10, i: "4", cond: "-", condCls: "", status: "程式結束", statusCls: "", badge: "完成", badgeCls: "", out: "1 3 [Done]", desc: "main 函式回傳 0，本次除錯追蹤完成！" }
        ];

        let ctrlIdx = -1;
        let ctrlTimer = null;

        const resetBtn = document.getElementById("ctrlResetBtn");
        const stepBtn = document.getElementById("ctrlStepBtn");
        const autoBtn = document.getElementById("ctrlAutoBtn");

        function stopCtrlAuto() {
            if (ctrlTimer) {
                clearInterval(ctrlTimer);
                ctrlTimer = null;
            }
            if (autoBtn) {
                autoBtn.textContent = "▶ 自動播放";
                autoBtn.classList.remove("playing");
            }
        }
        window.stopCtrlAuto = stopCtrlAuto;

        function renderCtrlState() {
            const total = ctrlSteps.length;
            const counterEl = document.getElementById("ctrl-step-counter");
            const iEl = document.getElementById("ctrl-var-i");
            const condEl = document.getElementById("ctrl-var-cond");
            const statusEl = document.getElementById("ctrl-var-status");
            const badgeEl = document.getElementById("ctrl-flow-badge");
            const descEl = document.getElementById("ctrl-explanation");
            const outEl = document.getElementById("ctrl-console-output");

            if (!counterEl) return;

            if (ctrlIdx < 0) {
                counterEl.textContent = `步驟：0 / ${total}`;
                renderCodeLines("ctrl-code-lines", ctrlCodeLines, -1);
                iEl.textContent = "未宣告";
                iEl.className = "watch-val";
                condEl.textContent = "-";
                condEl.className = "watch-val";
                statusEl.textContent = "準備開始";
                statusEl.className = "watch-val";
                badgeEl.textContent = "一般循環";
                badgeEl.className = "badge-flow";
                descEl.innerHTML = "點擊 <strong>「⏭ 下一步」</strong> 觀察 <code>i = 2</code> 遇到 <code>continue</code> 與 <code>i = 4</code> 遇到 <code>break</code> 時的跳轉路徑！";
                outEl.textContent = "";
                stepBtn.disabled = false;
                return;
            }

            const s = ctrlSteps[ctrlIdx];
            counterEl.textContent = `步驟：${ctrlIdx + 1} / ${total}`;
            renderCodeLines("ctrl-code-lines", ctrlCodeLines, s.line);

            iEl.textContent = s.i;
            iEl.className = "watch-val";

            condEl.textContent = s.cond;
            condEl.className = `watch-val ${s.condCls || ""}`;

            statusEl.textContent = s.status;
            statusEl.className = `watch-val ${s.statusCls || ""}`;

            badgeEl.textContent = s.badge;
            badgeEl.className = `badge-flow ${s.badgeCls || ""}`;

            descEl.innerHTML = s.desc;
            outEl.textContent = s.out;

            stepBtn.disabled = ctrlIdx >= total - 1;
            if (ctrlIdx >= total - 1) {
                stopCtrlAuto();
            }
        }

        function stepCtrlNext() {
            if (ctrlIdx < ctrlSteps.length - 1) {
                ctrlIdx++;
                renderCtrlState();
            }
        }

        if (resetBtn && stepBtn && autoBtn) {
            resetBtn.addEventListener("click", () => {
                stopCtrlAuto();
                ctrlIdx = -1;
                renderCtrlState();
            });

            stepBtn.addEventListener("click", () => {
                stopCtrlAuto();
                stepCtrlNext();
            });

            autoBtn.addEventListener("click", () => {
                if (ctrlTimer) {
                    stopCtrlAuto();
                    return;
                }
                if (ctrlIdx >= ctrlSteps.length - 1) {
                    ctrlIdx = -1;
                    renderCtrlState();
                }
                autoBtn.textContent = "⏸ 暫停播放";
                autoBtn.classList.add("playing");
                ctrlTimer = setInterval(() => {
                    if (ctrlIdx < ctrlSteps.length - 1) {
                        stepCtrlNext();
                    } else {
                        stopCtrlAuto();
                    }
                }, 900);
            });

            renderCtrlState();
        }
    })();

    // 初始載入時渲染預設 active 分頁的 Mermaid 流程圖
    renderMermaidInContainer("tab-cover");
</script>

</body>
</html>
