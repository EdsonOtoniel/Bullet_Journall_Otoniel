---
type: daily
tags: [daily-log]
date: <% tp.file.title %>
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
time: <% tp.date.now("HH:mm") %>
week: <% moment(tp.file.title, "YYYY-MM-DD").format("gggg-[W]ww") %>
month: <% moment(tp.file.title, "YYYY-MM-DD").format("YYYY-MM") %>
year: <% moment(tp.file.title, "YYYY-MM-DD").format("YYYY") %>
---
## Tittle
## 🗓️ <% moment(tp.file.title, "YYYY-MM-DD").format("dddd, DD/MM/YYYY") %> — Daily Log

## 🔗 Navegação

- ⬅️ Ontem: [[Daily/<% tp.date.now("YYYY-MM", -1, tp.file.title, "YYYY-MM-DD") %>/<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>|<% tp.date.now("DD/MM", -1, tp.file.title, "YYYY-MM-DD") %>]]
- ➡️ Amanhã: [[Daily/<% tp.date.now("YYYY-MM", 1, tp.file.title, "YYYY-MM-DD") %>/<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>|<% tp.date.now("DD/MM", 1, tp.file.title, "YYYY-MM-DD") %>]]
- 📅 Semana: [[Weekly/<% moment(tp.file.title,"YYYY-MM-DD").format("gggg-[W]ww") %>|W<% moment(tp.file.title,"YYYY-MM-DD").format("ww") %>]]
- 🗓️ Mês: [[Monthly/<% moment(tp.file.title,"YYYY-MM-DD").format("YYYY-MM") %>|<% moment(tp.file.title,"YYYY-MM-DD").format("MMMM YYYY") %>]]

---
## 🎯 Meta principal do dia
> (uma só coisa que, se feita, já faz o dia valer)


---
## ✅ Tarefas do Dia
- [ ] 
- [ ] 
- [ ] 


---
## 📝 Log rápido (Bullet Journal)
- 
- 
- 


---
## 🔁 Hábitos de Hoje

> Marque aqui.  
> Os hábitos vêm automaticamente de **Habits (Master)**.

<% tp.file.include("[[Habit Entry]]") %>

---
## 🌙 Reflexão do Dia
- O que deu certo?

- O que não deu?

- Próximo passo:


---
## 📌 Tasks (plugin Tasks)

#### 📌 Pendências + Hoje (Tasks)
```tasks
not done
(due before today) OR (scheduled before today) OR (due today) OR (scheduled today)
sort by due
sort by priority
```
#### 📌 Amanhã (prévia)
```tasks
not done
(due tomorrow) OR (scheduled tomorrow)
sort by priority
sort by description
```

