---
type: habit_dashboard
daily_folder: "📒 Bullet Journal/04_🗓️ Daily Log"
master_file: "📒 Bullet Journal/05_📊 Trackers/Habits (Master).md"
habit_log_file: "📒 Bullet Journal/05_📊 Trackers/Habit Log.md"
---

# 📊 Habit Dashboard — <% tp.date.now("MMMM YYYY") %>

> Registro: Daily (padrão) + Habit Log (backup)  
> Formato do hábito: `- [x] Nome ::peso(1..4)`

---

## 🔥 Heatmap por hábito (0–4)

```dataviewjs
// ==========================
// CONFIG via frontmatter
// ==========================
const cfg = dv.current();

// Se você quiser fixar o mês manualmente no Monthly, coloque no frontmatter:
// month: "2025-12"
const monthStr = String(cfg.month ?? window.moment().format("YYYY-MM"));

const dailyFolder  = String(cfg.daily_folder);
const masterPath   = String(cfg.master_file);
const habitLogPath = String(cfg.habit_log_file);

// ==========================
// helpers
// ==========================
function pad2(n){ return String(n).padStart(2,"0"); }
function parseMonth(s){ const [y,m]=s.split("-").map(Number); return {y,m}; }
function daysInMonth(y,m){ return new Date(y,m,0).getDate(); }
function dateKey(y,m,d){ return `${y}-${pad2(m)}-${pad2(d)}`; }

function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10);
}
function isISODate(s){ return /^\d{4}-\d{2}-\d{2}$/.test(s); }

// escala: 0 vazio, 1 vermelho, 2 laranja, 3 amarelo, 4 verde
function cell(score){
  if (score<=0) return "⬜";
  if (score===1) return "🟥";
  if (score===2) return "🟧";
  if (score===3) return "🟨";
  return "🟩"; // 4
}

// leitura robusta (evita dv.io.load com emojis)
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

// ==========================
// MASTER: áreas -> hábitos
// formato esperado por linha:
// - Academia 🏋️ ::4
// ==========================
async function loadMasterAreas(path){
  const text = await readFile(path);
  if (typeof text !== "string") return null;

  const areas = new Map(); // area -> [{name,w}]
  let cur = null;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(.*)\s*$/);
    if (h){
      cur = h[1].trim();
      if(!areas.has(cur)) areas.set(cur, []);
      continue;
    }

    // aceita "- Nome ::4" com espaços
    const it = line.match(/^- (.+?)\s*::\s*(\d)\s*$/);
    if (it && cur){
      areas.get(cur).push({name: it[1].trim(), w: parseInt(it[2],10)});
    }
  }
  return areas;
}

// ==========================
// HABIT LOG: date -> [{name,w,done}]
// ==========================
async function parseHabitLog(path){
  const text = await readFile(path);
  if (typeof text !== "string") return new Map();

  const map = new Map(); // date -> entries[]
  let currentDate = null;

  for (const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if (h){
      currentDate = h[1];
      if (!map.has(currentDate)) map.set(currentDate, []);
      continue;
    }

    // - [x] Nome ::4 (pode ter texto depois)
    const t = line.match(/^- \[( |x|X)\]\s+(.*)$/);
    if (t && currentDate){
      const done = (String(t[1]).toLowerCase() === "x");
      const body = t[2] ?? "";

      // extrai nome e peso
      const mW = body.match(/::\s*([1-4])/);
      const w  = mW ? parseInt(mW[1],10) : 1;

      // nome = corpo sem o "::N ..." (apenas pra facilitar "includes")
      const name = body.replace(/::\s*[1-4].*$/, "").trim();

      map.get(currentDate).push({name, w, done, raw: body});
    }
  }
  return map;
}

// ==========================
// DAILY: best score por texto do arquivo
// ==========================
async function bestScoreFromDailyFile(filePath, habitName){
  const text = await readFile(filePath);
  if (typeof text !== "string") return 0;

  let best = 0;

  for (const line of text.split("\n")){
    // task marcada
    const mTask = line.match(/^- \[[xX]\]\s+(.*)$/);
    if (!mTask) continue;

    const body = mTask[1];

    // precisa conter o nome do hábito
    if (!body.includes(habitName)) continue;

    // pega ::1..4 mesmo que tenha texto depois
    const mW = body.match(/::\s*([1-4])/);
    const w  = mW ? parseInt(mW[1],10) : 1;

    best = Math.max(best, w);
  }

  return best;
}

// ==========================
// LOG: best score no dia
// ==========================
function bestScoreFromLog(logMap, dateStr, habitName){
  const entries = logMap.get(dateStr) ?? [];
  let best = 0;

  for (const e of entries){
    if (!e.done) continue;

    // tenta casar tanto pelo nome limpo quanto pelo raw
    if ((e.name && habitName.includes(e.name)) || (e.raw && e.raw.includes(habitName)) || (e.name && e.name.includes(habitName))){
      best = Math.max(best, e.w);
    }
  }
  return best;
}

// ==========================
// Carregar fontes
// ==========================
const areas = await loadMasterAreas(masterPath);
if (!areas){
  dv.paragraph("⚠️ Não consegui ler o Habits (Master). Confira o caminho em `master_file`.");
  dv.paragraph("`" + masterPath + "`");
  return;
}

const habitLog = await parseHabitLog(habitLogPath);

// ==========================
// Montar índice de Dailies do mês
// ==========================
const {y,m} = parseMonth(monthStr);
const nDays = daysInMonth(y,m);
const monthPrefix = `${y}-${pad2(m)}-`;

const dailyPages = dv.pages(`"${dailyFolder}"`)
  .where(p => String(getDateFromPage(p)).startsWith(monthPrefix));

const dailyByDate = new Map();
for (const p of dailyPages){
  const d = getDateFromPage(p);
  if (isISODate(d)) dailyByDate.set(d, p);
}

// score final = max(Daily, HabitLog)
async function scoreFor(dateStr, habitName){
  const p = dailyByDate.get(dateStr);

  const sDaily = p ? await bestScoreFromDailyFile(p.file.path, habitName) : 0;
  const sLog   = bestScoreFromLog(habitLog, dateStr, habitName);

  return Math.min(Math.max(sDaily, sLog), 4);
}

// ==========================
// Tabela
// ==========================
const header = ["Área / Hábito", ...Array.from({length:nDays}, (_,i)=>String(i+1))];
const rows = [];

for (const [area, items] of areas.entries()){
  for (const it of items){
    const row = [`${area} — ${it.name}`];
    for (let d=1; d<=nDays; d++){
      const s = await scoreFor(dateKey(y,m,d), it.name);
      row.push(cell(s));
    }
    rows.push(row);
  }
}

dv.table(header, rows);

```

