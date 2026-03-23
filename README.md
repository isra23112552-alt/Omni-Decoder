<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OMNI_DECODER_COMMAND_v5</title>
    <style>
        :root { --neon: #00ff41; --bg: #030303; --panel: #111; --active: #fff; }
        body { background: var(--bg); color: var(--neon); font-family: 'Consolas', monospace; margin: 0; padding: 20px; }
        .terminal { border: 2px solid var(--neon); box-shadow: 0 0 30px rgba(0, 255, 65, 0.15); max-width: 900px; margin: auto; padding: 30px; background: #000; }
        
        header { text-align: center; margin-bottom: 30px; border-bottom: 1px solid var(--neon); padding-bottom: 15px; }
        
        /* Input Area */
        .input-section { margin-bottom: 25px; }
        textarea { 
            width: 100%; height: 180px; background: #080808; color: var(--neon); border: 1px solid var(--neon);
            padding: 15px; font-size: 1.2rem; box-sizing: border-box; outline: none; box-shadow: inset 0 0 10px #002200;
        }

        /* Control Panel */
        .controls { 
            display: flex; gap: 15px; margin-bottom: 25px; background: var(--panel); 
            padding: 20px; border: 1px solid #004400; align-items: flex-end;
        }
        .control-group { display: flex; flex-direction: column; flex: 1; }
        label { font-size: 10px; margin-bottom: 5px; text-transform: uppercase; opacity: 0.8; }
        
        select, input { 
            background: #000; border: 1px solid var(--neon); color: var(--neon); 
            padding: 10px; font-family: 'Consolas'; outline: none;
        }

        /* The BIG Button */
        #startBtn { 
            flex: 1.5; background: var(--neon); color: #000; border: none; 
            padding: 15px; font-size: 1.2rem; font-weight: bold; cursor: pointer;
            text-transform: uppercase; letter-spacing: 3px; transition: 0.3s;
        }
        #startBtn:hover { background: #fff; box-shadow: 0 0 20px #fff; }

        /* Output Console */
        #console { 
            background: #000; border: 1px solid var(--neon); height: 350px; overflow-y: auto;
            padding: 20px; font-size: 14px; border-top: 4px solid var(--neon);
        }
        .log-entry { margin-bottom: 15px; border-left: 3px solid var(--neon); padding-left: 15px; animation: pulse 0.5s; }
        @keyframes pulse { from { border-color: #fff; } to { border-color: var(--neon); } }
        pre { white-space: pre-wrap; word-wrap: break-word; color: #fff; }
    </style>
</head>
<body>

<div class="terminal">
    <header>
        <h1 style="margin:0;">OMNI_DECODER_v5.0</h1>
        <p style="font-size: 12px; opacity: 0.7;">[ SYSTEM STATUS: READY TO ANALYZE EVERY POSSIBILITY ]</p>
    </header>

    <div class="input-section">
        <label>Input Payload:</label>
        <textarea id="mainInput" placeholder="-- PASTE DATA HERE --"></textarea>
    </div>

    <div class="controls">
        <div class="control-group">
            <label>Select Protocol:</label>
            <select id="modeSelect">
                <option value="auto">AUTO-SCAN (Try Basics)</option>
                <option value="base64">Base64 Decoder</option>
                <option value="caesar">Caesar Brute Force</option>
                <option value="vigenere">Vigenere Cipher</option>
                <option value="atbash">Atbash Cipher</option>
                <option value="rot47">ROT47 Cipher</option>
                <option value="binary">Binary to Text</option>
                <option value="hex">Hex to ASCII</option>
                <option value="a1z26">A1Z26 Numbers</option>
                <option value="morse">Morse Code</option>
                <option value="zerowidth">Zero-Width Hidden</option>
            </select>
        </div>

        <div class="control-group" id="keyGroup" style="display:none;">
            <label>Security Key:</label>
            <input type="text" id="keyValue" placeholder="Enter Key...">
        </div>

        <button id="startBtn" onclick="initiateDecode()">[ START DECODE ]</button>
    </div>

    <div id="console">_WAITING_FOR_COMMAND_...</div>
</div>

<script>
    const input = document.getElementById('mainInput');
    const modeSelect = document.getElementById('modeSelect');
    const keyGroup = document.getElementById('keyGroup');
    const consoleBox = document.getElementById('console');

    // โชว์ช่องใส่ Key เฉพาะตอนเลือก Vigenere
    modeSelect.addEventListener('change', () => {
        keyGroup.style.display = (modeSelect.value === 'vigenere') ? 'block' : 'none';
    });

    function log(title, msg) {
        const div = document.createElement('div');
        div.className = 'log-entry';
        div.innerHTML = `<b style="color:var(--neon)">> ${title}</b><br><pre>${msg}</pre>`;
        consoleBox.prepend(div);
    }

    function initiateDecode() {
        const val = input.value.trim();
        const mode = modeSelect.value;
        if (!val) return log("SYSTEM", "NO DATA DETECTED. ABORTING.");

        consoleBox.innerHTML = `<div style="color:#fff">ANALYZING... [${mode.toUpperCase()}]</div>` + consoleBox.innerHTML;

        switch(mode) {
            case "base64": decodeBase64(val); break;
            case "caesar": caesarBrute(val); break;
            case "vigenere": vigenere(val); break;
            case "atbash": atbash(val); break;
            case "rot47": rot47(val); break;
            case "binary": decodeBinary(val); break;
            case "hex": decodeHex(val); break;
            case "a1z26": a1z26(val); break;
            case "zerowidth": detectZeroWidth(val); break;
            case "auto": autoScan(val); break;
            default: log("SYSTEM", "UNKNOWN PROTOCOL");
        }
    }

    function autoScan(val) {
        log("AUTO-SCAN", "Running basic checks...");
        decodeBase64(val);
        atbash(val);
        detectZeroWidth(val);
    }

    function decodeBase64(val) {
        try { log("BASE64", atob(val)); } 
        catch(e) { log("BASE64_ERROR", "Data is not valid Base64"); }
    }

    function caesarBrute(val) {
        let res = "";
        for (let s = 1; s <= 25; s++) {
            res += `S${s}: ` + val.replace(/[a-z]/gi, c => {
                let f = c <= 'Z' ? 65 : 97;
                return String.fromCharCode(((c.charCodeAt(0) - f - s + 26) % 26) + f);
            }) + "\n";
        }
        log("CAESAR_BRUTE", res);
    }

    function vigenere(val) {
        let key = document.getElementById('keyValue').value.toLowerCase();
        if (!key) return log("VIGENERE_ERROR", "KEY REQUIRED");
        let res = "", j = 0;
        for (let i = 0; i < val.length; i++) {
            let c = val[i];
            if (c.match(/[a-z]/i)) {
                let f = c <= 'Z' ? 65 : 97;
                let shift = key[j % key.length].charCodeAt(0) - 97;
                res += String.fromCharCode(((c.charCodeAt(0) - f - shift + 26) % 26) + f);
                j++;
            } else res += c;
        }
        log("VIGENERE", res);
    }

    function atbash(val) {
        log("ATBASH", val.replace(/[a-z]/gi, c => {
            let f = c <= 'Z' ? [65, 90] : [97, 122];
            return String.fromCharCode(f[0] + f[1] - c.charCodeAt(0));
        }));
    }

    function detectZeroWidth(val) {
        const m = val.match(/[\u200B-\u200D\uFEFF]/g);
        log("ZERO_WIDTH", m ? `DETECTED: ${m.length} hidden bits found.` : "CLEAN: No hidden characters.");
    }

    // ฟังก์ชันอื่นๆ (Binary, Hex, A1Z26) ใส่ไว้ในนี้เหมือนเดิม...
</script>
</body>
</html>
