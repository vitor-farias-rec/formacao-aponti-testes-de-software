# Atividade Avaliativa: Tradicional vs. BDD

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 01 — Unidade 6 |
| **Sistema Analisado** | Clinica PSI (Cenário Fictício)|
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Em andamento |

---

> **Objetivo da Atividade:**  
> Comparar o mesmo comportamento do sistema usando duas abordagens diferentes.

---

## CT-08 Validação Temporal da Função de Movimentação de Estoque

### **BDD-Gherkin:**

Cenário: Submeter registro de estoque com a data atual com sucesso

1. Given os seguintes dados de teste estão prontos para envio:

| Item | Quantidade | Tipo | Data |
| :--- | :--- | :--- | :--- |
| Papel A4 | 960 | Entrada | 19-08-2026 |

2. When usuário chamar a função "submitForm(e)" com esses dados

3. Then a função deve processar os dados aplicando o ".trim()"

4. And a função "save(d)" deve ser executada com sucesso

5. And o registro deve ser mantido no "localStorage"

6. And a mensagem de confirmação "Registro salvo com sucesso." deve ser exibida

7. And a tabela de estoque deve ser atualizada exibindo o novo item

Cenário: Impedir o registro de estoque com data futura

1. Given os seguintes dados de teste estão prontos para envio::

| Item | Quantidade | Tipo | Data |
| :--- | :--- | :--- | :--- |
| Papel A4 | 960 | Entrada | 22-08-2026 |

2. When usuário chamar a função "submitForm(e)" com esses dados contendo uma data futura

3. Then o envio deve ser bloqueado

4. And a função "save(d)" não deve ser chamada

5. And nenhuma alteração deve ser feita no "localStorage"

6. And o registro não deve ser adicionado à tabela

7. And um alerta deve ser exibido informando que não são permitidos lançamentos com datas posteriores à data atual

### **Script Teste Manual:**

ID: CT-08

Título: Validação Temporal da Função de Movimentação de Estoque

Pré-condições: Formulário de estoque carregado e função submitForm(e) disponível para chamada.

Passos:

Submeter o formulário chamando a função submitForm(e) contendo uma data igual à data atual no array de valores v = ["Papel A4", "960", "Entrada", "19-08-2026"].

Submeter o formulário chamando a função submitForm(e) contendo uma data posterior à data atual no array de valores v = ["Papel A4", "960", "Entrada", "22-08-2026"].

Resultado Esperado:

Passo 1: A função submitForm processa os dados com .trim(), executa a função save(d) com sucesso, mantém o registro no localStorage, exibe a mensagem de confirmação "Registro salvo com sucesso." e atualiza a tabela de estoque exibindo o novo item.

Passo 2: A função submitForm identifica a data futura, bloqueia o envio, não chama a função save(d) (impedindo qualquer gravação no localStorage), não adiciona o registro à tabela e exibe uma mensagem de alerta na tela informando que não são permitidos lançamentos com datas posteriores à data atual.