--- 
## 📈 Progresso + Streak (por hábito)
```dataviewjs
const cfg = dv.current();
const monthStr = window.moment().format("YYYY-MM");
const dailyFolder = String(cfg.daily_folder ?? "");
const masterPath = String(cfg.master_file ?? "");
const habitLogPath = String(cfg.habit_log_file ?? "");

function pad2(n){ return String(n).padStart(2,"0"); }
function parseMonth(s){
  const m = String(s).match(/^(\d{4})-(\d{2})$/);
  if(!m) return null;
  return {y: Number(m[1]), m: Number(m[2])};
}
function daysInMonth(y,m){ return new Date(y,m,0).getDate(); }
function dateKey(y,m,d){ return `${y}-${pad2(m)}-${pad2(d)}`; }

// leitura robusta (emoji OK)
async function readFile(path){
  try { return await app.vault.adapter.read(path); }
  catch(e){ return null; }
}

// carrega hábitos do Master
async function loadHabits(path){
  const text = await readFile(path);
  if(typeof text !== "string") return null;
  const habits = [];
  let area = "Geral";
  for(const line of text.split("\n")){
    const h = line.match(/^##\s+(.*)\s*$/);
    if(h){ area = h[1].trim(); continue; }
    const it = line.match(/^- (.+?)\s*::\s*(\d)\s*$/);
    if(it){
      habits.push({area, name: it[1].trim()});
    }
  }
  return habits;
}

// parse do Habit Log
async function parseHabitLog(path){
  const text = await readFile(path);
  if(typeof text !== "string") return new Map();
  const map = new Map(); // date -> entries
  let currentDate = null;
  for(const line of text.split("\n")){
    const h = line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if(h){ currentDate = h[1]; if(!map.has(currentDate)) map.set(currentDate, []); continue; }
    const t = line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if(t && currentDate){
      map.get(currentDate).push({
        done: (t[1].toLowerCase()==="x"),
        name: t[2].trim(),
        w: t[4] ? Number(t[4]) : 1
      });
    }
  }
  return map;
}

// pega date de uma página Daily
function getDateFromPage(p){
  // tenta: date:: (inline) -> frontmatter -> nome do arquivo
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10); // "YYYY-MM-DD"
}

function bestScoreFromDaily(p, habitName){
  if(!p) return 0;
  let best = 0;
  for(const t of (p.file.tasks ?? [])){
    const txt = t.text ?? "";
    if(t.completed && txt.includes(habitName)){
      const m = txt.match(/::\s*(\d)\s*$/);
      const w = m ? Number(m[1]) : 1;
      best = Math.max(best, w);
    }
  }
  return best;
}
function bestScoreFromLog(logMap, dateStr, habitName){
  const entries = logMap.get(dateStr) ?? [];
  let best = 0;
  for(const e of entries){
    if(e.done && e.name.includes(habitName)) best = Math.max(best, e.w);
  }
  return best;
}

// ---- validação do mês
const mm = parseMonth(monthStr);
if(!mm){
  dv.paragraph(`⚠️ month inválido no frontmatter: "${monthStr}". Use "YYYY-MM" (ex.: "2025-12").`);
  return;
}
const {y,m} = mm;
const nDays = daysInMonth(y,m);
const monthPrefix = `${y}-${pad2(m)}-`;

// ---- carregar dados
const habits = await loadHabits(masterPath);
if(!habits){
  dv.paragraph("⚠️ Não consegui ler o Master (master_file):");
  dv.paragraph("`" + masterPath + "`");
  return;
}
const habitLog = await parseHabitLog(habitLogPath);

// pega todas as páginas dentro da pasta daily (inclui subpastas)
const dailyPages = dv.pages(`"${dailyFolder}"`)
  .where(p => String(getDateFromPage(p)).startsWith(monthPrefix));

const dailyByDate = new Map();
for(const p of dailyPages){
  dailyByDate.set(getDateFromPage(p), p);
}

function scoreFor(dateStr, habitName){
  const p = dailyByDate.get(dateStr);
  return Math.max(bestScoreFromDaily(p, habitName), bestScoreFromLog(habitLog, dateStr, habitName));
}

function streakFor(habitName){
  let best=0, run=0;
  for(let d=1; d<=nDays; d++){
    const done = scoreFor(dateKey(y,m,d), habitName) > 0;
    if(done){ run++; best=Math.max(best,run); } else run=0;
  }
  let current=0;
  for(let d=nDays; d>=1; d--){
    const done = scoreFor(dateKey(y,m,d), habitName) > 0;
    if(done) current++; else break;
  }
  return {current, best};
}

function bar(pct){
  const blocks = 18;
  const filled = Math.round((pct/100)*blocks);
  return "█".repeat(filled) + "░".repeat(Math.max(0, blocks-filled)) + ` ${pct.toFixed(0)}%`;
}

// ---- tabela
const rows=[];
for(const h of habits){
  let doneDays=0, points=0;
  for(let d=1; d<=nDays; d++){
    const s = scoreFor(dateKey(y,m,d), h.name);
    if(s>0) doneDays++;
    points += s;
  }
  const pct = (doneDays/nDays)*100;
  const st = streakFor(h.name);
  rows.push([h.area, h.name, `${doneDays}/${nDays}`, bar(pct), points, `🔥 ${st.current} | 🏆 ${st.best}`]);
}
rows.sort((a,b)=>b[4]-a[4]);
dv.table(["Área","Hábito","Dias","Progresso","Pontos","Streak (atual | melhor)"], rows);

```
---

