# CalCount
Calorie counter
<!DOCTYPE html>

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Fuel Log</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&display=swap');
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { background: #080B10; color: #fff; font-family: 'DM Sans', system-ui, sans-serif; min-height: 100vh; padding-bottom: 60px; }
    input, button { font-family: inherit; outline: none; }
    input::placeholder { color: rgba(255,255,255,0.2); }
    ::-webkit-scrollbar { width: 3px; }
    ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.08); border-radius: 2px; }
    @keyframes fadeUp { from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)} }
    @keyframes slideDown { from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:translateY(0)} }
    @keyframes spin { to{transform:rotate(360deg)} }

```
.header { padding: 24px 22px 20px; border-bottom: 1px solid rgba(255,255,255,0.05); }
.header-top { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:4px; }
h1 { font-size:22px; font-weight:700; letter-spacing:-0.5px; }
.date-label { font-size:11px; color:rgba(255,255,255,0.3); margin-top:2px; }
.header-right { display:flex; flex-direction:column; align-items:flex-end; gap:6px; }
.remaining-box { border-radius:10px; padding:5px 12px; text-align:center; }
.remaining-val { font-size:17px; font-weight:700; font-family:monospace; }
.remaining-lbl { font-size:8px; color:rgba(255,255,255,0.3); letter-spacing:1px; text-transform:uppercase; }
.new-day-btn { background:rgba(255,255,255,0.04); border:1px solid rgba(255,255,255,0.08); border-radius:8px; padding:5px 10px; color:rgba(255,255,255,0.3); font-size:10px; cursor:pointer; transition:all 0.2s; }
.new-day-btn:hover { border-color:rgba(255,107,107,0.35); color:#ff6b6b; }
.tip-box { margin:12px 0 16px; background:rgba(96,165,250,0.06); border:1px solid rgba(96,165,250,0.12); border-radius:10px; padding:8px 12px; font-size:11px; color:rgba(96,165,250,0.7); display:flex; gap:6px; }
.tip-box strong { color:#60A5FA; }
.bar-row { display:flex; justify-content:space-between; margin-bottom:5px; }
.bar-lbl { font-size:10px; color:rgba(255,255,255,0.35); letter-spacing:1px; }
.bar-val { font-size:10px; font-family:monospace; color:rgba(255,255,255,0.4); }
.bar-track { height:6px; background:rgba(255,255,255,0.05); border-radius:3px; overflow:hidden; margin-bottom:16px; }
.bar-fill { height:100%; border-radius:3px; transition:width 0.7s cubic-bezier(.4,0,.2,1); }
.rings { display:flex; justify-content:space-around; }
.ring-wrap { display:flex; flex-direction:column; align-items:center; gap:2px; }
.ring-inner { position:relative; width:68px; height:68px; }
.ring-inner svg { position:absolute; }
.ring-text { position:absolute; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center; }
.ring-num { font-size:14px; font-weight:700; font-family:monospace; line-height:1; }
.ring-unit { font-size:8px; color:rgba(255,255,255,0.35); }
.ring-label { font-size:9px; color:rgba(255,255,255,0.4); letter-spacing:1.5px; text-transform:uppercase; }
.warning { margin:12px 22px 0; background:rgba(245,158,11,0.07); border:1px solid rgba(245,158,11,0.12); border-radius:10px; padding:8px 13px; font-size:11px; color:rgba(245,158,11,0.75); }
.add-section { padding:14px 22px 0; }
.add-btn-row { display:grid; grid-template-columns:1fr 1fr 1fr; gap:8px; }
.add-btn { padding:12px 6px; border-radius:12px; border:1.5px dashed rgba(255,255,255,0.12); background:transparent; color:rgba(255,255,255,0.5); font-size:12px; font-weight:500; cursor:pointer; display:flex; align-items:center; justify-content:center; gap:4px; transition:all 0.2s; }
#cameraBtn:hover { border-color:rgba(96,165,250,0.4); color:#60A5FA; }
#libraryBtn:hover { border-color:rgba(167,139,250,0.4); color:#a78bfa; }
#manualBtn:hover { border-color:rgba(126,232,162,0.4); color:#7EE8A2; }
.analyzing-box { background:rgba(96,165,250,0.06); border:1px solid rgba(96,165,250,0.15); border-radius:16px; padding:22px 16px; text-align:center; position:relative; overflow:hidden; }
.analyzing-preview { position:absolute; inset:0; background-size:cover; background-position:center; opacity:0.12; }
.spinner { width:30px; height:30px; border-radius:50%; border:2px solid rgba(96,165,250,0.3); border-top:2px solid #60A5FA; animation:spin 0.8s linear infinite; margin:0 auto 10px; }
.analyzing-txt { font-size:13px; color:#60A5FA; font-weight:500; position:relative; z-index:1; }
.error-box { background:rgba(255,107,107,0.07); border:1px solid rgba(255,107,107,0.15); border-radius:12px; padding:12px 14px; }
.error-msg { font-size:12px; color:#ff8a8a; margin-bottom:10px; }
.error-btns { display:flex; gap:8px; }
.err-btn { flex:1; padding:8px; border-radius:8px; font-size:12px; cursor:pointer; }

/* ── Manual form ── */
.form-box { background:rgba(255,255,255,0.04); border:1px solid rgba(255,255,255,0.09); border-radius:16px; padding:16px; animation:slideDown 0.25s ease; }
.form-title { font-size:13px; color:rgba(255,255,255,0.6); font-weight:600; margin-bottom:4px; }
.form-sub { font-size:11px; color:rgba(255,255,255,0.25); margin-bottom:14px; }
.emoji-label-row { display:flex; gap:8px; margin-bottom:10px; }
.emoji-btn { width:48px; height:48px; border-radius:10px; background:rgba(255,255,255,0.06); border:1px solid rgba(255,255,255,0.1); cursor:pointer; font-size:24px; display:flex; align-items:center; justify-content:center; position:relative; flex-shrink:0; }
.emoji-picker { position:absolute; top:54px; left:0; z-index:20; background:#1a1f2e; border:1px solid rgba(255,255,255,0.1); border-radius:12px; padding:10px; display:flex; flex-wrap:wrap; gap:4px; width:210px; box-shadow:0 8px 32px rgba(0,0,0,0.6); }
.emoji-opt { background:none; border:none; cursor:pointer; font-size:22px; padding:3px 5px; border-radius:6px; }
.emoji-opt:hover { background:rgba(255,255,255,0.1); }
.label-input { flex:1; height:48px; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.1); border-radius:10px; padding:0 12px; color:#fff; font-size:14px; }
.desc-input { width:100%; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.1); border-radius:10px; padding:11px 12px; color:#fff; font-size:13px; margin-bottom:10px; }
.macro-grid { display:grid; grid-template-columns:1fr 1fr 1fr 1fr; gap:8px; margin-bottom:14px; }
.macro-col { display:flex; flex-direction:column; gap:5px; }
.macro-lbl { font-size:10px; font-weight:600; }
.macro-input { width:100%; background:rgba(255,255,255,0.06); border:2px solid rgba(255,255,255,0.1); border-radius:8px; padding:10px 4px; font-size:16px; font-family:monospace; font-weight:700; text-align:center; transition:border-color 0.15s; }
.macro-input:focus { border-color:rgba(255,255,255,0.3); }
#calInput { border-color:rgba(255,255,255,0.2); }
#calInput:focus { border-color:#fff; }
.form-btns { display:flex; gap:8px; }
.cancel-btn { flex:1; padding:13px; border-radius:10px; border:1px solid rgba(255,255,255,0.1); background:transparent; color:rgba(255,255,255,0.4); font-size:14px; cursor:pointer; }
.submit-btn { flex:2; padding:13px; border-radius:10px; border:none; font-size:14px; font-weight:700; cursor:pointer; transition:all 0.2s; }
.submit-btn.active { background:linear-gradient(135deg,#7EE8A2,#60A5FA); color:#080B10; }
.submit-btn.inactive { background:rgba(255,255,255,0.05); color:rgba(255,255,255,0.2); cursor:not-allowed; }

.meal-list { padding:14px 22px 0; display:flex; flex-direction:column; gap:8px; }
.meal-list-header { display:flex; justify-content:space-between; margin-bottom:2px; }
.meal-list-lbl { font-size:10px; color:rgba(255,255,255,0.3); letter-spacing:1.5px; text-transform:uppercase; }
.meal-list-count { font-size:10px; font-family:monospace; color:rgba(255,255,255,0.2); }
.meal-card { background:rgba(255,255,255,0.035); border:1px solid rgba(255,255,255,0.07); border-radius:14px; padding:13px 15px; display:flex; gap:12px; align-items:flex-start; animation:fadeUp 0.3s ease both; }
.meal-emoji { width:40px; height:40px; border-radius:10px; background:rgba(255,255,255,0.05); display:flex; align-items:center; justify-content:center; font-size:20px; flex-shrink:0; }
.meal-body { flex:1; min-width:0; }
.meal-top { display:flex; justify-content:space-between; align-items:center; }
.meal-time { font-size:10px; color:#7EE8A2; font-family:monospace; }
.meal-label-txt { font-size:10px; color:rgba(255,255,255,0.25); margin-left:6px; }
.meal-cal-row { display:flex; align-items:center; gap:8px; }
.meal-cal { font-size:16px; font-weight:700; font-family:monospace; }
.meal-kcal { font-size:9px; color:rgba(255,255,255,0.25); }
.delete-btn { background:none; border:none; cursor:pointer; color:rgba(255,255,255,0.15); font-size:20px; padding:0; line-height:1; }
.delete-btn:hover { color:#ff6b6b; }
.meal-desc { font-size:11px; color:rgba(255,255,255,0.4); line-height:1.4; margin:3px 0 7px; }
.meal-macros { display:flex; gap:10px; }
.macro-pill { font-size:10px; color:rgba(255,255,255,0.3); }
.empty-state { text-align:center; padding:40px 0; }
.empty-icon { font-size:40px; margin-bottom:12px; }
.empty-txt { font-size:14px; color:rgba(255,255,255,0.25); line-height:1.6; }
.modal-overlay { position:fixed; inset:0; background:rgba(0,0,0,0.75); display:flex; align-items:center; justify-content:center; z-index:100; padding:24px; }
.modal-box { background:#13161f; border:1px solid rgba(255,255,255,0.1); border-radius:20px; padding:24px; width:100%; max-width:320px; animation:slideDown 0.2s ease; }
.modal-icon { font-size:36px; text-align:center; margin-bottom:12px; }
.modal-title { font-size:16px; font-weight:700; text-align:center; margin-bottom:8px; }
.modal-body { font-size:13px; color:rgba(255,255,255,0.4); text-align:center; line-height:1.5; margin-bottom:20px; }
.modal-btns { display:flex; gap:10px; }
.modal-keep { flex:1; padding:12px; border-radius:12px; border:1px solid rgba(255,255,255,0.1); background:transparent; color:rgba(255,255,255,0.5); font-size:14px; cursor:pointer; }
.modal-clear { flex:1; padding:12px; border-radius:12px; border:none; background:linear-gradient(135deg,#ff6b6b,#ff8e53); color:#fff; font-size:14px; font-weight:700; cursor:pointer; }
.api-setup { margin:12px 0 16px; background:rgba(126,232,162,0.05); border:1px solid rgba(126,232,162,0.15); border-radius:14px; padding:14px; }
.api-setup-title { font-size:12px; font-weight:600; color:#7EE8A2; margin-bottom:4px; }
.api-setup-sub { font-size:11px; color:rgba(255,255,255,0.35); margin-bottom:10px; line-height:1.5; }
.api-input-row { display:flex; gap:8px; }
.api-input { flex:1; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.1); border-radius:8px; padding:8px 10px; color:#fff; font-size:12px; font-family:monospace; }
.api-save-btn { padding:8px 14px; border-radius:8px; border:none; background:#7EE8A2; color:#080B10; font-size:12px; font-weight:700; cursor:pointer; }
```

  </style>
