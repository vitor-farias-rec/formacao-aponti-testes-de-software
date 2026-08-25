# Atividade Avaliativa: Escrevendo testes em Gherkin

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
> Escreva testes para uma funcionalidade de sua escolha no seu projeto, criando cenários de sucesso e cenários alternativos de falha.

---

## **TESTES MÓDULO DE CONTROLE DE ESTOQUE**

## CT-01 Listagem de registros

### **BDD-Gherkin:**

- Given usuário está autenticado como "admin" com perfil "Administrador"

- And e existe ao menos 1 registro cadastrado no Controle de Estoque

- When acesso o menu "Suprimentos" e clico em "Controle de estoque"

- Then a tela exibe o título "Controle de estoque", o campo de pesquisa e o botão "+ Novo registro"

- And a tabela exibe as colunas "PRODUTO", "QUANTIDADE", "MOVIMENTAÇÃO", "DATA" e "AÇÕES" com os registros existentes

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| A tela carregou com todos os elementos esperados. A tabela exibiu o registro "Papel A4", quantidade 18, movimentação "Entrada", data 2026-07-10, com ícones de editar e excluir na coluna Ações. | A tela carregou com apenas 3 colunas "PRODUTO", "QUANTIDADE" e "MOVIMENTAÇÃO", não constando coluna para "DATA", um elemento essencial para estoque de produtos. Dessa forma, a exibição da tabela do registro "Papel A4", quantidade 18, movimentação "Entrada" foi comprometida. Demais ícones de editar e excluir foram renderizados na coluna "AÇÕES". |

## CT-02 Acesso ao formulário de novo registro

### **BDD-Gherkin:**

- Given que estou autenticado como "admin" na listagem do Controle de Estoque

- When clico no botão "+ Novo registro"

- Then o sistema exibe a tela "Novo registro - Controle de estoque"

- And o formulário contém os campos Produto, Quantidade, Movimentação e Data

- And exibe os botões "Cancelar", "Salvar registro" e "Voltar"

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O formulário exibiu corretamente os 4 campos esperados (Produto, Quantidade, Movimentação, Data) e os botões "Cancelar", "Salvar registro" e "Voltar", mantendo um padrão coerente de interface. | O botão "+ Novo Registro" não carrega e nem redireciona a página para fluxo correto da ação do usuário. |

## CT-03 Validação de tipo no campo "Quantidade"

### **BDD-Gherkin:**

- Given que estou no formulário "Novo registro - Controle de estoque" com os demais campos preenchidos

- When digito um valor não numérico, como "dez", no campo "Quantidade"

- And clico em "Salvar registro"

- Then o sistema deve rejeitar o valor e impedir o envio do formulário

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O sistema rejeita valores não numéricos (ex: "dez"), indicando com mensagem de alerta que o campo deve ser preenchido com caracteres válidos. | O sistema não rejeita ou impede valores não numéricos, sendo possível registrar com mais um notação (ex: "10" e "dez"). |

## CT-04 Padronização do formato de datas

### **BDD-Gherkin:**

- Given que estou na tela "Controle de estoque" autenticado como "admin"

- When comparo a data do cabeçalho com a data da coluna "DATA" da tabela

- Then as duas deveriam seguir o mesmo formato de exibição

- But o cabeçalho mostra a data por extenso e a tabela mostra formato ("aaaa-mm-dd")

- And o campo "Data" do formulário não possui seletor de calendário nem indicação do formato esperado

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| Formatação padrão de data DD/MM/AAAA. O sistema não aceita formato fora do padrão da tabela. | Não há formatação padrão de data em nenhuma etapa: o campo aceita qualquer texto e a tabela exibe exatamente o que foi digitado, sem conversão. Se dois usuários cadastrarem datas em formatos diferentes, os dois formatos vão conviver na mesma tabela. Além disso, a divergência do formato de data estabelecido no cabeçalho da tela (data atual do sistema) e o que pode ser salvo em registro. |

## CT-05 Restrição de acesso ao módulo por perfil

### **BDD-Gherkin:**

- Given usuário está na tela de login do sistema

- When e entra com a conta de demonstração do perfil "Médico"

- Then o item "Controle de estoque" não aparece no menu lateral

- When usuário sair e entrar com a conta de demonstração do perfil "Cliente"

- Then o item "Controle de estoque" não aparece no menu lateral

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O sistema restringe o acesso ao módulo de Controle de Estoque aos perfis de admin e funcionários. | O sistema permite o acesso ao módulo de Controle de Estoque aos 4 perfis de admin, funcionário, cliente e médico. |
