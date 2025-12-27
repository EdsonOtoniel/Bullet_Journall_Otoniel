

```dataviewjs
dv.span("**🏋️ Heatmap — Academia 🏋️**");

// ===== CONFIG =====
const habitName = "Academia 🏋️";                 // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    green: ["#e9f8ec","#bdf0c8","#7ee19a","#35c56b","#1e8a49"]  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**🇺🇸 Heatmap — Beber 🍺**");
const habitName = "Beber 🍺";              // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    orange: ["#fff0e6", "#ffd1b3", "#ffb380", "#ff944d", "#ff751a"]
  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**💧 Heatmap — Beber água 💧**");
const habitName = "Beber água 💧";               // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    yellow: ["#fffbe6", "#fff2b3", "#ffe680", "#ffd94d", "#ffcc00"]
  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**😴 Heatmap — Dormir bem 😴**");
const habitName = "Dormir bem 😴";          // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    red: ["#ffe6e6", "#ffb3b3", "#ff8080", "#ff4d4d", "#e60000"]  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**🐍 Heatmap — Estudar Python 🐍**");
const habitName = "Estudar Python 🐍";               // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    blue: ["#e6f0ff", "#b3d1ff", "#80b3ff", "#4d94ff", "#1a75ff"]  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**📚 Heatmap — Leitura científica 📚**");
const habitName = "Leitura científica 📚";               // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    purple: ["#f1e9ff", "#d6c2ff", "#b399ff", "#8f6bff", "#6a3dff"]
  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```


```dataviewjs
dv.span("**🇺🇸 Heatmap — Estudar Inglês 🇺🇸**");
const habitName = "Estudar Inglês 🇺🇸";              // precisa bater com o texto da task
const dailyFolder = "📒 Bullet Journal/04_🗓️ Daily Log";
const habitLogPath = "📒 Bullet Journal/05_📊 Trackers/Habit Log.md";

const maxLevel = 4;                               // 1..4
const year = window.moment().year();              // ou fixe: 2025

// Cores (exemplo: uma paleta só)
const calendarData = {
  year, // opcional; remova pra autoswitch
  colors: {
    brown: ["#f5eee6", "#e0cdb3", "#c9a66b", "#a87c2a", "#7a4f00"]
  // 5 níveis (1..5). O plugin usa a lista pra intensidades.
  },
  entries: []
};

// ===== HELPERS =====
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // YYYY-MM-DD
}

// pega o melhor score (peso) do hábito num Daily
function bestScoreFromDaily(p){
  if (!p) return 0;
  let best = 0;
  for (const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if (t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*([1-4])/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return Math.min(best, maxLevel);
}

// parse do Habit Log (backup)
async function bestScoreFromHabitLog(dateStr){
  const text = await readFile(habitLogPath);
  if (typeof text !== "string") return 0;

  let currentDate = null;
  let best = 0;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){ currentDate = h[1]; continue; }

    if (currentDate !== dateStr) continue;

    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if (t){
      const done = (t[1].toLowerCase() === "x");
      const name = t[2].trim();
      const w = t[4] ? Number(t[4]) : 1;

      if (done && name.includes(habitName)){
        best = Math.max(best, w);
      }
    }
  }
  return Math.min(best, maxLevel);
}

// ===== BUILD ENTRIES =====

// 1) indexa todos os Dailies por data
const dailyPages = dv.pages(`"${dailyFolder}"`);
const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (/^\d{4}-\d{2}-\d{2}$/.test(d)) dailyByDate.set(d, p);
}

// 2) percorre todos os dias do ano e cria entry se tiver intensidade > 0
const start = window.moment(`${year}-01-01`);
const end   = window.moment(`${year}-12-31`);

for (let m = start.clone(); m.isSameOrBefore(end, "day"); m.add(1, "day")){
  const dateStr = m.format("YYYY-MM-DD");

  const p = dailyByDate.get(dateStr);
  const sDaily = bestScoreFromDaily(p);
  const sLog = await bestScoreFromHabitLog(dateStr);

  const intensity = Math.max(sDaily, sLog);

  if (intensity > 0){
    calendarData.entries.push({
      date: dateStr,                 // ✅ formato YYYY-MM-DD
      intensity: intensity,          // ✅ 1..4
      content: await dv.span(`[](${dateStr})`) // hover preview/link
    });
  }
}

// ===== RENDER =====
renderHeatmapCalendar(this.container, calendarData);
```
