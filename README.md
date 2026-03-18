<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0"/>
<title>Site Inspection</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f5f5f0;color:#1a1a1a;min-height:100vh;}
.header{background:#1a1a1a;color:white;padding:14px 16px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;}
.header h1{font-size:17px;font-weight:600;}
.header-right{font-size:12px;color:#999;}
.tab-bar{display:flex;background:white;border-bottom:1px solid #e5e5e5;position:sticky;top:49px;z-index:99;}
.tab{flex:1;padding:12px 8px;text-align:center;font-size:13px;font-weight:500;color:#888;border-bottom:2px solid transparent;cursor:pointer;}
.tab.active{color:#1a1a1a;border-bottom-color:#1a1a1a;}
.screen{display:none;padding:16px;max-width:600px;margin:0 auto;}
.screen.active{display:block;}
.area-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px;}
.area-btn{background:white;border:1.5px solid #e5e5e5;border-radius:12px;padding:16px 12px;text-align:center;cursor:pointer;transition:all 0.15s;}
.area-btn.selected{border-color:#1a1a1a;background:#f8f8f8;}
.area-btn .icon{font-size:24px;margin-bottom:6px;}
.area-btn .label{font-size:13px;font-weight:500;}
.area-btn .count{font-size:11px;color:#888;margin-top:2px;}
.card{background:white;border-radius:12px;padding:16px;margin-bottom:12px;border:1px solid #e5e5e5;}
.card-title{font-size:13px;font-weight:600;color:#888;letter-spacing:0.04em;margin-bottom:12px;}
.input-row{display:flex;gap:8px;align-items:flex-start;margin-bottom:10px;}
.input-row label{font-size:12px;color:#888;min-width:90px;padding-top:10px;}
textarea{width:100%;border:1px solid #e5e5e5;border-radius:8px;padding:10px;font-size:15px;font-family:inherit;resize:none;outline:none;line-height:1.5;}
textarea:focus{border-color:#1a1a1a;}
.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;padding:12px 18px;border-radius:10px;font-size:14px;font-weight:500;cursor:pointer;border:none;transition:all 0.15s;}
.btn-primary{background:#1a1a1a;color:white;width:100%;}
.btn-primary:active{background:#333;}
.btn-secondary{background:white;color:#1a1a1a;border:1.5px solid #1a1a1a;width:100%;}
.btn-record{background:#e63946;color:white;width:100%;}
.btn-record.recording{background:#c1121f;animation:pulse 1s infinite;}
.btn-green{background:#2d6a4f;color:white;width:100%;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.7;}}
.btn-sm{padding:8px 14px;font-size:13px;width:auto;}
.area-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;}
.area-title{font-size:20px;font-weight:700;}
.area-subtitle{font-size:13px;color:#888;margin-top:2px;}
.meas-table{width:100%;border-collapse:collapse;font-size:13px;}
.meas-table th{text-align:left;padding:8px 6px;color:#888;font-weight:500;border-bottom:1px solid #e5e5e5;font-size:11px;letter-spacing:0.04em;}
.meas-table td{padding:8px 6px;border-bottom:1px solid #f0f0f0;vertical-align:top;}
.meas-table tr:last-child td{border-bottom:none;}
.badge{display:inline-block;padding:3px 8px;border-radius:99px;font-size:11px;font-weight:500;}
.badge-blue{background:#e3f2fd;color:#1565c0;}
.badge-green{background:#e8f5e9;color:#2e7d32;}
.badge-orange{background:#fff3e0;color:#e65100;}
.badge-purple{background:#f3e5f5;color:#6a1b9a;}
.flag{background:#fff8e1;border:1px solid #ffe082;border-radius:8px;padding:10px 12px;font-size:13px;color:#f57f17;margin-bottom:8px;display:flex;align-items:flex-start;gap:8px;}
.summary-area{background:white;border-radius:12px;border:1px solid #e5e5e5;margin-bottom:12px;overflow:hidden;}
.summary-area-header{padding:14px 16px;background:#f8f8f8;border-bottom:1px solid #e5e5e5;display:flex;align-items:center;justify-content:space-between;}
.summary-area-header .name{font-size:15px;font-weight:600;}
.summary-area-body{padding:14px 16px;}
.summary-row{display:flex;justify-content:space-between;font-size:13px;padding:4px 0;border-bottom:1px solid #f5f5f5;}
.summary-row:last-child{border-bottom:none;}
.summary-row .key{color:#888;}
.summary-row .val{font-weight:500;}
.photo-strip{display:flex;gap:8px;overflow-x:auto;padding-bottom:4px;margin-top:8px;}
.photo-thumb{width:64px;height:64px;border-radius:8px;object-fit:cover;flex-shrink:0;background:#e5e5e5;border:1px solid #ddd;}
.photo-placeholder{width:64px;height:64px;border-radius:8px;background:#f0f0f0;border:1.5px dashed #ccc;display:flex;align-items:center;justify-content:center;font-size:20px;cursor:pointer;flex-shrink:0;}
.loading{display:flex;align-items:center;gap:10px;padding:16px;color:#888;font-size:14px;}
.spinner{width:20px;height:20px;border:2px solid #e5e5e5;border-top-color:#1a1a1a;border-radius:50%;animation:spin 0.8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg);}}
.ai-result{background:#f0fdf4;border:1px solid #86efac;border-radius:10px;padding:14px;margin-top:10px;}
.ai-result-title{font-size:12px;font-weight:600;color:#166534;margin-bottom:8px;display:flex;align-items:center;gap:6px;}
.section-divider{height:1px;background:#e5e5e5;margin:16px 0;}
.empty-state{text-align:center;padding:48px 24px;color:#888;}
.empty-state .icon{font-size:48px;margin-bottom:12px;}
.empty-state p{font-size:15px;}
.empty-state small{font-size:13px;color:#aaa;margin-top:6px;display:block;}
.area-list{margin-bottom:16px;}
.area-item{background:white;border-radius:10px;border:1px solid #e5e5e5;margin-bottom:8px;overflow:hidden;}
.area-item-header{padding:12px 14px;display:flex;align-items:center;justify-content:space-between;cursor:pointer;}
.area-item-header .left{display:flex;align-items:center;gap:10px;}
.area-item-header .icon{font-size:20px;}
.area-item-header .name{font-size:15px;font-weight:600;}
.area-item-header .meta{font-size:12px;color:#888;margin-top:1px;}
.area-item-header .right{display:flex;align-items:center;gap:8px;}
.status-dot{width:8px;height:8px;border-radius:50%;}
.dot-green{background:#22c55e;}
.dot-yellow{background:#f59e0b;}
.worksheet-section{background:white;border-radius:12px;border:1px solid #e5e5e5;margin-bottom:10px;overflow:hidden;}
.ws-header{padding:14px 16px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid #e5e5e5;}
.ws-header .left{display:flex;align-items:center;gap:10px;}
.ws-badge{font-size:10px;background:#e3f2fd;color:#1565c0;padding:2px 8px;border-radius:99px;font-weight:500;}
.ws-body{padding:14px 16px;}
.ws-row{display:flex;justify-content:space-between;font-size:13px;padding:3px 0;}
.ws-row .key{color:#888;}
.ws-row .val{font-weight:500;}
.toggle-row{display:flex;gap:8px;margin-top:10px;}
.toggle-btn{flex:1;padding:9px;border-radius:8px;font-size:13px;font-weight:500;cursor:pointer;border:1.5px solid #e5e5e5;background:white;color:#888;transition:all 0.15s;}
.toggle-btn.keep{border-color:#2d6a4f;background:#e8f5e9;color:#2d6a4f;}
.toggle-btn.remove{border-color:#e63946;background:#fee2e2;color:#e63946;}
input[type=file]{display:none;}
.voice-hint{font-size:12px;color:#888;text-align:center;margin-top:8px;}
.transcript-box{background:#f8f8f8;border-radius:8px;padding:12px;font-size:14px;color:#1a1a1a;margin-top:10px;min-height:60px;line-height:1.6;}
.section-label{font-size:11px;font-weight:600;color:#888;letter-spacing:0.05em;margin-bottom:8px;}
</style>
</head>
<body>

<div class="header">
  <h1>Site Inspection</h1>
  <div class="header-right" id="inspection-date"></div>
</div>

<div class="tab-bar">
  <div class="tab active" onclick="showTab('capture')">Capture</div>
  <div class="tab" onclick="showTab('areas')">Areas</div>
  <div class="tab" onclick="showTab('worksheet')">Worksheet</div>
</div>

<!-- CAPTURE TAB -->
<div class="screen active" id="screen-capture">

  <div style="margin-bottom:16px;">
    <div class="section-label">SELECT AREA</div>
    <div class="area-grid">
      <div class="area-btn" onclick="selectArea('basement')" id="btn-basement">
        <div class="icon">🏚️</div>
        <div class="label">Basement</div>
        <div class="count" id="count-basement">0 items</div>
      </div>
      <div class="area-btn" onclick="selectArea('attic')" id="btn-attic">
        <div class="icon">🏠</div>
        <div class="label">Attic</div>
        <div class="count" id="count-attic">0 items</div>
      </div>
      <div class="area-btn" onclick="selectArea('crawl')" id="btn-crawl">
        <div class="icon">🕳️</div>
        <div class="label">Crawl Space</div>
        <div class="count" id="count-crawl">0 items</div>
      </div>
      <div class="area-btn" onclick="selectArea('pole-barn')" id="btn-pole-barn">
        <div class="icon">🏗️</div>
        <div class="label">Pole Barn</div>
        <div class="count" id="count-pole-barn">0 items</div>
      </div>
      <div class="area-btn" onclick="selectArea('garage')" id="btn-garage">
        <div class="icon">🚗</div>
        <div class="label">Garage</div>
        <div class="count" id="count-garage">0 items</div>
      </div>
      <div class="area-btn" onclick="selectArea('other')" id="btn-other">
        <div class="icon">📍</div>
        <div class="label">Other</div>
        <div class="count" id="count-other">0 items</div>
      </div>
    </div>
  </div>

  <div id="capture-panel" style="display:none;">
    <div class="area-header">
      <div>
        <div class="area-title" id="selected-area-title">Basement</div>
        <div class="area-subtitle">Capture media and speak measurements</div>
      </div>
      <button class="btn btn-sm" style="background:#f0f0f0;color:#333;" onclick="clearArea()">✕ Close</button>
    </div>

    <!-- Photos -->
    <div class="card">
      <div class="card-title">PHOTOS & VIDEO</div>
      <div class="photo-strip" id="photo-strip">
        <div class="photo-placeholder" onclick="document.getElementById('photo-input').click()">+</div>
      </div>
      <input type="file" id="photo-input" accept="image/*,video/*" capture="environment" multiple onchange="handlePhotos(this)"/>
      <div style="margin-top:10px;display:flex;gap:8px;">
        <button class="btn btn-secondary btn-sm" onclick="document.getElementById('photo-input').click()">📷 Add Photo/Video</button>
      </div>
    </div>

    <!-- Measurements -->
    <div class="card">
      <div class="card-title">MEASUREMENTS</div>
      <div id="voice-btn-wrap">
        <button class="btn btn-record" id="voice-btn" onclick="toggleVoice()">
          🎤 Tap & Speak Measurements
        </button>
        <div class="voice-hint">Say: "Ceiling 20x20, wall height 14, lengths 12 7 9 5"</div>
      </div>
      <div id="transcript-wrap" style="display:none;">
        <div class="transcript-box" id="transcript-text"></div>
        <div style="display:flex;gap:8px;margin-top:10px;">
          <button class="btn btn-secondary btn-sm" onclick="retryVoice()">🔄 Re-record</button>
          <button class="btn btn-sm" style="background:#1a1a1a;color:white;flex:1;" onclick="parseMeasurements()">Parse with AI →</button>
        </div>
      </div>
      <div id="parse-loading" style="display:none;" class="loading">
        <div class="spinner"></div>
        AI is reading your measurements...
      </div>
      <div id="parse-result" style="display:none;"></div>

      <div class="section-divider"></div>
      <div class="section-label">OR TYPE MEASUREMENTS</div>
      <textarea id="manual-measurements" rows="4" placeholder="Ceiling 20x20&#10;Wall height 14&#10;Lengths 12 7 9 5&#10;Next section ceiling 13x20..."></textarea>
      <button class="btn btn-primary" style="margin-top:10px;" onclick="parseManual()">Parse with AI →</button>
    </div>

    <!-- Notes -->
    <div class="card">
      <div class="card-title">NOTES & FLAGS</div>
      <textarea id="area-notes" rows="3" placeholder="Anything the crew needs to know — access point, obstacles, moisture, protect floors..."></textarea>
      <button class="btn btn-record" style="margin-top:8px;" id="note-voice-btn" onclick="toggleNoteVoice()">
        🎤 Speak Notes
      </button>
    </div>

    <button class="btn btn-green" onclick="saveArea()" style="margin-bottom:24px;">
      ✓ Save This Area
    </button>
  </div>

  <div id="no-area-msg" style="text-align:center;padding:32px 16px;color:#888;">
    <div style="font-size:40px;margin-bottom:12px;">☝️</div>
    <div style="font-size:15px;font-weight:500;">Select an area above to begin</div>
    <div style="font-size:13px;margin-top:6px;">Tap any area to capture photos and measurements</div>
  </div>

</div>

<!-- AREAS TAB -->
<div class="screen" id="screen-areas">
  <div id="areas-empty" class="empty-state">
    <div class="icon">📋</div>
    <p>No areas captured yet</p>
    <small>Go to Capture tab to start your inspection</small>
  </div>
  <div id="areas-list" class="area-list"></div>
</div>

<!-- WORKSHEET TAB -->
<div class="screen" id="screen-worksheet">
  <div style="margin-bottom:16px;">
    <div style="font-size:15px;font-weight:600;margin-bottom:4px;">Estimate Worksheet Preview</div>
    <div style="font-size:13px;color:#888;">Review what will pre-populate. Keep or remove each section.</div>
  </div>
  <div id="worksheet-empty" class="empty-state">
    <div class="icon">📊</div>
    <p>Complete the inspection first</p>
    <small>Captured areas will appear here ready to send to the estimate</small>
  </div>
  <div id="worksheet-list"></div>
  <div id="worksheet-send" style="display:none;">
    <button class="btn btn-green" onclick="sendToWorksheet()">
      ✓ Send to Estimate Worksheet
    </button>
    <div style="font-size:12px;color:#888;text-align:center;margin-top:8px;">Only kept sections will be sent</div>
  </div>
</div>

<script>
const AREAS = {
  'basement': {label:'Basement', icon:'🏚️'},
  'attic': {label:'Attic', icon:'🏠'},
  'crawl': {label:'Crawl Space', icon:'🕳️'},
  'pole-barn': {label:'Pole Barn', icon:'🏗️'},
  'garage': {label:'Garage', icon:'🚗'},
  'other': {label:'Other Area', icon:'📍'},
};

let currentArea = null;
let capturedAreas = {};
let recognition = null;
let isRecording = false;
let isNoteRecording = false;
let currentTranscript = '';

document.getElementById('inspection-date').textContent = new Date().toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'});

function showTab(tab) {
  document.querySelectorAll('.tab').forEach((t,i) => {
    t.classList.toggle('active', ['capture','areas','worksheet'][i] === tab);
  });
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-'+tab).classList.add('active');
  if (tab === 'areas') renderAreas();
  if (tab === 'worksheet') renderWorksheet();
}

function selectArea(area) {
  currentArea = area;
  document.querySelectorAll('.area-btn').forEach(b => b.classList.remove('selected'));
  document.getElementById('btn-'+area).classList.add('selected');
  document.getElementById('capture-panel').style.display = 'block';
  document.getElementById('no-area-msg').style.display = 'none';
  document.getElementById('selected-area-title').textContent = AREAS[area].icon + ' ' + AREAS[area].label;
  document.getElementById('transcript-wrap').style.display = 'none';
  document.getElementById('voice-btn-wrap').style.display = 'block';
  document.getElementById('parse-result').style.display = 'none';
  document.getElementById('manual-measurements').value = '';
  document.getElementById('area-notes').value = '';
  const strip = document.getElementById('photo-strip');
  strip.innerHTML = '<div class="photo-placeholder" onclick="document.getElementById(\'photo-input\').click()">+</div>';
  if (capturedAreas[area]) {
    const d = capturedAreas[area];
    if (d.notes) document.getElementById('area-notes').value = d.notes;
    if (d.parsedText) document.getElementById('manual-measurements').value = d.parsedText;
  }
}

function clearArea() {
  currentArea = null;
  document.querySelectorAll('.area-btn').forEach(b => b.classList.remove('selected'));
  document.getElementById('capture-panel').style.display = 'none';
  document.getElementById('no-area-msg').style.display = 'block';
}

function handlePhotos(input) {
  const strip = document.getElementById('photo-strip');
  Array.from(input.files).forEach(file => {
    const reader = new FileReader();
    reader.onload = e => {
      const img = document.createElement('img');
      img.className = 'photo-thumb';
      img.src = e.target.result;
      strip.insertBefore(img, strip.lastElementChild);
    };
    reader.readAsDataURL(file);
  });
}

function toggleVoice() {
  if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
    alert('Voice input not supported in this browser. Please type your measurements.');
    return;
  }
  if (isRecording) {
    recognition && recognition.stop();
    return;
  }
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SR();
  recognition.continuous = true;
  recognition.interimResults = true;
  recognition.lang = 'en-US';
  let finalTranscript = '';
  recognition.onstart = () => {
    isRecording = true;
    const btn = document.getElementById('voice-btn');
    btn.textContent = '⏹ Stop Recording';
    btn.style.background = '#c1121f';
  };
  recognition.onresult = e => {
    let interim = '';
    for (let i = e.resultIndex; i < e.results.length; i++) {
      if (e.results[i].isFinal) finalTranscript += e.results[i][0].transcript + ' ';
      else interim = e.results[i][0].transcript;
    }
    document.getElementById('transcript-text').textContent = finalTranscript + interim;
    currentTranscript = finalTranscript + interim;
  };
  recognition.onend = () => {
    isRecording = false;
    const btn = document.getElementById('voice-btn');
    btn.textContent = '🎤 Tap & Speak Measurements';
    btn.style.background = '#e63946';
    if (currentTranscript.trim()) {
      document.getElementById('voice-btn-wrap').style.display = 'none';
      document.getElementById('transcript-wrap').style.display = 'block';
      document.getElementById('transcript-text').textContent = currentTranscript;
    }
  };
  recognition.onerror = () => {
    isRecording = false;
    alert('Could not access microphone. Please type your measurements instead.');
  };
  recognition.start();
}

function retryVoice() {
  currentTranscript = '';
  document.getElementById('transcript-wrap').style.display = 'none';
  document.getElementById('voice-btn-wrap').style.display = 'block';
  document.getElementById('parse-result').style.display = 'none';
}

function toggleNoteVoice() {
  if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
    alert('Voice input not supported. Please type your notes.');
    return;
  }
  if (isNoteRecording) {
    recognition && recognition.stop();
    return;
  }
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  const rec = new SR();
  rec.continuous = true;
  rec.interimResults = false;
  rec.lang = 'en-US';
  let noteFinal = '';
  rec.onstart = () => {
    isNoteRecording = true;
    document.getElementById('note-voice-btn').textContent = '⏹ Stop';
    document.getElementById('note-voice-btn').style.background = '#c1121f';
  };
  rec.onresult = e => {
    for (let i = e.resultIndex; i < e.results.length; i++) {
      if (e.results[i].isFinal) noteFinal += e.results[i][0].transcript + ' ';
    }
  };
  rec.onend = () => {
    isNoteRecording = false;
    document.getElementById('note-voice-btn').textContent = '🎤 Speak Notes';
    document.getElementById('note-voice-btn').style.background = '#e63946';
    const existing = document.getElementById('area-notes').value;
    document.getElementById('area-notes').value = (existing ? existing + '\n' : '') + noteFinal.trim();
  };
  rec.start();
  recognition = rec;
}

async function parseMeasurements() {
  const text = currentTranscript || document.getElementById('transcript-text').textContent;
  if (!text.trim()) return;
  document.getElementById('transcript-wrap').style.display = 'none';
  document.getElementById('parse-loading').style.display = 'flex';
  document.getElementById('parse-result').style.display = 'none';
  try {
    const result = await callClaude(text);
    document.getElementById('parse-loading').style.display = 'none';
    document.getElementById('parse-result').style.display = 'block';
    document.getElementById('parse-result').innerHTML = result;
    if (currentArea) {
      if (!capturedAreas[currentArea]) capturedAreas[currentArea] = {};
      capturedAreas[currentArea].parsedText = text;
      capturedAreas[currentArea].parsedHTML = result;
    }
  } catch(e) {
    document.getElementById('parse-loading').style.display = 'none';
    document.getElementById('transcript-wrap').style.display = 'block';
    alert('Could not parse measurements. Please check your connection and try again.');
  }
}

async function parseManual() {
  const text = document.getElementById('manual-measurements').value.trim();
  if (!text) { alert('Please enter your measurements first.'); return; }
  document.getElementById('parse-loading').style.display = 'flex';
  document.getElementById('parse-result').style.display = 'none';
  try {
    const result = await callClaude(text);
    document.getElementById('parse-loading').style.display = 'none';
    document.getElementById('parse-result').style.display = 'block';
    document.getElementById('parse-result').innerHTML = result;
    if (currentArea) {
      if (!capturedAreas[currentArea]) capturedAreas[currentArea] = {};
      capturedAreas[currentArea].parsedText = text;
      capturedAreas[currentArea].parsedHTML = result;
    }
  } catch(e) {
    document.getElementById('parse-loading').style.display = 'none';
    alert('Could not parse measurements. Please check your connection and try again.');
  }
}

async function callClaude(measurementText) {
  const areaName = currentArea ? AREAS[currentArea].label : 'Unknown Area';
  const prompt = `You are parsing spoken/typed measurements from a home insulation contractor doing a site inspection in the ${areaName}.

The contractor said or typed: "${measurementText}"

Parse this into a structured measurement table. Common patterns:
- "Ceiling 20x20" or "ceiling is 20 by 20" = ceiling section, 400 SF
- "Wall height 14" or "walls are 14 feet tall" = wall height in feet
- "Lengths 12 7 9 5" or "wall lengths are 12 7 9 5" = individual wall lengths in feet
- "Next section" = start a new section
- Numbers without context after wall heights = wall lengths

Calculate:
- Ceiling SF = length × width
- Wall SF = height × (sum of all lengths)
- Total SF per section

Also note any flags (moisture, tight access, obstacles, unusual items mentioned).

Return ONLY a clean HTML snippet (no markdown, no backticks) using this exact structure:
<div class="ai-result">
<div class="ai-result-title">✓ AI Parsed Measurements</div>
<table class="meas-table">
<thead><tr><th>Section</th><th>Type</th><th>Dimensions</th><th>SF</th></tr></thead>
<tbody>
[rows here - one row per measurement section]
</tbody>
</table>
[if any flags detected, add: <div class="flag">⚠ [flag text]</div>]
<div style="margin-top:10px;font-size:12px;color:#888;">Total estimated SF: [total]</div>
</div>`;

  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 1000,
      messages: [{ role: 'user', content: prompt }]
    })
  });
  const data = await response.json();
  const text2 = data.content?.[0]?.text || '';
  return text2.replace(/```html/g,'').replace(/```/g,'').trim();
}

function saveArea() {
  if (!currentArea) return;
  const notes = document.getElementById('area-notes').value.trim();
  const parsedHTML = document.getElementById('parse-result').innerHTML;
  const parsedText = document.getElementById('manual-measurements').value || currentTranscript;
  const photos = Array.from(document.querySelectorAll('#photo-strip img')).map(i => i.src);
  if (!capturedAreas[currentArea]) capturedAreas[currentArea] = {};
  capturedAreas[currentArea] = {
    ...capturedAreas[currentArea],
    area: currentArea,
    label: AREAS[currentArea].label,
    icon: AREAS[currentArea].icon,
    notes,
    parsedHTML,
    parsedText,
    photos,
    savedAt: new Date().toLocaleTimeString(),
    keep: true,
  };
  updateAreaCounts();
  const btn = document.getElementById('btn-'+currentArea);
  btn.querySelector('.count').textContent = '✓ Saved';
  btn.style.borderColor = '#2d6a4f';
  btn.style.background = '#e8f5e9';
  clearArea();
  showTab('areas');
}

function updateAreaCounts() {
  Object.keys(AREAS).forEach(a => {
    const el = document.getElementById('count-'+a);
    if (el) {
      el.textContent = capturedAreas[a] ? '✓ Saved' : '0 items';
    }
  });
}

function renderAreas() {
  const list = document.getElementById('areas-list');
  const empty = document.getElementById('areas-empty');
  const keys = Object.keys(capturedAreas);
  if (keys.length === 0) {
    empty.style.display = 'block';
    list.style.display = 'none';
    return;
  }
  empty.style.display = 'none';
  list.style.display = 'block';
  list.innerHTML = keys.map(k => {
    const d = capturedAreas[k];
    return `<div class="area-item">
      <div class="area-item-header">
        <div class="left">
          <div class="icon">${d.icon}</div>
          <div>
            <div class="name">${d.label}</div>
            <div class="meta">Saved at ${d.savedAt} · ${d.photos.length} photo(s)</div>
          </div>
        </div>
        <div class="right">
          <div class="status-dot dot-green"></div>
          <button class="btn btn-sm" style="background:#f0f0f0;color:#333;font-size:12px;" onclick="editArea('${k}')">Edit</button>
        </div>
      </div>
      ${d.parsedHTML ? `<div style="padding:0 14px 12px;">${d.parsedHTML}</div>` : ''}
      ${d.notes ? `<div style="padding:0 14px 12px;font-size:13px;color:#555;background:#fffbf0;border-top:1px solid #f0e8d0;"><b>Notes:</b> ${d.notes}</div>` : ''}
    </div>`;
  }).join('');
}

function editArea(area) {
  showTab('capture');
  selectArea(area);
  if (capturedAreas[area]?.parsedHTML) {
    document.getElementById('parse-result').style.display = 'block';
    document.getElementById('parse-result').innerHTML = capturedAreas[area].parsedHTML;
  }
}

function renderWorksheet() {
  const list = document.getElementById('worksheet-list');
  const empty = document.getElementById('worksheet-empty');
  const send = document.getElementById('worksheet-send');
  const keys = Object.keys(capturedAreas);
  if (keys.length === 0) {
    empty.style.display = 'block';
    list.style.display = 'none';
    send.style.display = 'none';
    return;
  }
  empty.style.display = 'none';
  list.style.display = 'block';
  send.style.display = 'block';
  list.innerHTML = keys.map(k => {
    const d = capturedAreas[k];
    const isKeep = d.keep !== false;
    return `<div class="worksheet-section">
      <div class="ws-header">
        <div class="left">
          <span style="font-size:18px;">${d.icon}</span>
          <div>
            <div style="font-size:15px;font-weight:600;">${d.label}</div>
            <div style="font-size:12px;color:#888;">${d.photos.length} photo(s) · ${d.notes ? 'Has notes' : 'No notes'}</div>
          </div>
        </div>
        <span class="ws-badge">From inspection</span>
      </div>
      <div class="ws-body">
        ${d.parsedHTML || '<div style="color:#888;font-size:13px;">No measurements captured</div>'}
        <div class="toggle-row">
          <button class="toggle-btn ${isKeep?'keep':''}" onclick="toggleKeep('${k}','keep',this)">✓ Keep</button>
          <button class="toggle-btn ${!isKeep?'remove':''}" onclick="toggleKeep('${k}','remove',this)">✕ Remove</button>
        </div>
      </div>
    </div>`;
  }).join('');
}

function toggleKeep(area, action, btn) {
  capturedAreas[area].keep = action === 'keep';
  const parent = btn.closest('.toggle-row');
  parent.querySelectorAll('.toggle-btn').forEach(b => {
    b.classList.remove('keep','remove');
  });
  if (action === 'keep') {
    parent.children[0].classList.add('keep');
  } else {
    parent.children[1].classList.add('remove');
  }
}

function sendToWorksheet() {
  const kept = Object.values(capturedAreas).filter(a => a.keep !== false);
  if (kept.length === 0) { alert('Please keep at least one section.'); return; }
  const summary = kept.map(a => `${a.icon} ${a.label}`).join(', ');
  alert(`✓ Sent to Estimate Worksheet!\n\nSections: ${summary}\n\nIn the real app, these would pre-populate the estimator with measurements and media attached.`);
}
</script>
</body>
</html>