</head>
<body>

<!-- Permanent file inputs — never re-rendered -->

<input type="file" id="cameraInput" accept="image/*" capture="environment" style="display:none">
<input type="file" id="libraryInput" accept="image/*" style="display:none">

<div id="app"></div>
<div id="modal"></div>

<script>
const GOAL_CAL = 2300, GOAL_PRO = 160;
const STORAGE_KEY = 'fuellog_v3';
const API_KEY_STORE = 'fuellog_apikey';
const EMOJIS = ["🍽️","🥗","🍳","🥩","🐟","🍣","🍱","🥙","🌮","🍜","🥤","☕","🍸","🧃","🍎","🍌","🥜","🍫","🧁","🍰"];

let meals = [], idCounter = 1;
let uiMode = 'idle'; // idle | analyzing | error | manual
let errorMsg = '', previewUrl = '', lastSource = 'library';
let showReset = false, showEmojiPicker = false;
let apiKey = localStorage.getItem(API_KEY_STORE) || '';

// Form state — kept separately so inputs don't reset on re-render
let form = { label:'', emoji:'🍽️', description:'', calories:'', protein:'', carbs:'', fat:'' };

function loadStorage() {
  try { const d = JSON.parse(localStorage.getItem(STORAGE_KEY)||'{}'); meals=d.meals||[]; idCounter=d.idCounter||1; } catch {}
}
function saveStorage() {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify({meals,idCounter})); } catch {}
}
loadStorage();

