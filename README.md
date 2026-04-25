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

```
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  background: #080B10;
  color: #fff;
  font-family: 'DM Sans', system-ui, sans-serif;
  min-height: 100vh;
  padding-bottom: 60px;
}
input, button, select { font-family: inherit; outline: none; }
input::placeholder { color: rgba(255,255,255,0.2); }
::-webkit-scrollbar { width: 3px; }
::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.08); border-radius: 2px; }

@keyframes fadeUp { from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)} }
@keyframes slideDown { from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:translateY(0)} }
@keyframes spin { to{transform:rotate(360deg)} }

/* ── Layout ── */
.header {
  padding: 24px 22px 20px;
  border-bottom: 1px solid rgba(255,255,255,0.05);
}
.header-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 4px;
}
h1 { font-size: 22px; font-weight: 700; letter-spacing: -0.5px; }
.date-label { font-size: 11px; color: rgba(255,255,255,0.3); margin-top: 2px; }

.header-right { display: flex; flex-direction: column; align-items: flex-end; gap: 6px; }

.remaining-box {
  border-radius: 10px;
  padding: 5px 12px;
  text-align: center;
}
.remaining-val { font-size: 17px; font-weight: 700; font-family: monospace; }
.remaining-lbl { font-size: 8px; color: rgba(255,255,255,0.3); letter-spacing: 1px; text-transform: uppercase; }

.new-day-btn {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 8px;
  padding: 5px 10px;
  color: rgba(255,255,255,0.3);
  font-size: 10px;
  cursor: pointer;
  letter-spacing: 0.5px;
  transition: all 0.2s;
}
.new-day-btn:hover { border-color: rgba(255,107,107,0.35); color: #ff6b6b; }

.tip-box {
  margin: 12px 0 16px;
  background: rgba(96,165,250,0.06);
  border: 1px solid rgba(96,165,250,0.12);
  border-radius: 10px;
  padding: 8px 12px;
  font-size: 11px;
  color: rgba(96,165,250,0.7);
  display: flex;
  gap: 6px;
  align-items: flex-start;
}
.tip-box strong { color: #60A5FA; }

/* ── Calorie bar ── */
.bar-row { display: flex; justify-content: space-between; margin-bottom: 5px; }
.bar-lbl { font-size: 10px; color: rgba(255,255,255,0.35); letter-spacing: 1px; }
.bar-val { font-size: 10px; font-family: monospace; color: rgba(255,255,255,0.4); }
.bar-track { height: 6px; background: rgba(255,255,255,0.05); border-radius: 3px; overflow: hidden; margin-bottom: 16px; }
.bar-fill { height: 100%; border-radius: 3px; transition: width 0.7s cubic-bezier(.4,0,.2,1); }

/* ── Macro rings ── */
.rings { display: flex; justify-content: space-around; }
.ring-wrap { display: flex; flex-direction: column; align-items: center; gap: 2px; }
.ring-inner { position: relative; width: 68px; height: 68px; }
.ring-inner svg { position: absolute; transform: rotate(-90deg); }
.ring-text { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.ring-num { font-size: 14px; font-weight: 700; font-family: monospace; line-height: 1; }
.ring-unit { font-size: 8px; color: rgba(255,255,255,0.35); }
.ring-label { font-size: 9px; color: rgba(255,255,255,0.4); letter-spacing: 1.5px; text-transform: uppercase; }

/* ── Warnings ── */
.warning {
  margin: 12px 22px 0;
  background: rgba(245,158,11,0.07);
  border: 1px solid rgba(245,158,11,0.12);
  border-radius: 10px;
  padding: 8px 13px;
  font-size: 11px;
  color: rgba(245,158,11,0.75);
}

/* ── Add section ── */
.add-section { padding: 14px 22px 0; }
.add-btn-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; }
.add-btn {
  padding: 12px 6px;
  border-radius: 12px;
  border: 1.5px dashed rgba(255,255,255,0.12);
  background: transparent;
  color: rgba(255,255,255,0.5);
  font-size: 12px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
  transition: all 0.2s;
}

/* ── Analyzing ── */
.analyzing-box {
  background: rgba(96,165,250,0.06);
  border: 1px solid rgba(96,165,250,0.15);
  border-radius: 16px;
  padding: 22px 16px;
  text-align: center;
  position: relative;
  overflow: hidden;
}
.analyzing-preview {
  position: absolute; inset: 0;
  background-size: cover; background-position: center;
  opacity: 0.12;
}
.spinner {
  width: 30px; height: 30px; border-radius: 50%;
  border: 2px solid rgba(96,165,250,0.3);
  border-top: 2px solid #60A5FA;
  animation: spin 0.8s linear infinite;
  margin: 0 auto 10px;
}
.analyzing-txt { font-size: 13px; color: #60A5FA; font-weight: 500; position: relative; z-index: 1; }

/* ── Error ── */
.error-box {
  background: rgba(255,107,107,0.07);
  border: 1px solid rgba(255,107,107,0.15);
  border-radius: 12px;
  padding: 12px 14px;
}
.error-msg { font-size: 12px; color: #ff8a8a; margin-bottom: 10px; }
.error-btns { display: flex; gap: 8px; }
.err-btn { flex: 1; padding: 8px; border-radius: 8px; font-size: 12px; cursor: pointer; border: none; }

/* ── Manual form ── */
.form-box {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.09);
  border-radius: 16px;
  padding: 16px;
  animation: slideDown 0.25s ease;
}
.form-title { font-size: 12px; color: rgba(255,255,255,0.5); font-weight: 600; margin-bottom: 4px; }
.form-sub { font-size: 11px; color: rgba(255,255,255,0.25); margin-bottom: 12px; }
.emoji-label-row { display: flex; gap: 8px; margin-bottom: 8px; }
.emoji-btn {
  width: 44px; height: 44px; border-radius: 10px;
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.08);
  cursor: pointer; font-size: 22px;
  display: flex; align-items: center; justify-content: center;
  position: relative;
}
.emoji-picker {
  position: absolute; top: 50px; left: 0; z-index: 10;
  background: #1a1f2e;
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 12px; padding: 10px;
  display: flex; flex-wrap: wrap; gap: 4px; width: 200px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.5);
}
.emoji-opt { background: none; border: none; cursor: pointer; font-size: 20px; padding: 2px 4px; border-radius: 6px; }
.emoji-opt:hover { background: rgba(255,255,255,0.1); }
.text-input {
  width: 100%;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 10px;
  padding: 10px 12px;
  color: #fff; font-size: 13px;
  margin-bottom: 8px;
}
.label-input {
  flex: 1; height: 44px;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 10px;
  padding: 0 12px;
  color: #fff; font-size: 13px;
}
.macro-grid { display: grid; grid-template-columns: 1fr 1fr 1fr 1fr; gap: 8px; margin-bottom: 12px; }
.macro-lbl { font-size: 9px; margin-bottom: 4px; }
.macro-input {
  width: 100%;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 8px;
  padding: 8px 4px;
  font-size: 15px; font-family: monospace; font-weight: 700;
  text-align: center;
}
.form-btns { display: flex; gap: 8px; }
.cancel-btn {
  flex: 1; padding: 11px; border-radius: 10px;
  border: 1px solid rgba(255,255,255,0.08);
  background: transparent; color: rgba(255,255,255,0.4);
  font-size: 13px; cursor: pointer;
}
.submit-btn {
  flex: 2; padding: 11px; border-radius: 10px; border: none;
  font-size: 13px; font-weight: 700; cursor: pointer;
  transition: all 0.2s;
}

/* ── Meal list ── */
.meal-list { padding: 14px 22px 0; display: flex; flex-direction: column; gap: 8px; }
.meal-list-header { display: flex; justify-content: space-between; margin-bottom: 2px; }
.meal-list-lbl { font-size: 10px; color: rgba(255,255,255,0.3); letter-spacing: 1.5px; text-transform: uppercase; }
.meal-list-count { font-size: 10px; font-family: monospace; color: rgba(255,255,255,0.2); }

.meal-card {
  background: rgba(255,255,255,0.035);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: 14px; padding: 13px 15px;
  display: flex; gap: 12px; align-items: flex-start;
  animation: fadeUp 0.3s ease both;
}
.meal-emoji {
  width: 40px; height: 40px; border-radius: 10px;
  background: rgba(255,255,255,0.05);
  display: flex; align-items: center; justify-content: center;
  font-size: 20px; flex-shrink: 0;
}
.meal-body { flex: 1; min-width: 0; }
.meal-top { display: flex; justify-content: space-between; align-items: center; }
.meal-time { font-size: 10px; color: #7EE8A2; font-family: monospace; }
.meal-label-txt { font-size: 10px; color: rgba(255,255,255,0.25); margin-left: 6px; }
.meal-cal-row { display: flex; align-items: center; gap: 8px; }
.meal-cal { font-size: 16px; font-weight: 700; font-family: monospace; }
.meal-kcal { font-size: 9px; color: rgba(255,255,255,0.25); }
.delete-btn {
  background: none; border: none; cursor: pointer;
  color: rgba(255,255,255,0.15); font-size: 18px; padding: 0; line-height: 1;
}
.delete-btn:hover { color: #ff6b6b; }
.meal-desc { font-size: 11px; color: rgba(255,255,255,0.4); line-height: 1.4; margin: 3px 0 7px; }
.meal-macros { display: flex; gap: 10px; }
.macro-pill { font-size: 10px; color: rgba(255,255,255,0.3); }

.empty-state { text-align: center; padding: 40px 0; }
.empty-icon { font-size: 40px; margin-bottom: 12px; }
.empty-txt { font-size: 14px; color: rgba(255,255,255,0.25); line-height: 1.6; }

/* ── Reset modal ── */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.75);
  display: flex; align-items: center; justify-content: center;
  z-index: 100; padding: 24px;
}
.modal-box {
  background: #13161f;
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 20px; padding: 24px;
  width: 100%; max-width: 320px;
  animation: slideDown 0.2s ease;
}
.modal-icon { font-size: 36px; text-align: center; margin-bottom: 12px; }
.modal-title { font-size: 16px; font-weight: 700; text-align: center; margin-bottom: 8px; }
.modal-body { font-size: 13px; color: rgba(255,255,255,0.4); text-align: center; line-height: 1.5; margin-bottom: 20px; }
.modal-btns { display: flex; gap: 10px; }
.modal-keep { flex: 1; padding: 12px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.1); background: transparent; color: rgba(255,255,255,0.5); font-size: 14px; cursor: pointer; }
.modal-clear { flex: 1; padding: 12px; border-radius: 12px; border: none; background: linear-gradient(135deg,#ff6b6b,#ff8e53); color: #fff; font-size: 14px; font-weight: 700; cursor: pointer; }

/* ── API key setup ── */
.api-setup {
  margin: 16px 22px 0;
  background: rgba(126,232,162,0.05);
  border: 1px solid rgba(126,232,162,0.15);
  border-radius: 14px;
  padding: 14px;
}
.api-setup-title { font-size: 12px; font-weight: 600; color: #7EE8A2; margin-bottom: 4px; }
.api-setup-sub { font-size: 11px; color: rgba(255,255,255,0.35); margin-bottom: 10px; line-height: 1.5; }
.api-input-row { display: flex; gap: 8px; }
.api-input {
  flex: 1; background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px; padding: 8px 10px;
  color: #fff; font-size: 12px; font-family: monospace;
}
.api-save-btn {
  padding: 8px 14px; border-radius: 8px; border: none;
  background: #7EE8A2; color: #080B10;
  font-size: 12px; font-weight: 700; cursor: pointer;
}
.api-saved { font-size: 11px; color: #7EE8A2; margin-top: 6px; }
```

  </style>
