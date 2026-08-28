# Atividade Avaliativa: Ferramentas de Gerenciamento

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 01 — Unidade 5 |
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Concluído |

---

> **Objetivo da Atividade:**  
> Compreender como casos de teste são organizados e rastreados em ferramentas de gerenciamento utilizadas pelo mercado.

---

-  Ferramentas de gerenciamento de software/de teste (como o JIRA) organizam o trabalho numa estrutura de hierarquia em que o projeto reúne tudo relacionado ao sistema testado; dentro dele, os planos de teste agrupam o que será testado numa entrega específica, como um release, sprint ou funcionalidade; e dentro dos planos ficam os ciclos de testes, que reúnem os casos de teste executados juntos numa rodada, como uma regressão de release. O caso de teste é o item individual, registrado num formato padronizado — com ID único, título, pré-condições, passos numerados, resultado esperado e status de execução (sucesso, falha, bloqueado ou não executado), além de campos como prioridade, responsável e evidências anexadas quando o teste falha. Essa padronização é o que permite que qualquer pessoa do time entenda o que foi testado sem depender de explicação verbal, e também permite reaproveitar os mesmos casos em ciclos futuros.

-  A rastreabilidade em si ocorre a partir do vínculo entre requisito, caso de teste, execução e, quando algo falha, o bug correspondente: o que permite saber o que já tem cobertura de teste e acompanhar métricas em relatórios. No caso do JIRA especificamente, a ferramenta tem vários recursos chamados plugins como Xray ou Zephyr Scale, além de opções com IA nativa, que possibilitam ter um fluxo de planos, ciclos e casos de teste dentro da própria ferramenta da atlassian.
