# 📒 bullet_journal_otoniel

Sistema completo de **Bullet Journal no Obsidian**, focado em **longo prazo, hábitos com intensidade, pesquisa acadêmica, ensino e vida pessoal**, com **dashboards, heatmaps e automações visuais**.

---

## 🎯 Visão Geral

Este repositório contém um **vault completo do Obsidian** que implementa um Bullet Journal moderno, estruturado e sustentável.

O sistema conecta:

- 🧭 **Identidade e valores** (Life Map)
- 🎯 **Objetivos de curto, médio e longo prazo**
- 📅 **Planejamento anual e mensal**
- 🗓️ **Execução diária**
- 📊 **Hábitos com intensidade (1–4), streaks e heatmaps**
- 📈 **Dashboards para decisão e revisão**

O foco não é produtividade extrema, mas **clareza, profundidade e consistência ao longo do tempo**.

---

## 🧠 Filosofia do Sistema

1. **Identidade → Direção → Execução**
2. **Poucas coisas bem feitas**
3. **Progresso mensurável, não perfeição**
4. **Sustentabilidade > intensidade episódica**

Este Bullet Journal foi desenhado para funcionar por **anos**, não por semanas.

---

## 🗂️ Estrutura do Vault

📒 Bullet Journal/
│
├── 00_⚙️ System/
│ └── Templates/
│ ├── Daily.md
│ ├── Weekly.md
│ ├── Monthly.md
│ ├── Future_Log.md
│ └── Habit_Entry.md
│
├── 01_📌 Goals/
│ ├── Life Map.md
│ ├── Short Term.md
│ ├── Medium Term.md
│ └── Long Term.md
│
├── 02_📅 Future Log/
│ └── 2025.md
│
├── 03_📆 Monthly Log/
│ └── YYYY-MM.md
│
├── 04_🗓️ Daily Log/
│ └── YYYY-MM/
│ └── YYYY-MM-DD.md
│
├── 05_📊 Trackers/
│ ├── Habits (Master).md
│ ├── Habit Log.md
│ ├── Academia Heatmap.md
│ ├── (outros heatmaps por hábito)
│ └── Habit Dashboard.md
│
└── Dashboard.md

---

## 📌 Componentes Principais

### 🧭 Life Map
Documento de identidade e direção.

Define:
- Papéis fundamentais
- Valores
- Princípios de decisão
- Filtros para priorização

É usado para alinhar decisões, não como checklist.

---

### 🎯 Goals (Short / Medium / Long Term)

- **Short Term (1–3 meses):** ações concretas que alimentam o Monthly
- **Medium Term (4–12 meses):** consolidação de projetos e competências
- **Long Term (1–5 anos):** identidade e direção estratégica

Formato padrão:

- Nome do objetivo ::peso (1–4)
📅 Future Log
Visão anual de crescimento.

Usado para:

Marcos importantes

Eventos relevantes

Revisões estruturais

Não é uma lista de tarefas.

📆 Monthly Log
Ponto central de controle mensal.

Inclui:

Metas derivadas dos Goals

Quadro Progresso + Streak por hábito

Revisão mensal guiada

🗓️ Daily Log
Unidade mínima de execução.

Inclui:

Meta principal do dia

Tarefas

Log rápido

Registro de hábitos com intensidade

Exemplo:

Copiar código
- [x] Academia 🏋️ ::4
🔁 Sistema de Hábitos com Intensidade
Habits (Master)
Arquivo central que define todos os hábitos.

Exemplo:

Copiar código
## Saúde
- Academia 🏋️ ::4
- Dormir bem 😴 ::4
- Beber água 💧 ::2
Habit Log
Arquivo de backup para dias sem Daily Log.

Exemplo:

Copiar código
## 2025-12-27
- [x] Academia 🏋️ ::4
- [x] Beber água 💧 ::2
Heatmaps
Cada hábito possui um heatmap anual, com intensidade de 1 a 4, lido automaticamente de:

Daily Log

Habit Log (fallback)

A visualização é feita com o plugin Heatmap Calendar.

📊 Dashboard
Arquivo central de navegação e decisão.

Reúne:

Links rápidos para Daily, Monthly e Future Log

Acesso aos Goals

Links para heatmaps

Visão geral do progresso

O Dashboard não é para editar, mas para decidir para onde ir.

🔌 Plugins Necessários
Essenciais
Dataview

DataviewJS

Tasks

Templater

Recomendados
Heatmap Calendar
https://github.com/Richardsl/heatmap-calendar-obsidian

Periodic Notes

Minimal Theme

⚙️ Como Colocar em Uso
Clone este repositório

Abra a pasta como um vault no Obsidian

Instale os plugins listados

Ative JavaScript queries no Dataview

Configure o Templater (pasta 00_⚙️ System/Templates)

Crie seu primeiro Daily

Registre hábitos usando ::1–4

Use o Dashboard como ponto central

🎯 Para Quem Este Sistema É Indicado
Pesquisadores

Professores

Estudantes avançados

Pessoas orientadas a longo prazo

Quem deseja integrar vida pessoal e profissional em um único sistema

❌ O Que Este Sistema Não É
Um app de produtividade instantânea

Uma lista infinita de tarefas

Um sistema de pressão ou cobrança constante

Este é um sistema de vida, não de estresse.

📜 Licença
Uso pessoal e educacional livre.
Sinta-se à vontade para adaptar, melhorar ou fazer fork.

✨ Autor
Edson Otoniel
Sistema desenvolvido a partir de práticas reais de pesquisa, ensino e organização pessoal.