</head>
<body>

<div id="app"></div>

<script>
// ─── Constants ───────────────────────────────────────────────────────────────
const GOAL_CAL = 2300;
const GOAL_PRO = 160;
const STORAGE_KEY = 'fuellog_v3';
const API_KEY_STORAGE = 'fuellog_apikey';
const EMOJIS = ["🍽️","🥗","🍳","🥩","🐟","🍣","🍱","🥙","🌮","🍜","🥤","☕","🍸","🧃","🍎","🍌","🥜","🍫","🧁","🍰"];

// ─── State ───────────────────────────────────────────────────────────────────
let state = {
  meals: [],
  idCounter: 1,
  mode: null,       // null | 'camera' | 'library' | 'manual' | 'analyzing' | 'error'
  errorMsg: '',
  previewUrl: null,
  showReset: false,
  showEmojiPicker: false,
  apiKey: localStorage.getItem(API_KEY_STORAGE) || '',
  apiKeySaved: !!localStorage.getItem(API_KEY_STORAGE),
  form: { label:'', emoji:'🍽️', description:'', calories:'', protein:'', carbs:'', fat:'' },
};

function loadStorage() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (raw) { const d = JSON.parse(raw); state.meals = d.meals||[]; state.idCounter = d.idCounter||1; }
  } catch {}
}