// ── File inputs wired once, permanently ───────────────────────────────────
document.getElementById('cameraInput').addEventListener('change', function() {
  if (this.files[0]) { lastSource='camera'; analyzeImage(this.files[0]); }
  else { uiMode='idle'; render(); }
  this.value='';
});
document.getElementById('libraryInput').addEventListener('change', function() {
  if (this.files[0]) { lastSource='library'; analyzeImage(this.files[0]); }
  else { uiMode='idle'; render(); }
  this.value='';
});

// ── Render ─────────────────────────────────────────────────────────────────
function render() {
  const tot = meals.reduce((a,m)=>({
    calories:a.calories+(m.calories||0), protein:a.protein+(m.protein||0),
    carbs:a.carbs+(m.carbs||0), fat:a.fat+(m.fat||0)
  }), {calories:0,protein:0,carbs:0,fat:0});

  const rem = GOAL_CAL - tot.calories;
  const pct = Math.min(tot.calories/GOAL_CAL, 1);
  const today = new Date().toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric'});
  const remColor = rem>=0?'#7EE8A2':'#ff6b6b';
  const remBg    = rem>=0?'rgba(126,232,162,0.08)':'rgba(255,107,107,0.08)';
  const remBdr   = rem>=0?'rgba(126,232,162,0.15)':'rgba(255,107,107,0.15)';
  const barGrad  = pct>0.92?'linear-gradient(90deg,#F59E0B,#ff6b6b)':'linear-gradient(90deg,#7EE8A2,#60A5FA)';
  const hasKey   = !!apiKey;

  document.getElementById('app').innerHTML = `
    <div class="header">
      <div class="header-top">
        <div><h1>Fuel Log</h1><div class="date-label">${today}</div></div>
        <div class="header-right">
          <div class="remaining-box" style="background:${remBg};border:1px solid ${remBdr}">
            <div class="remaining-val" style="color:${remColor}">${Math.abs(rem)}</div>
            <div class="remaining-lbl">${rem>=0?'left':'over'}</div>
          </div>
          <button class="new-day-btn" onclick="showResetModal()">↺ New day</button>
        </div>
      </div>

      ${!hasKey ? `
      <div class="api-setup">
        <div class="api-setup-title">🔑 Enter your Anthropic API Key</div>
        <div class="api-setup-sub">Required for photo analysis. Get one free at <strong style="color:#7EE8A2">console.anthropic.com</strong> → API Keys. Saved in your browser only.</div>
        <div class="api-input-row">
          <input class="api-input" id="apiKeyInput" type="password" placeholder="sk-ant-...">
          <button class="api-save-btn" onclick="saveApiKey()">Save</button>
        </div>
      </div>` : `
      <div class="tip-box">
        <span>📸</span>
        <span>Tap <strong>📷 Camera</strong> or <strong>🖼️ Library</strong> to log with a photo, or <strong>✏️ Manual</strong> to type numbers in.</span>
      </div>`}

      <div class="bar-row"><span class="bar-lbl">CALORIES</span><span class="bar-val">${tot.calories} / ${GOAL_CAL}</span></div>
      <div class="bar-track"><div class="bar-fill" style="width:${pct*100}%;background:${barGrad}"></div></div>
      <div class="rings">
        ${ring(tot.protein,GOAL_PRO,'#7EE8A2','Protein')}
        ${ring(tot.carbs,250,'#60A5FA','Carbs')}
        ${ring(tot.fat,80,'#F59E0B','Fat')}
      </div>
    </div>

    ${tot.protein<100 && meals.length>=2 ? `<div class="warning">⚠️ ${tot.protein}g protein so far — aim for ${GOAL_PRO}g to protect muscle.</div>` : ''}

    <div class="add-section" id="addSection"></div>

    <div class="meal-list">
      ${meals.length>0 ? `
        <div class="meal-list-header">
          <span class="meal-list-lbl">Today's meals</span>
          <span class="meal-list-count">${meals.length} items</span>
        </div>
        ${meals.map(mealCard).join('')}
      ` : `
        <div class="empty-state">
          <div class="empty-icon">🍽️</div>
          <div class="empty-txt">No meals logged yet.<br>Tap a button above to get started!</div>
        </div>
      `}
    </div>
  `;

  renderAddSection();
  renderModal();
}

