<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Physical-Style Scientific Calculator — Fixed</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Roboto+Mono:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bezel:#b9bec2;
      --panel:#17191b;
      --lcd:#081216;
      --num:#e6e9eb;
      --fn:#f1e6c9;
      --op:#ffd8a8;
      --eq:#ff9a2e;
      --shadow: rgba(2,6,10,0.6);
      --text-dark: #0b0b0b;
      --text-light:#f7fbfd;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial;background:linear-gradient(180deg,#0b1220,#131a21);color:var(--text-light);display:flex;align-items:center;justify-content:center;padding:20px}

    .shell{width:420px;max-width:96vw;border-radius:16px;padding:16px;background:linear-gradient(180deg,var(--bezel),#9aa3a6);box-shadow:0 18px 50px var(--shadow), inset 0 2px 0 rgba(255,255,255,0.18)}
    .body{background:linear-gradient(180deg,var(--panel),#0f1112);border-radius:10px;padding:14px;border:3px solid rgba(0,0,0,0.45);box-shadow:inset 0 -6px 20px rgba(0,0,0,0.6)}

    .top{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px}
    .brand{font-weight:700;letter-spacing:1px;color:#dde9ef}
    .solar{width:76px;height:22px;border-radius:4px;background:linear-gradient(90deg,#9ad3ff,#4fb3ff);box-shadow:inset 0 -2px 6px rgba(0,0,0,0.25)}

    .display{background:linear-gradient(180deg,var(--lcd),#0b0f12);border-radius:8px;padding:10px 12px;margin-bottom:12px;border:1px solid rgba(255,255,255,0.03);position:relative;overflow:hidden}
    .display:after{content:'';position:absolute;left:0;right:0;top:0;height:40%;background:linear-gradient(180deg,rgba(255,255,255,0.06),transparent);pointer-events:none}
    .small-line{font-family:'Roboto Mono',monospace;color:#9fdff8;font-size:13px;text-align:right;opacity:0.9;min-height:18px}
    .big-line{font-family:'Roboto Mono',monospace;color:#e8ffff;font-size:36px;font-weight:700;text-align:right;min-height:46px}
    .status{display:flex;gap:8px;align-items:center;margin-top:8px}
    .status .dot{width:8px;height:8px;border-radius:50%;background:#3fe27a;box-shadow:0 0 8px #3fe27a66}
    .mode-label{font-size:12px;color:#b6c9cf;margin-left:auto}

    .mem{display:flex;gap:8px;margin-bottom:10px}
    .mem button{background:linear-gradient(180deg,#2a2f31,#141616);border:1px solid rgba(255,255,255,0.03);color:#cfe8f8;padding:8px 10px;border-radius:6px;font-weight:700;cursor:pointer}

    .keys{display:grid;grid-template-columns:repeat(5,1fr);gap:10px}
    button.key{border:0;padding:12px;border-radius:8px;font-weight:700;font-size:15px;cursor:pointer;box-shadow:0 8px 0 rgba(0,0,0,0.45);transition:transform 90ms linear, box-shadow 90ms linear}
    button.key:active{transform:translateY(6px);box-shadow:0 2px 0 rgba(0,0,0,0.6)}

    .num{background:linear-gradient(180deg,var(--num),#d0d4d6);color:var(--text-dark)}
    .fn{background:linear-gradient(180deg,var(--fn),#e8ddba);color:var(--text-dark)}
    .op{background:linear-gradient(180deg,var(--op),#ffcc88);color:var(--text-dark)}
    .eq{background:linear-gradient(180deg,var(--eq),#ff7b1a);color:var(--text-dark)}

    .wide{grid-column:1 / 3}
    .footer{margin-top:12px;text-align:center;font-size:12px;color:#b8c4c8}

    @media (max-width:420px){.shell{width:100%}.big-line{font-size:28px}}
  </style>
</head>
<body>
  <div class="shell" role="application" aria-label="Scientific calculator">
    <div class="body">
      <div class="top">
        <div class="brand">SCIENTIFIC — PRO</div>
        <div class="solar" aria-hidden="true"></div>
      </div>

      <div class="display" aria-live="polite">
        <div id="small" class="small-line">&nbsp;</div>
        <div id="big" class="big-line">0</div>
        <div class="status"><div class="dot" aria-hidden="true"></div><div class="mode-label" id="modeLabel">RAD</div></div>
      </div>

      <div class="mem" role="group" aria-label="Memory controls">
        <button id="mPlus">M+</button>
        <button id="mMinus">M-</button>
        <button id="mR">MR</button>
        <button id="mC">MC</button>
        <div style="margin-left:auto;display:flex;gap:8px;align-items:center">
          <label style="font-size:13px;color:#cfe8f8">Deg</label>
          <input type="checkbox" id="degToggle" aria-label="Toggle degrees" />
        </div>
      </div>

      <div class="keys" role="group" aria-label="Calculator keys">
        <button class="key fn" data-val="AC">AC</button>
        <button class="key fn" data-val="DEL">DEL</button>
        <button class="key fn" data-val="(">(</button>
        <button class="key fn" data-val=")">)</button>
        <button class="key op" data-val="%">%</button>

        <button class="key num" data-val="7">7</button>
        <button class="key num" data-val="8">8</button>
        <button class="key num" data-val="9">9</button>
        <button class="key op" data-val="/">÷</button>
        <button class="key fn" data-val="sqrt(">√</button>

        <button class="key num" data-val="4">4</button>
        <button class="key num" data-val="5">5</button>
        <button class="key num" data-val="6">6</button>
        <button class="key op" data-val="*">×</button>
        <button class="key fn" data-val="^">xʸ</button>

        <button class="key num" data-val="1">1</button>
        <button class="key num" data-val="2">2</button>
        <button class="key num" data-val="3">3</button>
        <button class="key op" data-val="-">−</button>
        <button class="key fn" data-val="!">n!</button>

        <button class="key wide num" data-val="00">00</button>
        <button class="key num" data-val="0">0</button>
        <button class="key num" data-val=".">.</button>
        <button class="key eq" data-val="=">=</button>

        <button class="key fn" data-val="sin(">sin</button>
        <button class="key fn" data-val="cos(">cos</button>
        <button class="key fn" data-val="tan(">tan</button>
        <button class="key fn" data-val="ln(">ln</button>
        <button class="key fn" data-val="log(">log</button>

        <button class="key fn" data-val="abs(">abs</button>
        <button class="key fn" data-val="pow(">pow</button>
        <button class="key fn" data-val="PI">π</button>
        <button class="key fn" data-val="E">e</button>
        <button class="key fn" data-val=","> , </button>
      </div>

      <div class="footer">Keyboard: Enter = evaluate · Backspace = DEL · Esc = AC</div>
    </div>
  </div>

  <script>
    (function(){
      const small = document.getElementById('small');
      const big = document.getElementById('big');
      const keys = document.querySelectorAll('button.key');
      const degToggle = document.getElementById('degToggle');
      const modeLabel = document.getElementById('modeLabel');
      const mPlus = document.getElementById('mPlus');
      const mMinus = document.getElementById('mMinus');
      const mR = document.getElementById('mR');
      const mC = document.getElementById('mC');

      let expr = '';
      let lastAns = '';
      let memory = 0;
      const settings = { deg: false };

      function fact(n){ n = Number(n); if(!Number.isFinite(n) || n < 0 || Math.floor(n) !== n) throw 'Factorial only for non-negative integers'; let r = 1; for(let i=2;i<=n;i++) r *= i; return r; }
      function sin(x){ return settings.deg ? Math.sin(x*Math.PI/180) : Math.sin(x); }
      function cos(x){ return settings.deg ? Math.cos(x*Math.PI/180) : Math.cos(x); }
      function tan(x){ return settings.deg ? Math.tan(x*Math.PI/180) : Math.tan(x); }
      function asin(x){ return settings.deg ? Math.asin(x)*180/Math.PI : Math.asin(x); }
      function acos(x){ return settings.deg ? Math.acos(x)*180/Math.PI : Math.acos(x); }
      function atan(x){ return settings.deg ? Math.atan(x)*180/Math.PI : Math.atan(x); }
      function ln(x){ return Math.log(x); }
      function log10(x){ return Math.log10 ? Math.log10(x) : Math.log(x)/Math.LN10; }
      function pow(a,b){ return Math.pow(a,b); }
      function sqrt(x){ return Math.sqrt(x); }
      function abs(x){ return Math.abs(x); }

      function preprocess(s){
        if(!s) return '';
        s = String(s);
        s = s.replace(/÷/g, '/').replace(/×/g, '*').replace(/−/g, '-').replace(/π/g, 'PI');
        s = s.replace(/(\d+(?:\.\d+)?|\([^()]*\))%/g, '($1/100)');
        s = s.replace(/(\d+(?:\.\d+)?|\([^()]*\))!/g, 'fact($1)');
        s = s.replace(/\^/g, '**');
        s = s.replace(/\blog\(/gi, 'log10(');
        s = s.replace(/\bANS\b/gi, '(' + (lastAns || '0') + ')');
        return s;
      }

      function isSafe(s){
        return /^[0-9+\-*/%^().,!\\sA-Za-zPIE]+$/.test(s);
      }

      function evaluate(inputStr){
        if(!inputStr || inputStr.trim() === '') return '';
        const exprJS = preprocess(inputStr);
        if(!isSafe(exprJS)) throw 'Invalid characters in expression';
        const fn = new Function('Math','fact','sin','cos','tan','asin','acos','atan','ln','log10','pow','sqrt','abs','PI','E', 'return (' + exprJS + ');');
        const val = fn(Math, fact, sin, cos, tan, asin, acos, atan, ln, log10, pow, sqrt, abs, Math.PI, Math.E);
        if(val === undefined) throw 'Invalid expression';
        if(Number.isFinite(val)) {
          const rounded = Math.round((val + Number.EPSILON) * 1e12) / 1e12;
          lastAns = String(rounded);
          return rounded;
        }
        return val;
      }

      function refresh(){
        small.textContent = expr || '\u00A0';
        try{
          const v = evaluate(expr);
          big.textContent = (v === '' ? '0' : v);
        }catch(e){
          big.textContent = 'ERR';
          // show short error in small line for debugging (user-friendly)
          small.textContent = String(e);
        }
      }

      function handleInput(val){
        if(val === 'AC'){
          expr = '';
          big.textContent = '0';
          small.textContent = '\u00A0';
          return;
        }
        if(val === 'DEL'){
          expr = expr.slice(0, -1);
          refresh();
          return;
        }
        if(val === '='){
          try{
            const res = evaluate(expr || lastAns || '0');
            big.textContent = res;
            expr = String(res);
            small.textContent = expr;
          }catch(e){
            big.textContent = 'ERR';
            small.textContent = String(e);
          }
          return;
        }

        expr += val;
        refresh();
      }

      keys.forEach(k => {
        k.addEventListener('click', () => {
          const v = k.dataset.val !== undefined ? k.dataset.val : k.textContent.trim();
          handleInput(v);
        });
      });

      mPlus.addEventListener('click', () => { try{ const v = evaluate(expr || lastAns || '0'); memory += Number(v || 0); }catch(e){ small.textContent = String(e); } });
      mMinus.addEventListener('click', () => { try{ const v = evaluate(expr || lastAns || '0'); memory -= Number(v || 0); }catch(e){ small.textContent = String(e); } });
      mR.addEventListener('click', () => { expr += String(memory); refresh(); });
      mC.addEventListener('click', () => { memory = 0; });

      degToggle.addEventListener('change', () => { settings.deg = degToggle.checked; modeLabel.textContent = settings.deg ? 'DEG' : 'RAD'; refresh(); });

      window.addEventListener('keydown', (e) => {
        if(e.key === 'Enter') { e.preventDefault(); handleInput('='); return; }
        if(e.key === 'Backspace') { handleInput('DEL'); return; }
        if(e.key === 'Escape') { handleInput('AC'); return; }
        const map = { '/':'/','*':'*','-':'-','+':'+','%':'%','^':'^', '(': '(', ')':')', ',':',', '.':'.' };
        if(/^[0-9]$/.test(e.key)) { expr += e.key; refresh(); return; }
        if(map[e.key]){ expr += map[e.key]; refresh(); return; }
        if(e.key.toLowerCase() === 'p'){ expr += 'PI'; refresh(); return; }
      });

      refresh();
      window.calcApp = { evaluate, getState: () => ({expr, lastAns, memory, settings}) };
    })();
  </script>
</body>
</html>
