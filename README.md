# 📒 Bullet Journal - Obsidian System

Bem-vindo ao seu sistema pessoal de organização digital! Este Vault foi estruturado inspirado no método **Bullet Journal**, mas potencializado pelas automações do Obsidian.

---

## 🏗️ Estrutura e Definições

### 01_📌 Goals (Objetivos)
Aqui é onde você define o **PORQUÊ** das suas ações. A separação temporal ajuda a manter o foco sem perder a visão de futuro.

*   **Short Term (Curto Prazo)**: Foco nos próximos **1 a 3 meses**. São metas táticas e imediatas.
    *   *Exemplo*: "Ler 2 livros técnicos", "Lançar o MVP do projeto X", "Contratar um designer".
*   **Medium Term (Médio Prazo)**: Horizonte de **3 a 12 meses**. São os marcos (milestones) que conectam você aos seus sonhos maiores.
    *   *Exemplo*: "Atingir nível intermediário em Italiano", "Juntar R$ 10k para viagem", "Terminar a pós-graduação".
*   **Long Term (Longo Prazo)**: Visão de **1 a 5+ anos**. Define quem você quer se tornar. É a bússola da vida.
    *   *Exemplo*: "Ser fluente em 3 idiomas", "Comprar a casa própria", "Ser referência na minha área profissional".

### 05_📊 Trackers (Hábitos)
O sistema de hábitos neste Vault é **automático e centralizado**.

**Como funciona:**
1.  **Cadastro (Configuração)**: Você NÃO edita os hábitos na nota do dia a dia. Você os define no arquivo mestre:
    *   Vá em: `05_📊 Trackers/Habits (Master).md`
    *   Adicione ou remova hábitos seguindo a estrutura: `- Nome do Hábito ::Pontos` (ex: `- Beber água 💧 ::2`).
    *   Os pontos (::2, ::4) servem para dar peso ao hábito (ex: hábitos difíceis valem mais no seu score diário).
2.  **Execução (Dia a Dia)**:
    *   Toda vez que você criar um novo **Daily Log**, o sistema lê o arquivo "Master" e cria automaticamente a lista de checkboxes para aquele dia.
    *   Você apenas marca o checkbox `- [x]` no seu Daily Log quando realizar a tarefa.
    *   O Dashboard e os Heatmaps atualizam sozinhos lendo esses checkboxes marcados.

---

## �️ Requisitos e Configuração

Para o funcionamento completo, certifique-se de ativar os plugins da comunidade:
1.  **Dataview** (Enable JavaScript): Para os painéis.
2.  **Templater**: Para criar as notas com as datas corretas.
    *   *Folder Templates*: `00_⚙️ System/Templates`
3.  **Tasks**: Para gerenciar pendências globais.

---

## 🚀 Fluxo de Trabalho (Workflow)

### ☀️ Manhã (Planejamento)
1.  Abra o **Dashboard**.
2.  Crie a nota do dia (Daily). O script importará seus hábitos do *Master*.
3.  Defina sua **Meta Principal** do dia.

### 🌙 Noite (Encerramento)
1.  Marque os hábitos realizados no arquivo do dia.
2.  Faça a reflexão rápida.
3.  Se sobrou tarefa não feita:
    *   Cancele (`- [~]`), Mova para amanhã (`>`) ou Mova para o Future Log (`<`).

---

<br>

> **Sistema Automatizado de Produtividade**
> *Desenvolvido e Criado por **Edson Otoniel***
