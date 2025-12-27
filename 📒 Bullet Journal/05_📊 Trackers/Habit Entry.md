
## 🔁 Hábitos

<%*
const masterPath = "📒 Bullet Journal/05_📊 Trackers/Habits (Master).md";
const text = await app.vault.adapter.read(masterPath);

// Parse Master: ## Área e "- Hábito ::N"
let area = null;
let out = [];

for (const raw of text.split("\n")) {
  const line = raw.trimEnd();

  const h = line.match(/^##\s+(.*)\s*$/);
  if (h) {
    area = h[1].trim();
    out.push(`### ${area}`);
    continue;
  }

  const it = line.match(/^- (.+?)\s*::\s*(\d)\s*$/);
  if (it && area) {
    out.push(`- [ ] ${it[1].trim()} ::${it[2]}`);
  }
}

tR += out.join("\n");
%>