## 🧾 Sumário mensal (por área)
```dataviewjs
const cfg = dv.current();
const monthStr = window.moment().format("YYYY-MM");
const dailyFolder = String(cfg.daily_folder);
const masterPath = String(cfg.master_file);
const habitLogPath = String(cfg.habit_log_file);

function pad2(n){ return String(n).padStart(2,"0"); }
function parseMonth(s){ const [y,m]=s.split("-").map(Number); return {y,m}; }
function daysInMonth(y,m){ return new Date(y,m,0).getDate(); }
function dateKey(y,m,d){ return `${y}-${pad2(m)}-${pad2(d)}`; }
function getDateFromPage(p){
  const d = p.date ?? p.file.frontmatter?.date ?? p.file.name;
  return String(d).slice(0,10);
}
async function readFile(path){
  try { return await app.vault.adapter.read(path); } catch(e){ return null; }
}
async function loadAreas(path){
  const text = await readFile(path);
  if(typeof text !== "string") return null;
  const areas = new Map();
  let cur=null;
  for(let line of text.split("\n")){
    const h=line.match(/^##\s+(.*)\s*$/);
    if(h){cur=h[1].trim(); if(!areas.has(cur)) areas.set(cur, []); continue;}
    const it=line.match(/^- (.+?)\s*::\s*(\d)\s*$/);
    if(it && cur) areas.get(cur).push(it[1].trim());
  }
  return areas;
}
async function parseHabitLog(path){
  const text = await readFile(path);
  if(typeof text !== "string") return new Map();
  const map = new Map();
  let currentDate=null;
  for(const line of text.split("\n")){
    const h=line.match(/^##\s+(\d{4}-\d{2}-\d{2})\s*$/);
    if(h){currentDate=h[1]; if(!map.has(currentDate)) map.set(currentDate, []); continue;}
    const t=line.match(/^- \[( |x)\]\s+(.*?)\s*(::\s*(\d)\s*)?$/i);
    if(t && currentDate){
      map.get(currentDate).push({done:(t[1].toLowerCase()==="x"), name:t[2].trim(), w:(t[4]?parseInt(t[4],10):1)});
    }
  }
  return map;
}
function bestScoreFromDaily(p, habitName){
  if(!p) return 0;
  let best=0;
  for(const t of (p.file.tasks ?? [])){
    const txt=t.text ?? "";
    if(txt.includes(habitName) && t.completed){
      const m=txt.match(/::\s*(\d)\s*$/);
      const w=m?parseInt(m[1],10):1;
      best=Math.max(best,w);
    }
  }
  return best;
}
function bestScoreFromLog(logMap, dateStr, habitName){
  const entries=logMap.get(dateStr) ?? [];
  let best=0;
  for(const e of entries){
    if(e.done && e.name.includes(habitName)) best=Math.max(best,e.w);
  }
  return best;
}

const areas = await loadAreas(masterPath);
if(!areas){
  dv.paragraph("⚠️ Não consegui ler o Master (master_file).");
  dv.paragraph("`" + masterPath + "`");
  return;
}
const habitLog = await parseHabitLog(habitLogPath);

const {y,m}=parseMonth(monthStr);
const nDays=daysInMonth(y,m);
const monthPrefix=`${y}-${pad2(m)}-`;

const dailyPages = dv.pages(`"${dailyFolder}"`)
  .where(p => String(getDateFromPage(p)).startsWith(monthPrefix));
const dailyByDate = new Map();
for(const p of dailyPages) dailyByDate.set(getDateFromPage(p), p);

function scoreFor(dateStr, habitName){
  const p = dailyByDate.get(dateStr);
  return Math.max(bestScoreFromDaily(p, habitName), bestScoreFromLog(habitLog, dateStr, habitName));
}

const rows=[];
for(const [area, habitNames] of areas.entries()){
  let marks=0, points=0;
  for(let d=1; d<=nDays; d++){
    const dk=dateKey(y,m,d);
    for(const hn of habitNames){
      const s=scoreFor(dk, hn);
      points += s;
      if(s>0) marks += 1;
    }
  }
  rows.push([area, marks, points]);
}
rows.sort((a,b)=>b[2]-a[2]);

dv.table(["Área","Marcas (feitos)","Pontos (soma dos pesos)"], rows);
```