function renderAddSection() {
  const el = document.getElementById('addSection');
  if (!el) return;

  if (uiMode === 'idle') {
    el.innerHTML = `
      <div class="add-btn-row">
        <button id="cameraBtn" class="add-btn" onclick="document.getElementById('cameraInput').click()">📷 Camera</button>
        <button id="libraryBtn" class="add-btn" onclick="document.getElementById('libraryInput').click()">🖼️ Library</button>
        <button id="manualBtn" class="add-btn" onclick="openManual()">✏️ Manual</button>
      </div>`;
    return;
  }

  if (uiMode === 'analyzing') {
    el.innerHTML = `
      <div class="analyzing-box">
        ${previewUrl?`<div class="analyzing-preview" style="background-image:url(${previewUrl})"></div>`:''}
        <div style="position:relative;z-index:1"><div class="spinner"></div><div class="analyzing-txt">Analyzing your food…</div></div>
      </div>`;
    return;
  }

  if (uiMode === 'error') {
    el.innerHTML = `
      <div class="error-box">
        <div class="error-msg">⚠️ ${errorMsg}</div>
        <div class="error-btns">
          <button class="err-btn" style="border:1px solid rgba(96,165,250,0.2);background:transparent;color:#60A5FA" onclick="retryPhoto()">Try again</button>
          <button class="err-btn" style="border:none;background:rgba(126,232,162,0.15);color:#7EE8A2" onclick="openManual()">Manual entry</button>
          <button class="err-btn" style="flex:0;padding:8px 12px;border:1px solid rgba(255,255,255,0.08);background:transparent;color:rgba(255,255,255,0.3)" onclick="uiMode='idle';render()">Cancel</button>
        </div>
      </div>`;
    return;
  }

  if (uiMode === 'manual') {
    renderManualForm(el);
  }
}

