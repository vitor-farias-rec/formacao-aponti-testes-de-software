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

1. Given usuário está autenticado como "admin" com perfil "Administrador"

2. And e existe ao menos 1 registro cadastrado no Controle de Estoque

3. When acesso o menu "Suprimentos" e clico em "Controle de estoque"

4. Then a tela exibe o título "Controle de estoque", o campo de pesquisa e o botão "+ Novo registro"

5. And a tabela exibe as colunas "PRODUTO", "QUANTIDADE", "MOVIMENTAÇÃO", "DATA" e "AÇÕES" com os registros existentes

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| A tela carregou com todos os elementos esperados. A tabela exibiu o registro "Papel A4", quantidade 18, movimentação "Entrada", data 2026-07-10, com ícones de editar e excluir na coluna Ações. | A tela carregou com todos os elementos esperados. A tabela exibiu o registro "Papel A4", quantidade 18, movimentação "Entrada", data 2026-07-10, com ícones de editar e excluir na coluna Ações. |

## CT-02 Acesso ao formulário de novo registro

### **BDD-Gherkin:**

1. Given que estou autenticado como "admin" na listagem do Controle de Estoque

2. When clico no botão "+ Novo registro"

3. Then o sistema exibe a tela "Novo registro - Controle de estoque"

4. And o formulário contém os campos Produto, Quantidade, Movimentação e Data

5. And exibe os botões "Cancelar", "Salvar registro" e "Voltar"

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O formulário exibiu corretamente os 4 campos esperados (Produto, Quantidade, Movimentação, Data) e os botões "Cancelar", "Salvar registro" e "Voltar", mantendo um padrão coerente de interface. | O formulário exibiu corretamente os 4 campos esperados (Produto, Quantidade, Movimentação, Data) e os botões "Cancelar", "Salvar registro" e "Voltar", mantendo um padrão coerente de interface. |

## CT-03 Validação de tipo no campo "Quantidade"

### **BDD-Gherkin:**

1. Given que estou no formulário "Novo registro - Controle de estoque" com os demais campos preenchidos

2. When digito um valor não numérico, como "dez", no campo "Quantidade"

3. And clico em "Salvar registro"

4. Then o sistema deve rejeitar o valor e impedir o envio do formulário

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O sistema rejeita valores não numéricos (ex: "dez"), indicando com mensagem de alerta que o campo deve ser preenchido com caracteres válidos. | O sistema não rejeita ou impede valores não numéricos, sendo possível registrar com mais um notação (ex: "10" e "dez"). |

## CT-04 Padronização do formato de datas

### **BDD-Gherkin:**

1. Given que estou na tela "Controle de estoque" autenticado como "admin"

2. When comparo a data do cabeçalho com a data da coluna "DATA" da tabela

3. Then as duas deveriam seguir o mesmo formato de exibição

4. But o cabeçalho mostra a data por extenso e a tabela mostra formato ("aaaa-mm-dd")

5. And o campo "Data" do formulário não possui seletor de calendário nem indicação do formato esperado

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| Não há formatação padrão de data em nenhuma etapa: o campo aceita qualquer texto e a tabela exibe exatamente o que foi digitado, sem conversão. Se dois usuários cadastrarem datas em formatos diferentes, os dois formatos vão conviver na mesma tabela. Além disso, a divergência do formato de data estabelecido no cabeçalho da tela (data atual do sistema) e o que pode ser salvo em registro. | Não há formatação padrão de data em nenhuma etapa: o campo aceita qualquer texto e a tabela exibe exatamente o que foi digitado, sem conversão. Se dois usuários cadastrarem datas em formatos diferentes, os dois formatos vão conviver na mesma tabela. Além disso, a divergência do formato de data estabelecido no cabeçalho da tela (data atual do sistema) e o que pode ser salvo em registro. |

## CT-05 Restrição de acesso ao módulo por perfil

### **BDD-Gherkin:**

1. Given usuário está na tela de login do sistema

2. When e entra com a conta de demonstração do perfil "Médico"

3. Then o item "Controle de estoque" não aparece no menu lateral

4. When usuário sair e entrar com a conta de demonstração do perfil "Cliente"

5. Then o item "Controle de estoque" não aparece no menu lateral

| Cenário de Sucesso | Cenário de Falha |
|---|---|
| O sistema restringe o acesso ao módulo de Controle de Estoque aos perfis de admin e funcionários. | O sistema restringe o acesso ao módulo de Controle de Estoque aos perfis de admin e funcionários. |