function saveStorage() {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify({ meals: state.meals, idCounter: state.idCounter })); } catch {}
}

loadStorage();

// ─── Render ──────────────────────────────────────────────────────────────────
function render() {
  const totals = state.meals.reduce((a,m) => ({
    calories: a.calories+(m.calories||0), protein: a.protein+(m.protein||0),
    carbs: a.carbs+(m.carbs||0), fat: a.fat+(m.fat||0),
  }), {calories:0,protein:0,carbs:0,fat:0});

  const remaining = GOAL_CAL - totals.calories;
  const calPct = Math.min(totals.calories / GOAL_CAL, 1);
  const remColor = remaining >= 0 ? '#7EE8A2' : '#ff6b6b';
  const remBg = remaining >= 0 ? 'rgba(126,232,162,0.08)' : 'rgba(255,107,107,0.08)';
  const remBorder = remaining >= 0 ? 'rgba(126,232,162,0.15)' : 'rgba(255,107,107,0.15)';
  const barGradient = calPct > 0.92 ? 'linear-gradient(90deg,#F59E0B,#ff6b6b)' : 'linear-gradient(90deg,#7EE8A2,#60A5FA)';
  const today = new Date().toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric'});

  document.getElementById('app').innerHTML = `
    ${state.showReset ? `
    <div class="modal-overlay" onclick="if(event.target===this)setState({showReset:false})">
      <div class="modal-box">
        <div class="modal-icon">🗑️</div>
        <div class="modal-title">Start a new day?</div>
        <div class="modal-body">This will clear all of today's meals. Use this each morning to reset your log.</div>
        <div class="modal-btns">
          <button class="modal-keep" onclick="setState({showReset:false})">Keep it</button>
          <button class="modal-clear" onclick="resetDay()">Clear day</button>
        </div>
      </div>
    </div>` : ''}

    <div class="header">
      <div class="header-top">
        <div>
          <h1>Fuel Log</h1>
          <div class="date-label">${today}</div>
        </div>
        <div class="header-right">
          <div class="remaining-box" style="background:${remBg};border:1px solid ${remBorder}">
            <div class="remaining-val" style="color:${remColor}">${Math.abs(remaining)}</div>
            <div class="remaining-lbl">${remaining>=0?'left':'over'}</div>
          </div>
          <button class="new-day-btn" onclick="setState({showReset:true})">↺ New day</button>
        </div>
      </div>

      ${!state.apiKeySaved ? `
      <div class="api-setup">
        <div class="api-setup-title">🔑 Set your Anthropic API Key</div>
        <div class="api-setup-sub">Needed to analyze food photos. Get one free at console.anthropic.com → API Keys. It stays saved in your browser.</div>
        <div class="api-input-row">
          <input class="api-input" id="apiKeyInput" type="password" placeholder="sk-ant-..." value="${state.apiKey}" />
          <button class="api-save-btn" onclick="saveApiKey()">Save</button>
        </div>
      </div>` : `
      <div class="tip-box">
        <span>📸</span>
        <span>Tap <strong>📷 Camera</strong> or <strong>🖼️ Library</strong> to log a meal with a photo, or use <strong>✏️ Manual</strong> to type in numbers from this chat.</span>
      </div>`}

      <div class="bar-row"><span class="bar-lbl">CALORIES</span><span class="bar-val">${totals.calories} / ${GOAL_CAL}</span></div>
      <div class="bar-track"><div class="bar-fill" style="width:${calPct*100}%;background:${barGradient}"></div></div>

      <div class="rings">
        ${ring(totals.protein, GOAL_PRO, '#7EE8A2', 'Protein')}
        ${ring(totals.carbs, 250, '#60A5FA', 'Carbs')}
        ${ring(totals.fat, 80, '#F59E0B', 'Fat')}
      </div>
    </div>

    ${totals.protein < 100 && state.meals.length >= 2 ? `
    <div class="warning">⚠️ ${totals.protein}g protein so far — aim for ${GOAL_PRO}g to protect muscle.</div>` : ''}

    <div class="add-section">
      ${addSection()}
    </div>

    <div class="meal-list">
      ${state.meals.length > 0 ? `
        <div class="meal-list-header">
          <span class="meal-list-lbl">Today's meals</span>
          <span class="meal-list-count">${state.meals.length} items</span>
        </div>
        ${state.meals.map(mealCard).join('')}
      ` : `
        <div class="empty-state">
          <div class="empty-icon">🍽️</div>
          <div class="empty-txt">No meals logged yet.<br>Tap a button above to get started!</div>
        </div>
      `}
    </div>
  `;
}