function renderManualForm(container) {
  const f = form;
  const ok = f.calories.trim() !== '';
  container.innerHTML = `
    <div class="form-box">
      <div class="form-title">Log a meal</div>
      <div class="form-sub">Fill in the calories (required) and any other details</div>

      <div class="emoji-label-row">
        <button class="emoji-btn" id="emojiBtnEl" onclick="toggleEmoji()">
          <span>${f.emoji}</span>
          ${showEmojiPicker ? `<div class="emoji-picker" onclick="event.stopPropagation()">${EMOJIS.map(e=>`<button class="emoji-opt" onclick="pickEmoji('${e}')">${e}</button>`).join('')}</div>` : ''}
        </button>
        <input class="label-input" id="labelInput" placeholder="Meal name (e.g. Breakfast)" value="${esc(f.label)}">
      </div>

      <input class="desc-input" id="descInput" placeholder="Description (optional)" value="${esc(f.description)}">

      <div class="macro-grid">
        <div class="macro-col">
          <div class="macro-lbl" style="color:#fff">Cal *</div>
          <input class="macro-input" id="calInput" type="number" inputmode="numeric" placeholder="0" value="${f.calories}" style="color:#fff">
        </div>
        <div class="macro-col">
          <div class="macro-lbl" style="color:#7EE8A2">Protein g</div>
          <input class="macro-input" id="proInput" type="number" inputmode="numeric" placeholder="0" value="${f.protein}" style="color:#7EE8A2">
        </div>
        <div class="macro-col">
          <div class="macro-lbl" style="color:#60A5FA">Carbs g</div>
          <input class="macro-input" id="carbInput" type="number" inputmode="numeric" placeholder="0" value="${f.carbs}" style="color:#60A5FA">
        </div>
        <div class="macro-col">
          <div class="macro-lbl" style="color:#F59E0B">Fat g</div>
          <input class="macro-input" id="fatInput" type="number" inputmode="numeric" placeholder="0" value="${f.fat}" style="color:#F59E0B">
        </div>
      </div>

      <div class="form-btns">
        <button class="cancel-btn" onclick="cancelForm()">Cancel</button>
        <button class="submit-btn ${ok?'active':'inactive'}" id="submitBtn" onclick="submitForm()">
          ${ok ? '✓ Add to Log' : 'Enter calories to continue'}
        </button>
      </div>
    </div>`;

  // Attach live listeners AFTER rendering — no oninput in HTML
  bindFormListeners();
}

