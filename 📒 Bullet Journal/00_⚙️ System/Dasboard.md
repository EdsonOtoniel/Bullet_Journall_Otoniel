# 📊 Dashboard — Bullet Journal

> **Painel central de navegação, visão e decisão**  
> Atualizado automaticamente a partir dos Logs, Goals e Trackers

---

## 🧭 Identidade & Direção

> (âncora — você lê isso quando estiver sobrecarregado)

- 📌 **Life Map:** [[📒 Bullet Journal/01_📌 Goals/Life Map]]
    
- 🎯 **Short / Medium / Long Goals:**
    
    - [[📒 Bullet Journal/01_📌 Goals/Short Term]]
        
    - [[📒 Bullet Journal/01_📌 Goals/Medium Term]]
        
    - [[📒 Bullet Journal/01_📌 Goals/Long Term]]
        

---

## 📅 Navegação Rápida

```dataviewjs

const today = window.moment().format("YYYY-MM-DD"); const month = window.moment().format("YYYY-MM"); dv.list([   `🗓️ Hoje: [[📒 Bullet Journal/04_🗓️ Daily Log/${month}/${today}]]`,   `📆 Mês atual: [[📒 Bullet Journal/03_📆 Monthly Log/${month}]]`,   `📅 Future Log: [[📒 Bullet Journal/02_📅 Future Log/${window.moment().year()}]]` ]);
```
---

## 🔥 Foco do Dia

> _Uma coisa que, se feita, já faz o dia valer._

```dataview 

LIST FROM "📒 Bullet Journal/04_🗓️ Daily Log" WHERE file.name = date(today) LIMIT 1

```


_(ou simplesmente um link manual para o Daily de hoje — simples também funciona)_


---

## 📈 Hábitos — Visão Geral

> Acesse os painéis visuais de cada hábito

- [[Heatmap - Habits]]
    
## 📊 Progresso do Mês (Resumo)

> Painel quantitativo (barra + streak)

```dataview 
FROM "📒 Bullet Journal/03_📆 Monthly Log" 
WHERE file.name = dateformat(today, "YYYY-MM")
```


_(Aqui entra o quadro **Progresso + Streak** que você já finalizou)_

---

## 🗂️ Ações em Aberto (cross-daily)

> Tudo que ainda está pendente, independente do dia em que foi criado

```tasks
not done 
path includes "📒 Bullet Journal/04_🗓️ Daily Log" 
sort by due
```

> 🔎 **Uso prático:** revisar isso 1x/dia ou 1x/semana