function ring(value, max, color, label) {
  const r = 26, circ = 2 * Math.PI * r;
  const pct = Math.min(value/max, 1);
  const dash = circ * pct;
  return `
    <div class="ring-wrap">
      <div class="ring-inner">
        <svg width="68" height="68">
          <circle cx="34" cy="34" r="${r}" fill="none" stroke="rgba(255,255,255,0.07)" stroke-width="5"/>
          <circle cx="34" cy="34" r="${r}" fill="none" stroke="${color}" stroke-width="5"
            stroke-dasharray="${dash} ${circ}" stroke-linecap="round"
            style="transform:rotate(-90deg);transform-origin:34px 34px;transition:stroke-dasharray 0.7s"/>
        </svg>
        <div class="ring-text">
          <span class="ring-num">${value}</span>
          <span class="ring-unit">g</span>
        </div>
      </div>
      <span class="ring-label">${label}</span>
    </div>`;
}

function addSection() {
  const m = state.mode;
  if (m === null) {
    return `<div class="add-btn-row">
      <button class="add-btn" style="--c:#60A5FA" onmouseover="this.style.borderColor='rgba(96,165,250,0.4)';this.style.color='#60A5FA'" onmouseout="this.style.borderColor='rgba(255,255,255,0.12)';this.style.color='rgba(255,255,255,0.5)'" onclick="triggerCamera()">📷 Camera</button>
      <button class="add-btn" onmouseover="this.style.borderColor='rgba(167,139,250,0.4)';this.style.color='#a78bfa'" onmouseout="this.style.borderColor='rgba(255,255,255,0.12)';this.style.color='rgba(255,255,255,0.5)'" onclick="triggerLibrary()">🖼️ Library</button>
      <button class="add-btn" onmouseover="this.style.borderColor='rgba(126,232,162,0.4)';this.style.color='#7EE8A2'" onmouseout="this.style.borderColor='rgba(255,255,255,0.12)';this.style.color='rgba(255,255,255,0.5)'" onclick="setState({mode:'manual'})">✏️ Manual</button>
    </div>
    <input type="file" id="cameraInput" accept="image/*" capture="environment" style="display:none" onchange="handleFile(this,'camera')">
    <input type="file" id="libraryInput" accept="image/*" style="display:none" onchange="handleFile(this,'library')">`;
  }
  if (m === 'analyzing') {
    return `<div class="analyzing-box">
      ${state.previewUrl ? `<div class="analyzing-preview" style="background-image:url(${state.previewUrl})"></div>` : ''}
      <div style="position:relative;z-index:1"><div class="spinner"></div><div class="analyzing-txt">Analyzing your food…</div></div>
    </div>`;
  }
  if (m === 'error') {
    return `<div class="error-box">
      <div class="error-msg">⚠️ ${state.errorMsg}</div>
      <div class="error-btns">
        <button class="err-btn" style="border:1px solid rgba(96,165,250,0.2);background:transparent;color:#60A5FA" onclick="retryPhoto()">Try again</button>
        <button class="err-btn" style="border:none;background:rgba(126,232,162,0.15);color:#7EE8A2" onclick="setState({mode:'manual',errorMsg:''})">Enter manually</button>
        <button class="err-btn" style="flex:0;padding:8px 12px;border:1px solid rgba(255,255,255,0.08);background:transparent;color:rgba(255,255,255,0.3)" onclick="setState({mode:null,errorMsg:''})">Cancel</button>
      </div>
    </div>`;
  }
  if (m === 'manual') {
    const f = state.form;
    const canSubmit = f.calories;
    return `<div class="form-box">
      <div class="form-title">Log a meal</div>
      <div class="form-sub">Enter numbers from Claude's analysis, or type manually</div>
      <div class="emoji-label-row">
        <div class="emoji-btn" onclick="toggleEmoji()">
          <span>${f.emoji}</span>
          ${state.showEmojiPicker ? `<div class="emoji-picker" onclick="event.stopPropagation()">
            ${EMOJIS.map(e=>`<button class="emoji-opt" onclick="pickEmoji('${e}')">${e}</button>`).join('')}
          </div>` : ''}
        </div>
        <input class="label-input" placeholder="Meal name (e.g. Breakfast)" value="${f.label}" oninput="updateForm('label',this.value)">
      </div>
      <input class="text-input" placeholder="Description (optional)" value="${f.description}" oninput="updateForm('description',this.value)">
      <div class="macro-grid">
        ${[['Cal *','calories','rgba(255,255,255,0.4)','#fff'],['Protein','protein','#7EE8A2','#7EE8A2'],['Carbs','carbs','#60A5FA','#60A5FA'],['Fat','fat','#F59E0B','#F59E0B']].map(([label,field,lc,ic])=>`
          <div>
            <div class="macro-lbl" style="color:${lc}">${label}</div>
            <input class="macro-input" type="number" placeholder="0" value="${f[field]}" style="color:${ic}" oninput="updateForm('${field}',this.value)">
          </div>`).join('')}
      </div>
      <div class="form-btns">
        <button class="cancel-btn" onclick="setState({mode:null,form:{label:'',emoji:'🍽️',description:'',calories:'',protein:'',carbs:'',fat:''},showEmojiPicker:false})">Cancel</button>
        <button class="submit-btn" style="background:${canSubmit?'linear-gradient(135deg,#7EE8A2,#60A5FA)':'rgba(255,255,255,0.05)'};color:${canSubmit?'#080B10':'rgba(255,255,255,0.2)'};cursor:${canSubmit?'pointer':'default'}" onclick="submitForm()" ${canSubmit?'':' disabled'}>Add to Log</button>
      </div>
    </div>`;
  }
  return '';
}