function bindFormListeners() {
  const cal  = document.getElementById('calInput');
  const pro  = document.getElementById('proInput');
  const carb = document.getElementById('carbInput');
  const fat  = document.getElementById('fatInput');
  const lbl  = document.getElementById('labelInput');
  const desc = document.getElementById('descInput');

  if (cal) {
    cal.addEventListener('input', () => {
      form.calories = cal.value;
      updateSubmitBtn();
    });
    // Focus the calories field automatically
    setTimeout(() => cal.focus(), 80);
  }
  if (pro)  pro.addEventListener('input',  () => { form.protein = pro.value; });
  if (carb) carb.addEventListener('input', () => { form.carbs   = carb.value; });
  if (fat)  fat.addEventListener('input',  () => { form.fat     = fat.value; });
  if (lbl)  lbl.addEventListener('input',  () => { form.label   = lbl.value; });
  if (desc) desc.addEventListener('input', () => { form.description = desc.value; });
}

function updateSubmitBtn() {
  const btn = document.getElementById('submitBtn');
  if (!btn) return;
  const ok = form.calories.trim() !== '';
  btn.className = `submit-btn ${ok?'active':'inactive'}`;
  btn.textContent = ok ? '✓ Add to Log' : 'Enter calories to continue';
}

function renderModal() {
  document.getElementById('modal').innerHTML = showReset ? `
    <div class="modal-overlay" onclick="if(event.target===this){showReset=false;render();}">
      <div class="modal-box">
        <div class="modal-icon">🗑️</div>
        <div class="modal-title">Start a new day?</div>
        <div class="modal-body">This will clear all of today's meals. Use this each morning to reset.</div>
        <div class="modal-btns">
          <button class="modal-keep" onclick="showReset=false;render()">Keep it</button>
          <button class="modal-clear" onclick="resetDay()">Clear day</button>
        </div>
      </div>
    </div>` : '';
}

function ring(value, max, color, label) {
  const r=26, circ=2*Math.PI*r, dash=circ*Math.min(value/max,1);
  return `<div class="ring-wrap">
    <div class="ring-inner">
      <svg width="68" height="68">
        <circle cx="34" cy="34" r="${r}" fill="none" stroke="rgba(255,255,255,0.07)" stroke-width="5"/>
        <circle cx="34" cy="34" r="${r}" fill="none" stroke="${color}" stroke-width="5"
          stroke-dasharray="${dash} ${circ}" stroke-linecap="round"
          style="transform:rotate(-90deg);transform-origin:34px 34px"/>
      </svg>
      <div class="ring-text"><span class="ring-num">${value}</span><span class="ring-unit">g</span></div>
    </div>
    <span class="ring-label">${label}</span>
  </div>`;
}

function mealCard(m) {
  return `<div class="meal-card">
    <div class="meal-emoji">${m.emoji}</div>
    <div class="meal-body">
      <div class="meal-top">
        <div><span class="meal-time">${m.time}</span><span class="meal-label-txt">· ${m.label}</span></div>
        <div class="meal-cal-row">
          <span class="meal-cal">${m.calories}</span><span class="meal-kcal">kcal</span>
          <button class="delete-btn" onclick="deleteMeal(${m.id})">×</button>
        </div>
      </div>
      <div class="meal-desc">${m.description||''}</div>
      <div class="meal-macros">
        <span class="macro-pill"><span style="color:#7EE8A2;font-weight:600">P</span> ${m.protein}g</span>
        <span class="macro-pill"><span style="color:#60A5FA;font-weight:600">C</span> ${m.carbs}g</span>
        <span class="macro-pill"><span style="color:#F59E0B;font-weight:600">F</span> ${m.fat}g</span>
      </div>
    </div>
  </div>`;
}

