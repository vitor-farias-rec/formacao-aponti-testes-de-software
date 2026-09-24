# Atividade Avaliativa: Testes Manuais x Automatizado

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 01 — Unidade 5 |
| **Sistema Analisado** | Nova versão de Sistema Bancário (Cenário Fictício) |
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Concluído |

---

> **Objetivo da Atividade:**  
> Classificar cada cenário como Manual ou Automatizado e justificar brevemente a decisão.

---

## Análise dos Cenários de Teste

| Situação Analisada | Modo de Execução | Justificativa |
|---|---|---|
| **1. Entrada de usuários no sistema** | **Automatizado** | **Muitas repetições e regra fixa:** Esse teste precisa ser refeito toda vez que o sistema passa por uma melhoria. Como a tela de entrada quase não muda, colocar o computador para testar economiza o tempo da equipe. |
| **2. Impedir dois agendamentos no mesmo horário** | **Automatizado** | **Evitar erros graves com rapidez:** É uma regra matemática e fixa. O computador consegue testar a tentativa de bloqueio em poucos segundos a cada nova versão, sem o risco de esquecer nenhum detalhe. |
| **3. Atualização automática da agenda após a recepção** | **Automatizado** | **Conferência de regras internas:** O objetivo é garantir que as informações troquem de lugar sem se perderem. Como o caminho é sempre o mesmo, o computador faz essa checagem contínua com custo muito baixo. |
| **4. Visualização de cores, tamanho dos botões e facilidade de leitura** | **Manual** | **Necessidade do olhar humano:** Saber se uma tela é agradável e fácil de ler exige a percepção de uma pessoa. Como a aparência muda bastante no início, seria muito caro ensinar o computador a avaliar cada pequena mudança visual. |
| **5. Cálculos de cobranças, recibos e relatórios de dinheiro** | **Automatizado** | **Precisão com números:** Fazer contas na mão gasta muito tempo e gera cansaço. O computador confere se as somas e os relatórios estão corretos com exatidão e sem erros humanos. |
| **6. Teste da rotina de trabalho com a equipe da clínica** | **Manual** | **Avaliação prática:** O objetivo é saber se o sistema realmente ajuda recepcionistas e profissionais no trabalho diário. Somente pessoas de verdade usando o sistema conseguem dar esse retorno. |
