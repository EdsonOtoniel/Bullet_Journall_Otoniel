
---
type: monthly
month: <% tp.date.now("YYYY-MM") %>
year: <% tp.date.now("YYYY") %>
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>

---

# 📆 MONTHLY LOG — <% tp.date.now("MMMM YYYY") %>


> Este Monthly é o **hub estratégico do mês**.  
> Ele conecta **Goals (Short / Medium / Long)** → **Ações Mensais** → **Daily Logs**.

---
## 🧭 CONTEXTO ESTRATÉGICO (GOALS)

> Seção de **alinhamento**, não de execução.

### 🟢 Short Term Goals (1–3 meses)
```dataview
LIST
FROM "📒 Bullet Journal/01_📌 Goals/Short Term.md"
```
### 🟡 Medium Term Goals (4–12 meses)
```dataview
LIST
FROM "📒 Bullet Journal/01_📌 Goals/Medium Term.md"
```
### 🔵 Long Term Goals (1–5 anos)
```dataview
LIST
FROM "📒 Bullet Journal/01_📌 Goals/Long Term.md"
```
---
## 🎯 METAS DO MÊS (DERIVADAS DOS GOALS)

> Estas metas são **puxadas automaticamente** dos arquivos de Goals (Short/Medium/Long).  
> Para aparecer aqui, a meta deve conter:
> - `gstatus:: active`
> - `area:: research|teaching|health|organization`
> - `target_month:: YYYY-MM` (ex.: `target_month:: 2025-12`)
>
> ✅ Marque a conclusão **no arquivo de origem (Goals)** e ela some daqui automaticamente.

---
### 🧠 Pesquisa & Programação
```dataview
TASK
FROM "📒 Bullet Journal/01_📌 Goals"
WHERE contains(text, "gstatus:: active")
AND contains(text, "area:: research")
```
---
### 👨‍🏫 Ensino

```dataview
TASK
FROM "📒 Bullet Journal/01_📌 Goals"
WHERE contains(text, "gstatus:: active")
AND contains(text, "area:: teaching")
```

---
### 🧘 Saúde & Rotina

```dataview
TASK
FROM "📒 Bullet Journal/01_📌 Goals"
WHERE contains(text, "gstatus:: active")
AND contains(text, "area:: health")
```
---
### 💻 Organização & Produtividade

```dataview
TASK
FROM "📒 Bullet Journal/01_📌 Goals"
WHERE contains(text, "gstatus:: active")
AND contains(text, "area:: organization")
```
---

## 🛠️ AÇÕES DO MÊS (EXECUTÁVEIS)

> Estas tarefas **descem automaticamente para o Daily**.  
> Escreva apenas ações **concretas e mensuráveis**.

- [ ]
    
- [ ]
    
- [ ]
    

---

## 📌 AÇÕES PENDENTES (VISÃO DINÂMICA)

> Lida automaticamente no **Daily Log**

`TASK FROM this.file WHERE !completed`

---

## 📅 EVENTOS IMPORTANTES DO MÊS


---

## 🧠 REVISÃO DE FIM DE MÊS

(preencher no último dia)

- O que avancei nos **Short Goals**?
    
- Algum **Medium Goal** começou a se materializar?
    
- Algum **Long Goal** foi impactado?
    
- O que não avançou e por quê?
    
- O que migra para o próximo mês?
    

---

## 🔄 MIGRAÇÃO

-  Tarefas que seguem para o próximo mês
    
-  Tarefas que voltam para Goals
    
-  Tarefas que viraram projetos
    

---
## 🔗 NAVEGAÇÃO

- 🎯 Goals: [[Short Term]] | [[Medium Term]] | [[Long Term]]
    
- 📅 Future Log: [[2025]]
    
- 🗓️ Daily Logs: `04_🗓️ Daily Log/`
    
- 📊 Habit Tracker: [[Habit Dashboard]]
---