// ── Actions ────────────────────────────────────────────────────────────────
function esc(s) { return (s||'').replace(/"/g,'&quot;'); }

function openManual() { uiMode='manual'; showEmojiPicker=false; render(); }

function cancelForm() {
  form = { label:'', emoji:'🍽️', description:'', calories:'', protein:'', carbs:'', fat:'' };
  showEmojiPicker = false; uiMode='idle'; render();
}

function retryPhoto() {
  if (lastSource==='camera') document.getElementById('cameraInput').click();
  else document.getElementById('libraryInput').click();
}

function toggleEmoji() { showEmojiPicker=!showEmojiPicker; renderManualForm(document.getElementById('addSection')); }
function pickEmoji(e) { form.emoji=e; showEmojiPicker=false; renderManualForm(document.getElementById('addSection')); }

function submitForm() {
  if (!form.calories.trim()) return;
  const t = new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
  meals.push({
    id: idCounter++, time: t,
    label: form.label||'Meal', description: form.description,
    calories: +form.calories||0, protein: +form.protein||0,
    carbs: +form.carbs||0, fat: +form.fat||0,
    emoji: form.emoji
  });
  saveStorage();
  form = { label:'', emoji:'🍽️', description:'', calories:'', protein:'', carbs:'', fat:'' };
  showEmojiPicker=false; uiMode='idle'; render();
}

function deleteMeal(id) { meals=meals.filter(m=>m.id!==id); saveStorage(); render(); }
function showResetModal() { showReset=true; render(); }
function resetDay() { meals=[]; idCounter=1; saveStorage(); showReset=false; uiMode='idle'; render(); }

function saveApiKey() {
  const val = document.getElementById('apiKeyInput')?.value?.trim();
  if (!val) return;
  apiKey=val; localStorage.setItem(API_KEY_STORE, val); render();
}

async function analyzeImage(file) {
  if (!apiKey) { errorMsg='Please save your API key first.'; uiMode='error'; render(); return; }
  const reader = new FileReader();
  reader.onload = async (e) => {
    previewUrl=e.target.result; uiMode='analyzing'; render();
    try {
      const base64=e.target.result.split(',')[1];
      const res = await fetch('https://api.anthropic.com/v1/messages', {
        method:'POST',
        headers:{ 'Content-Type':'application/json','x-api-key':apiKey,'anthropic-version':'2023-06-01','anthropic-dangerous-allow-browser':'true' },
        body: JSON.stringify({
          model:'claude-sonnet-4-20250514', max_tokens:600,
          system:`Nutrition analyzer. User: 5'10", 193lb, active, 2300cal/day, 160g protein goal.
Return ONLY valid JSON, no markdown:
{"label":"short type","emoji":"single emoji","description":"max 80 chars","calories":number,"protein":number,"carbs":number,"fat":number}`,
          messages:[{role:'user',content:[
            {type:'image',source:{type:'base64',media_type:file.type,data:base64}},
            {type:'text',text:'Analyze this food and return JSON.'}
          ]}]
        })
      });
      const data=await res.json();
      if (data.error) throw new Error(data.error.message);
      const text=data.content?.find(b=>b.type==='text')?.text||'';
      const p=JSON.parse(text.replace(/```json|```/g,'').trim());
      const t=new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
      meals.push({ id:idCounter++, time:t, label:p.label||'Meal', description:p.description||'', calories:p.calories||0, protein:p.protein||0, carbs:p.carbs||0, fat:p.fat||0, emoji:p.emoji||'🍽️' });
      saveStorage(); uiMode='idle'; previewUrl=''; render();
    } catch(err) {
      errorMsg='Analysis failed — check your API key or use Manual entry.';
      uiMode='error'; previewUrl=''; render();
    }
  };
  reader.readAsDataURL(file);
}

render();
</script>

</body>
</html>
