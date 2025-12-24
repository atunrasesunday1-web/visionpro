<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Vision Pro Premium</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700&display=swap" rel="stylesheet">

    <style>
        :root { --accent: #00d4ff; --bg: #0b0b0b; --panel: rgba(22, 22, 22, 0.7); --btn: #1f1f1f; --red: #ff3b30; --green: #34c759; }
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; font-family: 'Inter', sans-serif; outline: none; }
        body { background: var(--bg); color: white; margin: 0; overflow: hidden; height: 100vh; width: 100vw; }

        /* SPLASH SCREEN */
        #splash-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--bg); z-index: 9999; display: flex; align-items: center; justify-content: center;
            transition: opacity 0.8s ease, visibility 0.8s;
        }
        .v-logo-svg { width: 280px; height: 280px; }
        .signature-path {
            fill: none; stroke: var(--accent); stroke-width: 2.5;
            stroke-dasharray: 5000; stroke-dashoffset: 5000;
            animation: drawSignature 2.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
        }
        @keyframes drawSignature {
            0% { stroke-dashoffset: 5000; fill: rgba(0, 212, 255, 0); }
            65% { stroke-dashoffset: 0; fill: rgba(0, 212, 255, 0); }
            100% { stroke-dashoffset: 0; fill: rgba(0, 212, 255, 1); filter: drop-shadow(0 0 15px var(--accent)); }
        }
        .fade-out { opacity: 0; visibility: hidden; }

        /* UI STRUCTURE */
        .app-container { display: flex; flex-direction: column; height: 100vh; opacity: 0; transition: 1s; padding-top: env(safe-area-inset-top); }
        .app-ready { opacity: 1; }

        .header-container { padding: 20px 20px 10px; background: rgba(11, 11, 11, 0.8); backdrop-filter: blur(20px); z-index: 10; }
        .navigation { background: var(--panel); padding: 4px; border-radius: 18px; display: flex; position: relative; border: 1px solid rgba(255,255,255,0.05); }
        .nav-btn { flex: 1; padding: 14px; border: none; background: none; color: #555; font-size: 10px; font-weight: 800; letter-spacing: 1.5px; z-index: 2; }
        .nav-btn.active { color: #fff; }
        .nav-pill { position: absolute; width: calc(50% - 4px); height: calc(100% - 8px); background: #282828; border-radius: 14px; top: 4px; left: 4px; transition: transform 0.4s cubic-bezier(0.2, 1, 0.3, 1); z-index: 1; }
        
        .main-stage { display: flex; width: 200%; flex: 1; transition: transform 0.5s cubic-bezier(0.2, 1, 0.3, 1); }
        .room { width: 50%; height: 100%; display: flex; flex-direction: column; padding: 25px; }
        
        .display { flex: 1; display: flex; flex-direction: column; justify-content: flex-end; text-align: right; padding-bottom: 20px; }
        .screen { font-size: 58px; font-weight: 300; color: #fff; letter-spacing: -2px; overflow: hidden; }
        .sub-screen { font-size: 14px; color: var(--accent); opacity: 0.7; margin-bottom: 5px; display: flex; justify-content: flex-end; align-items: center; gap: 8px; }
        
        .live-dot { width: 7px; height: 7px; background: var(--green); border-radius: 50%; box-shadow: 0 0 10px var(--green); }

        .grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; flex: 2.8; }
        button { border-radius: 20px; border: none; background: var(--btn); color: #fff; font-size: 22px; transition: 0.2s; display: flex; align-items: center; justify-content: center; }
        button:active { transform: scale(0.9); background: #333; }
        .op { color: var(--accent); background: rgba(0, 212, 255, 0.08); }
        .action { background: var(--accent); color: #000; font-weight: 700; }
        
        .mic-btn.listening { background: var(--red) !important; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(255, 59, 48, 0.6); } 70% { box-shadow: 0 0 0 20px rgba(255, 59, 48, 0); } 100% { box-shadow: 0 0 0 0 rgba(255, 59, 48, 0); } }

        .currency-switch { display: flex; gap: 10px; margin-bottom: 15px; }
        .c-tab { flex: 1; padding: 14px; background: var(--btn); border-radius: 14px; font-size: 11px; text-align: center; color: #666; font-weight: 700; }
        .c-tab.active { background: var(--accent); color: #000; }
    </style>
</head>
<body>

    <div id="splash-overlay">
        <svg class="v-logo-svg" viewBox="0 0 512 512">
            <path class="signature-path" d="M197.97 134.771C191.02 134.771 185.095 136.593 180.196 140.239C175.297 143.771 171.253 148.898 168.062 155.62C164.872 162.228 162.537 170.317 161.056 179.888C159.575 189.344 158.834 199.94 158.834 211.675C158.834 222.612 159.461 234.062 160.714 246.025C161.967 257.988 163.847 270.122 166.354 282.427C168.746 294.618 171.765 306.694 175.411 318.657C178.943 330.62 182.988 342.013 187.545 352.837C192.102 363.66 197.172 373.743 202.755 383.086C208.338 392.314 214.376 400.347 220.87 407.183C227.364 413.905 234.314 419.146 241.72 422.905C249.011 426.779 256.702 428.716 264.791 428.716C273.336 428.716 280.742 427.178 287.008 424.102C293.16 421.139 298.401 417.095 302.73 411.968C306.946 406.727 310.364 400.745 312.984 394.023C315.605 387.188 317.656 380.01 319.137 372.49C320.504 364.971 321.415 357.337 321.871 349.59C322.327 341.729 322.555 334.266 322.555 327.202C322.555 313.644 321.985 300.485 320.846 287.725C319.706 274.85 318.453 262.432 317.086 250.469C315.605 238.506 314.295 227.056 313.155 216.118C312.016 205.181 311.446 194.756 311.446 184.844C311.446 176.527 311.902 169.634 312.813 164.165C313.725 158.582 315.149 154.139 317.086 150.835C318.909 147.531 321.244 145.195 324.093 143.828C326.941 142.461 330.302 141.777 334.176 141.777C339.189 141.777 344.145 142.917 349.044 145.195C353.943 147.474 358.671 150.664 363.229 154.766C367.672 158.753 371.773 163.538 375.533 169.121C379.293 174.59 382.54 180.571 385.274 187.065C388.123 193.56 390.344 200.396 391.939 207.573C393.535 214.637 394.332 221.758 394.332 228.936C394.332 236.341 393.819 242.55 392.794 247.563C391.769 252.576 390.344 256.621 388.521 259.697C386.812 262.773 384.705 264.995 382.198 266.362C379.578 267.729 376.729 268.413 373.653 268.413C368.299 268.413 363.456 266.989 359.127 264.141C354.798 261.178 351.038 257.077 347.848 251.836C344.658 246.481 342.037 240.101 339.986 232.695C337.936 225.176 336.454 216.916 335.543 207.915L327.511 208.257C327.853 212.131 328.479 216.574 329.391 221.587C330.188 226.486 331.384 231.499 332.979 236.626C334.575 241.639 336.568 246.595 338.961 251.494C341.354 256.393 344.316 260.723 347.848 264.482C351.38 268.242 355.481 271.318 360.152 273.711C364.824 275.99 370.235 277.129 376.388 277.129C380.831 277.129 385.445 276.274 390.23 274.565C395.016 272.856 399.402 270.179 403.39 266.533C407.377 262.887 410.624 258.159 413.131 252.349C415.751 246.424 417.062 239.303 417.062 230.986C417.062 222.783 415.865 214.694 413.473 206.719C411.194 198.743 408.004 191.11 403.902 183.818C399.915 176.527 395.13 169.805 389.547 163.652C384.078 157.5 378.097 152.145 371.603 147.588C365.108 143.031 358.272 139.499 351.095 136.992C343.917 134.372 336.739 133.062 329.562 133.062C299.028 133.062 283.761 155.221 283.761 199.541C283.761 210.365 284.273 221.872 285.299 234.062C286.21 246.253 287.236 258.729 288.375 271.489C289.4 284.136 290.426 296.839 291.451 309.6C292.363 322.246 292.818 334.437 292.818 346.172C292.818 361.439 292.135 373.8 290.768 383.257C289.4 392.713 287.407 400.062 284.786 405.303C282.166 410.43 278.976 413.905 275.216 415.728C271.456 417.437 267.184 418.291 262.398 418.291C255.221 418.291 248.157 415.671 241.207 410.43C234.257 405.075 227.649 397.897 221.383 388.896C215.117 379.782 209.363 369.186 204.122 357.109C198.767 344.919 194.153 331.987 190.279 318.315C186.406 304.53 183.386 290.345 181.222 275.762C179.057 261.064 177.975 246.709 177.975 232.695C177.975 216.859 178.829 203.7 180.538 193.218C182.247 182.622 184.583 174.191 187.545 167.925C190.507 161.545 193.925 157.044 197.799 154.424C201.673 151.803 205.831 150.493 210.274 150.493C213.351 150.493 216.313 151.063 219.161 152.202C221.896 153.228 224.459 154.538 226.852 156.133C229.244 157.728 231.409 159.437 233.346 161.26C235.283 163.083 236.878 164.735 238.131 166.216L245.138 160.918C243.543 158.867 241.207 156.361 238.131 153.398C234.941 150.436 231.295 147.588 227.193 144.854C222.978 142.119 218.421 139.784 213.521 137.847C208.508 135.796 203.325 134.771 197.97 134.771Z" />
        </svg>
    </div>

    <div class="app-container" id="app-ui">
        <div class="header-container">
            <div class="navigation">
                <div id="pill" class="nav-pill"></div>
                <button class="nav-btn active" onclick="goto(0, this)">CORE CALC</button>
                <button class="nav-btn" onclick="goto(1, this)">CONVERTER</button>
            </div>
        </div>

        <div class="main-stage" id="stage">
            <div class="room">
                <div class="display">
                    <div class="sub-screen" id="c-sub"></div>
                    <div class="screen" id="c-scr">0</div>
                </div>
                <div class="grid">
                    <button class="op" onclick="cClr()">AC</button>
                    <button class="op" onclick="cIn('/')">÷</button>
                    <button class="op" onclick="cIn('*')">×</button>
                    <button class="op" onclick="cBack()">⌫</button>
                    <button onclick="cIn('7')">7</button><button onclick="cIn('8')">8</button><button onclick="cIn('9')">9</button><button class="op" onclick="cIn('-')">−</button>
                    <button onclick="cIn('4')">4</button><button onclick="cIn('5')">5</button><button onclick="cIn('6')">6</button><button class="op" onclick="cIn('+')">+</button>
                    <button onclick="cIn('1')">1</button><button onclick="cIn('2')">2</button><button onclick="cIn('3')">3</button><button class="action" onclick="cSolve()">=</button>
                    <button class="op mic-btn" id="mic-trigger" onclick="startVoice()">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="var(--accent)"><path d="M12 14c1.66 0 3-1.34 3-3V5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3z"/><path d="M17 11c0 2.76-2.24 5-5 5s-5-2.24-5-5H5c0 3.53 2.61 6.43 6 6.92V21h2v-3.08c3.39-.49 6-3.39 6-6.92h-2z"/></svg>
                    </button>
                    <button onclick="cIn('0')">0</button><button onclick="cIn('.')">.</button>
                    <button class="op" onclick="goto(1, document.querySelectorAll('.nav-btn')[1])">₦</button>
                </div>
            </div>

            <div class="room">
                <div class="display">
                    <div class="sub-screen" id="curr-label">
                        <span id="update-status">SYNCING...</span>
                        <div class="live-dot" id="live-dot" style="opacity: 0;"></div>
                    </div>
                    <div class="screen" id="f-scr">₦0</div>
                </div>
                <div class="currency-switch">
                    <div class="c-tab active" id="tab-USD" onclick="setCurr('USD')">USD</div>
                    <div class="c-tab" id="tab-GBP" onclick="setCurr('GBP')">GBP</div>
                    <div class="c-tab" id="tab-EUR" onclick="setCurr('EUR')">EUR</div>
                </div>
                <div class="grid">
                    <button class="op" onclick="fClr()">AC</button>
                    <button style="grid-column: span 2; font-size: 11px; color: var(--accent);">MARKET AMOUNT</button>
                    <button class="op" onclick="fBack()">⌫</button>
                    <button onclick="fIn('7')">7</button><button onclick="fIn('8')">8</button><button onclick="fIn('9')">9</button><button class="op" onclick="fIn('4')">4</button>
                    <button onclick="fIn('5')">5</button><button onclick="fIn('6')">6</button><button onclick="fIn('1')">1</button><button class="op" onclick="fIn('2')">2</button>
                    <button onclick="fIn('3')">3</button><button onclick="fIn('0')">0</button><button onclick="fIn('.')">.</button>
                    <button class="action" onclick="goto(0, document.querySelectorAll('.nav-btn')[0])">DONE</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        let rates = { USD: 1685, GBP: 2160, EUR: 1840 };
        let activeCurr = 'USD';

        async function fetchRates() {
            try {
                const res = await fetch('https://open.er-api.com/v6/latest/USD');
                const data = await res.json();
                rates.USD = data.rates.NGN;
                rates.GBP = rates.USD / data.rates.GBP;
                rates.EUR = rates.USD / data.rates.EUR;
                document.getElementById('update-status').innerText = "LIVE MARKET";
                document.getElementById('live-dot').style.opacity = "1";
            } catch (e) {
                document.getElementById('update-status').innerText = "OFFLINE MODE";
            }
        }

        window.onload = () => { fetchRates(); setTimeout(() => {
            document.getElementById('splash-overlay').classList.add('fade-out');
            document.getElementById('app-ui').classList.add('app-ready');
        }, 3200); };

        function goto(idx, btn) {
            document.getElementById('stage').style.transform = `translateX(-${idx * 50}%)`;
            document.getElementById('pill').style.transform = `translateX(${idx * 100}%)`;
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        let cData = "";
        function cIn(v) { cData += v; cUp(); }
        function cClr() { cData = ""; cUp(); document.getElementById('c-sub').innerText = ""; }
        function cBack() { cData = cData.slice(0,-1); cUp(); }
        function cUp() { document.getElementById('c-scr').innerText = cData || "0"; }
        function cSolve() { try { 
            let res = eval(cData.replace('×','*').replace('÷','/'));
            document.getElementById('c-sub').innerText = cData + " =";
            cData = Number.isInteger(res) ? res.toString() : res.toFixed(2);
            cUp(); 
        } catch(e){ cData = "Error"; cUp(); } }

        let fData = "";
        function setCurr(name) {
            activeCurr = name;
            document.querySelectorAll('.c-tab').forEach(t => t.classList.remove('active'));
            document.getElementById('tab-' + name).classList.add('active');
            fUp();
        }
        function fIn(v) { fData += v; fUp(); }
        function fClr() { fData = ""; fUp(); }
        function fBack() { fData = fData.slice(0,-1); fUp(); }
        function fUp() { 
            let val = parseFloat(fData) || 0;
            document.getElementById('f-scr').innerText = "₦" + (val * rates[activeCurr]).toLocaleString(undefined, {maximumFractionDigits:0}); 
        }

        function startVoice() {
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            if (!SpeechRecognition) return;
            const rec = new SpeechRecognition();
            const btn = document.getElementById('mic-trigger');
            rec.onstart = () => btn.classList.add('listening');
            rec.onend = () => btn.classList.remove('listening');
            rec.onresult = (e) => {
                let txt = e.results[0][0].transcript.toLowerCase();
                const map = {'one':'1','two':'2','three':'3','four':'4','five':'5','six':'6','seven':'7','eight':'8','nine':'9','zero':'0','plus':'+','minus':'-','times':'*','multiply':'*','divide':'/'};
                let res = "";
                txt.split(/[\s-]+/).forEach(w => {
                    if(map[w]) res += map[w];
                    else if(!isNaN(w)) res += w;
                });
                cData += res; cUp();
            };
            rec.start();
        }
    </script>
</body>
</html>