function mealCard(m) {
  return `<div class="meal-card">
    <div class="meal-emoji">${m.emoji}</div>
    <div class="meal-body">
      <div class="meal-top">
        <div><span class="meal-time">${m.time}</span><span class="meal-label-txt">· ${m.label}</span></div>
        <div class="meal-cal-row">
          <span class="meal-cal">${m.calories}</span>
          <span class="meal-kcal">kcal</span>
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

// ─── Actions ─────────────────────────────────────────────────────────────────
function setState(patch) { Object.assign(state, patch); render(); }

function triggerCamera() {
  setState({ mode: 'camera' });
  setTimeout(() => { const el = document.getElementById('cameraInput'); if(el) el.click(); }, 50);
}
function triggerLibrary() {
  setState({ mode: 'library' });
  setTimeout(() => { const el = document.getElementById('libraryInput'); if(el) el.click(); }, 50);
}
function retryPhoto() {
  const lastMode = state.lastPhotoMode || 'library';
  setState({ mode: lastMode, errorMsg: '', previewUrl: null });
  setTimeout(() => {
    const el = document.getElementById(lastMode === 'camera' ? 'cameraInput' : 'libraryInput');
    if(el) el.click();
  }, 50);
}

function handleFile(input, source) {
  const file = input.files[0];
  input.value = '';
  if (!file) { setState({ mode: null }); return; }
  state.lastPhotoMode = source;
  analyzeImage(file);
}

async function analyzeImage(file) {
  const apiKey = state.apiKey;
  if (!apiKey) {
    setState({ mode: 'error', errorMsg: 'Please save your Anthropic API key first (see setup above).' });
    return;
  }
  const reader = new FileReader();
  reader.onload = async (e) => {
    const base64 = e.target.result.split(',')[1];
    setState({ mode: 'analyzing', previewUrl: e.target.result });
    try {
      const res = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': apiKey,
          'anthropic-version': '2023-06-01',
          'anthropic-dangerous-allow-browser': 'true',
        },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 1000,
          system: `You are a nutrition analyzer. User: 5'10", 193 lbs, active, 2300 cal/day, 160g protein goal for fat loss.
Return ONLY valid JSON, no markdown or explanation:
{"label":"short meal type","emoji":"single emoji","description":"max 80 chars","calories":number,"protein":number,"carbs":number,"fat":number}
If you see a nutrition label, use those exact numbers.`,
          messages: [{
            role: 'user',
            content: [
              { type: 'image', source: { type: 'base64', media_type: file.type, data: base64 } },
              { type: 'text', text: 'Analyze this food and return JSON nutrition data.' }
            ]
          }]
        })
      });
      const data = await res.json();
      if (data.error) throw new Error(data.error.message);
      const text = data.content?.find(b => b.type === 'text')?.text || '';
      const parsed = JSON.parse(text.replace(/```json|```/g, '').trim());
      const nowTime = new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
      const meal = {
        id: state.idCounter++,
        time: nowTime,
        label: parsed.label || 'Meal',
        description: parsed.description || '',
        calories: parsed.calories || 0,
        protein: parsed.protein || 0,
        carbs: parsed.carbs || 0,
        fat: parsed.fat || 0,
        emoji: parsed.emoji || '🍽️',
      };
      state.meals.push(meal);
      saveStorage();
      setState({ mode: null, previewUrl: null });
    } catch(err) {
      setState({ mode: 'error', errorMsg: 'Analysis failed — check your API key or try manual entry.', previewUrl: null });
    }
  };
  reader.readAsDataURL(file);
}

function saveApiKey() {
  const val = document.getElementById('apiKeyInput')?.value?.trim();
  if (!val) return;
  localStorage.setItem(API_KEY_STORAGE, val);
  setState({ apiKey: val, apiKeySaved: true });
}

function updateForm(field, value) { state.form[field] = value; render(); }

function toggleEmoji() { setState({ showEmojiPicker: !state.showEmojiPicker }); }

function pickEmoji(e) { state.form.emoji = e; setState({ showEmojiPicker: false }); }

function submitForm() {
  const f = state.form;
  if (!f.calories) return;
  const nowTime = new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
  state.meals.push({
    id: state.idCounter++,
    time: nowTime,
    label: f.label || 'Meal',
    description: f.description,
    calories: +f.calories||0,
    protein: +f.protein||0,
    carbs: +f.carbs||0,
    fat: +f.fat||0,
    emoji: f.emoji,
  });
  saveStorage();
  setState({ mode: null, form: { label:'',emoji:'🍽️',description:'',calories:'',protein:'',carbs:'',fat:'' }, showEmojiPicker: false });
}

function deleteMeal(id) {
  state.meals = state.meals.filter(m => m.id !== id);
  saveStorage();
  render();
}

function resetDay() {
  state.meals = [];
  state.idCounter = 1;
  saveStorage();
  setState({ showReset: false, mode: null });
}

// Initial render
render();
</script>

</body>
</html>
