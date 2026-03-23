<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OMNI_DECODER_FINAL_99</title>
    <style>
        :root { --neon: #00ff41; --bg: #030303; --panel: #111; --danger: #ff003c; }
        body { background: var(--bg); color: var(--neon); font-family: 'Consolas', 'Courier New', monospace; margin: 0; padding: 20px; font-size: 14px; }
        .terminal { border: 2px solid var(--neon); box-shadow: 0 0 25px rgba(0, 255, 65, 0.2); max-width: 1200px; margin: auto; padding: 20px; background: #000; }
        
        /* Header & Input */
        header { text-align: center; border-bottom: 2px solid var(--neon); margin-bottom: 20px; padding-bottom: 10px; }
        textarea { 
            width: 100%; height: 150px; background: #080808; color: var(--neon); border: 1px solid var(--neon);
            padding: 15px; font-size: 1.1rem; resize: vertical; box-sizing: border-box; outline: none;
        }
        
        /* Grid Layout */
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; margin-bottom: 20px; }
        .module { border: 1px solid #004400; padding: 15px; background: var(--panel); position: relative; }
        .module h3 { margin: -15px -15px 15px -15px; background: #004400; color: #fff; padding: 5px 10px; font-size: 12px; text-transform: uppercase; }

        /* Controls */
        input[type="text"], input[type="number"] { background: #000; border: 1px solid var(--neon); color: var(--neon); padding: 5px; margin: 5px 0; width: calc(100% - 12px); }
        button { 
            width: 100%; background: transparent; color: var(--neon); border: 1px solid var(--neon); 
            padding: 10px; cursor: pointer; margin-top: 5px; font-weight: bold; transition: 0.2s;
        }
        button:hover { background: var(--neon); color: #000; box-shadow: 0 0 10px var(--neon); }
        .btn-danger { color: var(--danger); border-color: var(--danger); }
        .btn-danger:hover { background: var(--danger); color: #fff; box-shadow: 0 0 10px var(--danger); }

        /* Console Output */
        #console { 
            background: #000; border: 1px solid var(--neon); height: 300px; overflow-y: scroll;
            padding: 15px; font-size: 13px; line-height: 1.5; scroll-behavior: smooth;
        }
        .log-item { border-bottom: 1px solid #002200; padding: 10px 0; animation: fadeIn 0.3s; }
        .log-tag { color: #fff; background: #006622; padding: 2px 5px; border-radius: 3px; font-size: 10px; margin-right: 5px; }
        .highlight { color: #fff; font-weight: bold; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div class="terminal">
    <header>
        <h1 style="letter-spacing: 10px;">OMNI_DECODER_CORE_v4.0</h1>
        <p>RECOGNIZING PROTOCOLS... 99% GLOBAL COVERAGE ENABLED</p>
    </header>

    <textarea id="mainInput" placeholder="-- DROP YOUR ENCRYPTED PAYLOAD HERE --"></textarea>

    <div class="grid">
        <div class="module">
            <h3>Classical & Advanced Ciphers</h3>
            <button onclick="caesarBrute()">Caesar Brute (1-25)</button>
            <input type="text" id="vigenereKey" placeholder="Vigenere Key...">
            <button onclick="vigenere()">Vigenere Decode</button>
            <button onclick="atbash()">Atbash Cipher</button>
            <button onclick="rot47()">ROT47 (Full ASCII)</button>
        </div>

        <div class="module">
            <h3>Modern Encodings</h3>
            <button onclick="decodeBase64()">Base64 Decode</button>
            <button onclick="decodeHex()">Hex to ASCII</button>
            <button onclick="decodeBinary()">Binary to Text</button>
            <button onclick="decodeMorse()">Morse (.- to Text)</button>
        </div>

        <div class="module">
            <h3>Esoteric & ARG Utils</h3>
            <button onclick="detectZeroWidth()">Zero-Width Detector</button>
            <button onclick="reverseText()">Reverse Payload</button>
            <button onclick="freqAnalysis()">Frequency Analysis</button>
            <button onclick="a1z26()">A1Z26 (1-26 to A-Z)</button>
        </div>
    </div>

    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px;">
        <span>SYSTEM_LOG_OUTPUT:</span>
        <button onclick="clearLogs()" class="btn-danger" style="width: auto; padding: 2px 10px;">PURGE_LOGS</button>
    </div>
    <div id="console">_IDLE_</div>
</div>

<script>
    const input = document.getElementById('mainInput');
    const consoleBox = document.getElementById('console');

    function log(title, msg) {
        const item = document.createElement('div');
        item.className = 'log-item';
        item.innerHTML = `<span class="log-tag">${title}</span> <span class="highlight">${msg}</span>`;
        consoleBox.prepend(item);
    }

    // --- CAESAR BRUTE ---
    function caesarBrute() {
        let results = "";
        for (let s = 1; s <= 25; s++) {
            let res = input.value.replace(/[a-z]/gi, c => {
                let f = c <= 'Z' ? 65 : 97;
                return String.fromCharCode(((c.charCodeAt(0) - f - s + 26) % 26) + f);
            });
            results += `[S${s}] ${res}\n`;
        }
        log("CAESAR_BRUTE", `<pre>${results}</pre>`);
    }

    // --- VIGENERE ---
    function vigenere() {
        let key = document.getElementById('vigenereKey').value.toLowerCase();
        if (!key) return log("ERROR", "KEY REQUIRED FOR VIGENERE");
        let res = "", j = 0;
        for (let i = 0; i < input.value.length; i++) {
            let c = input.value[i];
            if (c.match(/[a-z]/i)) {
                let f = c <= 'Z' ? 65 : 97;
                let shift = key[j % key.length].charCodeAt(0) - 97;
                res += String.fromCharCode(((c.charCodeAt(0) - f - shift + 26) % 26) + f);
                j++;
            } else res += c;
        }
        log("VIGENERE", res);
    }

    // --- BASE64 ---
    function decodeBase64() {
        try { log("BASE64", atob(input.value)); }
        catch(e) { log("ERROR", "INVALID BASE64 FORMAT"); }
    }

    // --- ZERO WIDTH DETECTOR (ARG Special) ---
    function detectZeroWidth() {
        const zwChars = /[\u200B-\u200D\uFEFF]/g;
        const matches = input.value.match(zwChars);
        if (matches) {
            log("ZERO_WIDTH", `Found ${matches.length} hidden characters! Potential hidden Binary code detected.`);
        } else {
            log("ZERO_WIDTH", "No hidden characters found.");
        }
    }

    // --- BINARY ---
    function decodeBinary() {
        try {
            let res = input.value.split(/[ ,]+/).map(b => String.fromCharCode(parseInt(b, 2))).join('');
            log("BINARY", res);
        } catch(e) { log("ERROR", "INVALID BINARY"); }
    }

    // --- HEX ---
    function decodeHex() {
        try {
            let res = input.value.replace(/\s+/g, '').match(/.{1,2}/g).map(h => String.fromCharCode(parseInt(h, 16))).join('');
            log("HEX", res);
        } catch(e) { log("ERROR", "INVALID HEX"); }
    }

    // --- ROT47 ---
    function rot47() {
        let res = input.value.split('').map(c => {
            let code = c.charCodeAt(0);
            return (code >= 33 && code <= 126) ? String.fromCharCode(33 + ((code + 14) % 94)) : c;
        }).join('');
        log("ROT47", res);
    }

    // --- ATBASH ---
    function atbash() {
        let res = input.value.replace(/[a-z]/gi, c => {
            let f = c <= 'Z' ? [65, 90] : [97, 122];
            return String.fromCharCode(f[0] + f[1] - c.charCodeAt(0));
        });
        log("ATBASH", res);
    }

    // --- A1Z26 ---
    function a1z26() {
        let res = input.value.split(/[ ,.-]+/).map(n => {
            let num = parseInt(n);
            return (num >= 1 && num <= 26) ? String.fromCharCode(num + 64) : '?';
        }).join('');
        log("A1Z26", res);
    }

    function reverseText() { log("REVERSE", input.value.split('').reverse().join('')); }

    function freqAnalysis() {
        let f = {};
        input.value.toLowerCase().replace(/[^a-z]/g, "").split('').forEach(c => f[c] = (f[c] || 0) + 1);
        let res = Object.entries(f).sort((a,b)=>b[1]-a[1]).map(e=>`${e[0]}:${e[1]}`).join(' | ');
        log("FREQUENCY", res || "NO DATA");
    }

    function clearLogs() { consoleBox.innerHTML = "_SYSTEM_PURGED_"; }
</script>

</body>
</html>
