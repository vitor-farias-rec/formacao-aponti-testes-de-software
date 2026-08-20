# Atividade Avaliativa: Desafio Testes Funcionais e não funcionais

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 01 — Unidade 3 |
| **Sistema Analisado** | Clinica PSI (Cenário Fictício)|
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Em Execução |

---

> **Objetivo da Atividade:**  
> Propor uma análise de diversos testes funcionais e não funcionais.

---

## Parte 1 — Testes funcionais

### Exercício 1 — Identificação das funcionalidades

Foram selecionadas seis funcionalidades (acima do mínimo de cinco solicitado), escolhidas por representarem o fluxo mais crítico da clínica — do cadastro do paciente ao encerramento financeiro do atendimento — e reutilizadas como base para os exercícios seguintes.

| Funcionalidade | Objetivo | Usuário | Dados necessários | Resultado esperado | Possíveis erros |
|---|---|---|---|---|---|
| Cadastro de paciente | Registrar os dados de um novo paciente para permitir agendamentos e atendimentos futuros. | Recepcionista | Nome completo; CPF; Data de nascimento; Telefone; E-mail; Convênio | Paciente cadastrado com sucesso e disponível para busca e agendamento. | CPF inválido ou duplicado; Campos obrigatórios não preenchidos; E-mail em formato inválido |
| Agendar consulta | Marcar uma consulta entre paciente e psicólogo em uma data e horário disponíveis. | Recepcionista | Paciente; Psicólogo; Data; Horário | Consulta registrada e exibida na lista de Agendamentos e na agenda do psicólogo. | Horário indisponível (conflito de agenda); Data no passado; Falta de preenchimento de campo (ex: Paciente ou Psicólogo) |
| Check-in e controle de presença | Registrar a chegada do paciente no dia da consulta. | Recepcionista | Agendamento correspondente; Horário de chegada | Status de presença atualizado (ex.: "presente") e vinculado à consulta. | Check-in em consulta cancelada ou inexistente; Check-in duplicado para a mesma consulta |
| Registrar evolução de sessão | Documentar as anotações clínicas referentes à sessão realizada. | Psicólogo | Paciente; Data da sessão; Tema abordado; Observações Clínicas | Evolução salva e vinculada ao prontuário do paciente, visível apenas a usuários autorizados. | Conteúdo obrigatório em branco; Tentativa de acesso/edição por perfil não autorizado |
| Lançar receita | Registrar um valor recebido pela clínica, referente a uma consulta ou outro serviço. | Recepcionista / Financeiro | Valor; Categoria; Data; Descrição; Consulta vinculada | Receita lançada, saldo financeiro atualizado e valor refletido no Relatório financeiro. | Valor zero ou negativo; Data inválida |
| Controlar estoque (entrada/saída) | Atualizar a quantidade disponível de um produto após entrada ou saída. | Recepcionista / Administrador | Produto; Quantidade; Tipo de movimentação (entrada ou saída) | Quantidade em estoque atualizada corretamente; alerta emitido se abaixo do mínimo. | Saída maior que o estoque disponível; Quantidade negativa ou não numérica |

### Exercício 2 — Testes unitários

| Função/regra | Entrada | Resultado esperado | Por que é unitário? |
|---|---|---|---|
| Validação de CPF | CPF "529.982.247-25" | Válido | Avalia isoladamente o algoritmo de validação dos dígitos verificadores, sem depender de interface, banco de dados ou serviços externos. |
| Validação do número do CRP | CRP "06/12345" e CRP "12345" | "06/12345" válido; "12345" inválido (sem região) | Testa apenas a regra de formatação do registro profissional, isolada do restante do sistema. |
| Cálculo do saldo financeiro | Receitas: R$ 2.000; despesas: R$ 800 | R$ 1.200 | Verifica isoladamente uma regra de cálculo (subtração), sem acessar banco de dados. |
| Verificação de disponibilidade de horário | Horário já ocupado: 20/07/2026 10:30; novo pedido no mesmo horário | Indisponível | Avalia a lógica de comparação de horários com dados simulados (mock), sem depender da interface. |
| Identificação de estoque abaixo do mínimo | Estoque atual: 3; estoque mínimo: 5 | Alerta (abaixo do mínimo) | Testa uma função de comparação isolada, sem necessidade de banco de dados real. |
| Cálculo da taxa de presença | 8 consultas agendadas; 6 compareceram | 75% | Avalia isoladamente uma fórmula de cálculo (6 ÷ 8 × 100), sem dependências externas. |

### Exercício 3 — Testes de integração

| Componentes integrados | Ação | Resultado esperado | Risco | Justificativa |
|---|---|---|---|---|
| Cadastro de paciente + Banco de dados | Cadastrar paciente com dados válidos e reabrir a lista de pacientes. | Paciente persistido e localizável na busca após o cadastro. | Perda de dados ou paciente não aparecer na busca. | Verifica a troca de dados entre o formulário de cadastro e a camada de persistência. |
| Agendamento + Agenda do psicólogo | Agendar consulta para um psicólogo em horário livre. | Horário passa a constar como ocupado na agenda do psicólogo. | Dois pacientes agendados no mesmo horário (conflito). | Garante que o módulo de agendamento comunique corretamente a ocupação à agenda do profissional. |
| Check-in + Controle de presença | Realizar check-in de paciente com consulta confirmada. | Status de presença atualizado e refletido no histórico de atendimentos. | Presença não registrada, prejudicando relatórios de frequência. | Confirma a integração entre a tela de check-in e o módulo de presença. |
| Consulta realizada + Lançamento financeiro | Concluir o atendimento e lançar a receita correspondente. | Receita registrada e vinculada à consulta realizada. | Consulta sem cobrança lançada (perda financeira) ou lançamento duplicado. | Verifica a comunicação entre o módulo de atendimento e o módulo financeiro. |
| Compra/uso de produto + Estoque | Registrar saída de um produto do estoque. | Quantidade em estoque decrementada corretamente. | Estoque desatualizado, permitindo uso/venda sem disponibilidade real. | Avalia a troca de dados entre o módulo de movimentação e o módulo de estoque. |
| Login + Perfis e permissões | Autenticar usuário e carregar as permissões do respectivo perfil. | Usuário visualiza apenas os menus e dados permitidos pelo seu perfil. | Acesso a funcionalidades restritas por falha na integração de permissões. | Confirma que o módulo de autenticação comunica corretamente as permissões ao restante do sistema. |

### Exercício 4 — Testes de sistema

#### Cenário A — Atendimento completo

**Pré-condições:** Sistema acessível; usuário autenticado com perfil de recepcionista (e psicólogo, para a etapa de evolução); ao menos um psicólogo já cadastrado (ex.: Dra. Juliana Martins).

**Dados utilizados:**
- Paciente: Beatriz Fernandes, CPF fictício 123.456.789-00, nascida em 15/03/1990, telefone (81) 99999-1234
- Psicólogo: Dra. Juliana Martins
- Agendamento: 21/07/2026 às 11:00
- Evolução: texto fictício de evolução da sessão
- Receita: R$ 150,00 referente à consulta

**Passos:**
1. Acessar o menu "Pacientes" e cadastrar a paciente Beatriz Fernandes com os dados fictícios acima.
2. Utilizar o campo "Pesquisar registros..." para localizar "Beatriz Fernandes" na lista de pacientes.
3. Acessar "Agendamentos" > "+ Novo registro" e agendar a consulta com a Dra. Juliana Martins em 21/07/2026 às 11:00.
4. Acessar "Check-in e presença" e registrar o check-in da paciente no horário da consulta.
5. Acessar "Evoluções de sessão" e registrar a evolução referente ao atendimento realizado.
6. Acessar "Receitas" e lançar o valor de R$ 150,00 referente à consulta.
7. Acessar "Relatório financeiro" e conferir se o valor lançado aparece corretamente.

| Resultado esperado | Resultado obtido | Situação |
|---|---|---|
| Cada etapa é concluída sem erros: a paciente fica visível na busca; o agendamento passa a constar na agenda da psicóloga; o check-in atualiza o status de presença; a evolução é registrada e vinculada à paciente; a receita é lançada e refletida no saldo e no Relatório financeiro. | Cada etapa necessita de registro manual e a falta de integração entre os módulos do sistema faz com que não haja atualização, por exemplo, no relatório financeiro  após lançamento da receita referente à consulta. | Falha |

**Evidência:**

<details>
   <summary>📹 <b>Clique para expandir evidências </b></summary>
   <br>
   
  <img width="1919" height="890" alt="Captura de tela 2026-08-20 093056" src="https://github.com/user-attachments/assets/d5b31cfb-5c48-4e4d-bf43-2d30c25eb774" />
<img width="1919" height="887" alt="Captura de tela 2026-08-20 092909" src="https://github.com/user-attachments/assets/cf1448c3-c62e-4df8-9591-cadefc470822" />
<img width="1917" height="889" alt="Captura de tela 2026-08-20 092740" src="https://github.com/user-attachments/assets/8497f09f-288c-463f-9292-cbc665166c16" />
<img width="1919" height="892" alt="Captura de tela 2026-08-20 092520" src="https://github.com/user-attachments/assets/bf0a34d3-7061-4846-9ae4-d67d475c4320" />
<img width="1919" height="896" alt="Captura de tela 2026-08-20 092349" src="https://github.com/user-attachments/assets/ed5ad8d9-c795-4228-a56a-861fe9045d67" />
<img width="1919" height="895" alt="Captura de tela 2026-08-20 092322" src="https://github.com/user-attachments/assets/29277dd0-7c45-49c9-a220-9b3a4a9221fb" />
<img width="1919" height="880" alt="Captura de tela 2026-08-20 093113" src="https://github.com/user-attachments/assets/09f504ee-c7a3-4eef-a12f-8e0a6ff06f70" />

   </details>

**Justificativa da classificação:** Teste de sistema: percorre um fluxo completo do usuário final, do cadastro à conferência do relatório, atravessando módulos integrados (Pacientes, Agendamentos, Check-in, Evoluções, Receitas, Relatório financeiro), como ocorre no uso real pela recepção da clínica.

#### Cenário B — Reagendamento

**Pré-condições:** Paciente e psicólogo já cadastrados; usuário autenticado como recepcionista.

**Dados utilizados:**
- Paciente: Lucas Almeida
- Psicólogo: Dr. Rafael Lima
- Horário original: 20/07/2026 às 10:30
- Novo horário: 22/07/2026 às 15:00

**Passos:**
1. Acessar "Agendamentos" e confirmar a consulta de Lucas Almeida com o Dr. Rafael Lima em 20/07/2026 às 10:30.
2. Acessar "Reagendamentos" e alterar essa consulta para 22/07/2026 às 15:00.
3. Acessar a "Agenda profissional" do Dr. Rafael Lima e verificar se o horário 20/07/2026 10:30 voltou a ficar disponível.
4. Verificar se o novo horário, 22/07/2026 15:00, passou a constar como ocupado.
5. Conferir na tela de "Agendamentos" se a data e o horário exibidos refletem a alteração feita.

| Resultado esperado | Resultado obtido | Situação |
|---|---|---|
| O sistema atualiza o agendamento existente sem criar duplicidade; o horário anterior é liberado; o novo horário passa a constar como ocupado; a agenda reflete a alteração de forma consistente em todas as telas. | Pendente de execução no ambiente real do sistema. | A definir após a execução. |

**Evidência:**

<details>
   <summary>📹 <b>Clique para expandir a evidência em vídeo (CT02)</b></summary>
   <br>
   
   [https://github.com/user-attachments/assets/](https://github.com/user-attachments/assets/COLE-O-LINK-CT02)
   </details>

**Justificativa da classificação:** Teste de sistema: percorre múltiplas telas (Agendamentos, Reagendamentos, Agenda profissional) em um fluxo completo, semelhante ao realizado pela recepcionista no uso real do sistema.

#### Cenário C — Controle de estoque

**Pré-condições:** Usuário autenticado com permissão de acesso a "Produtos e materiais" e "Controle de estoque".

**Dados utilizados:**
- Produto: Bloco de anotações A5
- Estoque mínimo definido: 5 unidades
- Entrada registrada: 10 unidades
- Saída registrada: 7 unidades

**Passos:**
1. Acessar "Produtos e materiais" e cadastrar o produto "Bloco de anotações A5", definindo o estoque mínimo em 5 unidades.
2. Acessar "Controle de estoque" e registrar uma entrada de 10 unidades.
3. Registrar uma saída de 7 unidades.
4. Verificar a quantidade final exibida no sistema (esperado: 3 unidades).
5. Como a quantidade final (3) é menor que o mínimo (5), verificar se o sistema exibe um alerta de estoque baixo.

| Resultado esperado | Resultado obtido | Situação |
|---|---|---|
| Quantidade final exibida = 3 unidades; o sistema exibe um alerta visual (cor, ícone ou mensagem) indicando estoque abaixo do mínimo. | Pendente de execução no ambiente real do sistema. | A definir após a execução. |

**Evidência:**

<details>
   <summary>📹 <b>Clique para expandir a evidência em vídeo (CT03)</b></summary>
   <br>
   
   [https://github.com/user-attachments/assets/](https://github.com/user-attachments/assets/COLE-O-LINK-CT03)
   </details>

**Justificativa da classificação:** Teste de sistema: avalia o fluxo completo de cadastro e movimentação de estoque pela interface, incluindo a regra de negócio de alerta, tal como seria operado por um usuário real.

#### Cenário D — Controle de acesso

**Pré-condições:** Existência de um usuário administrador para a criação de perfis; menu "Perfis e permissões" disponível.

**Dados utilizados:**
- Perfil "Recepcionista": sem acesso a Prontuários
- Perfil "Psicólogo": com acesso a Prontuários
- Um usuário de teste para cada perfil

**Passos:**
1. Acessar "Perfis e permissões" e configurar/confirmar que o perfil Recepcionista não tem permissão de acesso a "Prontuários", e que o perfil Psicólogo tem.
2. Efetuar login com um usuário do perfil Recepcionista.
3. Tentar acessar o menu "Prontuários".
4. Encerrar a sessão e efetuar login novamente com um usuário do perfil Psicólogo.
5. Acessar "Prontuários" e verificar se o conteúdo é exibido normalmente.

| Resultado esperado | Resultado obtido | Situação |
|---|---|---|
| Com o perfil Recepcionista, o acesso a "Prontuários" é bloqueado (menu oculto ou acesso negado, com mensagem informativa). Com o perfil Psicólogo, o acesso funciona normalmente, exibindo os prontuários. | Pendente de execução no ambiente real do sistema. | A definir após a execução. |

**Evidência:**

<details>
   <summary>📹 <b>Clique para expandir a evidência em vídeo (CT04)</b></summary>
   <br>
   
   [https://github.com/user-attachments/assets/](https://github.com/user-attachments/assets/COLE-O-LINK-CT04)
   </details>

**Justificativa da classificação:** O teste é de sistema porque avalia o sistema completo, por meio de um fluxo semelhante ao realizado pelo usuário final (login com diferentes perfis e tentativa de acesso), conforme indicado no próprio enunciado da atividade.

#### Cenário E — Cancelamento de agendamento

**Pré-condições:** Paciente e psicólogo cadastrados; usuário autenticado como recepcionista.

**Dados utilizados:**
- Paciente: Mariana Costa
- Psicólogo: Dra. Juliana Martins
- Horário do agendamento: 20/07/2026 às 09:00

**Passos:**
1. Acessar "Agendamentos" e localizar a consulta de Mariana Costa com a Dra. Juliana Martins em 20/07/2026 às 09:00.
2. Cancelar esse agendamento.
3. Verificar se o horário foi liberado na agenda da psicóloga.
4. Verificar se o status do agendamento foi atualizado (ex.: "Cancelado").
5. Confirmar que o agendamento cancelado não aparece mais como compromisso ativo na tela principal de Agendamentos.

| Resultado esperado | Resultado obtido | Situação |
|---|---|---|
| O agendamento muda de status para "Cancelado"; o horário correspondente é liberado na agenda da psicóloga; nenhum dado de outros agendamentos é afetado. | Pendente de execução no ambiente real do sistema. | A definir após a execução. |

**Evidência:**

<details>
   <summary>📹 <b>Clique para expandir a evidência em vídeo (CT05)</b></summary>
   <br>
   
   [https://github.com/user-attachments/assets/](https://github.com/user-attachments/assets/257f8e6d-f8a9-4e6f-9fe6-13ee19bda056)
   </details>

**Justificativa da classificação:** Teste de sistema: avalia um fluxo completo do usuário final (localizar, cancelar, verificar reflexos em outras telas), atravessando módulos integrados.

### Exercício 5 — Testes de aceitação

#### Critério 1 — Impedir conflito de horário

**Dado que** já exista uma consulta confirmada para o Dr. Rafael Lima em 20/07/2026 às 10:30, **quando** a recepcionista tentar agendar outro paciente para o mesmo psicólogo no mesmo horário, **então** o sistema deve impedir o novo agendamento e exibir uma mensagem informando que o horário está indisponível.

#### Critério 2 — Restringir acesso a prontuários

**Dado que** um usuário esteja autenticado com o perfil de recepcionista, **quando** ele tentar acessar o prontuário de um paciente, **então** o sistema deve negar o acesso, permitindo a consulta apenas a psicólogos autorizados.

#### Critério 3 — Atualizar saldo financeiro

**Dado que** o saldo financeiro atual seja R$ 1.200,00, **quando** uma nova receita de R$ 300,00 for lançada, **então** o saldo exibido no Relatório financeiro deve ser atualizado para R$ 1.500,00.

#### Critério 4 — Alertar estoque mínimo

**Dado que** um produto tenha estoque mínimo configurado para 5 unidades, **quando** a quantidade disponível cair para 5 unidades ou menos após uma saída, **então** o sistema deve exibir um alerta de estoque baixo.

#### Critério 5 — Preservar dados entre sessões

**Dado que** um usuário tenha cadastrado dados de um paciente e encerrado a sessão do sistema, **quando** ele efetuar login novamente, **então** os dados cadastrados devem continuar disponíveis e inalterados.

#### Critério 6 — Localizar paciente por nome ou CPF

**Dado que** existam pacientes cadastrados com nome e CPF preenchidos, **quando** o usuário digitar parte do nome ou o número do CPF no campo de pesquisa, **então** o sistema deve retornar os pacientes correspondentes.

### Exercício 6 — Classificação dos testes

| Cenário | Classificação | Justificativa |
|---|---|---|
| 1. Verificar se receitas − despesas retorna o saldo correto. | Unitário | Avalia uma função de cálculo isolada (subtração), sem depender de interface, banco de dados ou outros módulos. |
| 2. Verificar se uma receita salva aparece no relatório financeiro. | Integração | Envolve a comunicação entre o módulo de lançamento financeiro e o módulo de relatórios, verificando a troca de dados entre dois componentes. |
| 3. Executar todo o fluxo entre cadastro, atendimento e pagamento. | Sistema | Percorre um fluxo completo pela interface, do início ao fim, simulando a jornada real do usuário por múltiplos módulos integrados. |
| 4. Confirmar com a direção da clínica se o relatório atende às necessidades administrativas. | Aceitação | Validação feita do ponto de vista do negócio, por um responsável da clínica, verificando se a funcionalidade atende às necessidades reais. |
| 5. Verificar isoladamente a validação de CPF. | Unitário | Testa uma regra específica de forma isolada, sem dependências externas — o próprio enunciado indica "isoladamente". |
| 6. Verificar se um reagendamento atualiza a agenda. | Integração | Envolve a comunicação entre o módulo de reagendamento e o módulo de agenda, garantindo a correta troca de dados entre eles. |
| 7. Avaliar se apenas psicólogos podem visualizar prontuários. | Sistema | Requer percorrer o sistema completo, autenticando com diferentes perfis pela interface, para validar a regra de controle de acesso de ponta a ponta. |
| 8. Confirmar com a recepcionista se o processo de agendamento é adequado à rotina da clínica. | Aceitação | Validação do ponto de vista do usuário do negócio, verificando a adequação do processo à rotina real de trabalho. |

## Parte 2 — Checklist de testes não funcionais

### Performance

| O que verificar | Como verificar | Critério esperado | Risco associado | Prioridade |
|---|---|---|---|---|
| Tempo de carregamento do Dashboard | Medir o tempo entre o login e a exibição completa do dashboard, usando a aba "Network"/"Performance" das ferramentas de desenvolvedor do navegador. | Carregamento completo em até 3 segundos. | Má impressão inicial e lentidão percebida logo no início do uso. | Alta |
| Tempo de abertura da tela de Agendamentos com grande volume de registros | Popular a base com 500 a 1.000 agendamentos e medir o tempo até a tabela ser exibida. | Abertura em até 2 segundos. | Atraso no atendimento durante a recepção de pacientes. | Alta |
| Velocidade da pesquisa de pacientes | Cadastrar cerca de 1.000 pacientes e medir o tempo de resposta ao digitar um termo em "Pesquisar registros...". | Resultados exibidos em até 1 segundo. | Dificuldade para localizar prontamente um paciente durante o atendimento. | Média |
| Tempo para salvar um novo agendamento | Cronometrar o intervalo entre clicar em "+ Novo registro", preencher os dados e confirmar o salvamento. | Confirmação de salvamento em até 2 segundos. | Percepção de travamento e possível duplicidade de cliques/registros. | Média |
| Estabilidade após uso prolongado | Realizar 50 ou mais operações consecutivas (cadastros, buscas, edições) sem recarregar a página, observando lentidão ou consumo de memória. | Desempenho estável, sem degradação perceptível ao longo do uso. | Travamento do navegador durante um dia cheio de atendimentos. | Média |

### Segurança

| O que verificar | Como verificar | Critério esperado | Risco associado | Prioridade |
|---|---|---|---|---|
| Acesso a prontuários sem autenticação | Tentar acessar diretamente a URL da tela de prontuários com o usuário deslogado. | Sistema redireciona para a tela de login e bloqueia o acesso ao conteúdo. | Violação de sigilo e exposição de dados de saúde de pacientes. | Alta |
| Restrição de funcionalidades por perfil de usuário | Autenticar com perfil de recepcionista e tentar acessar prontuários e relatórios financeiros. | Acesso bloqueado a telas não autorizadas para o perfil utilizado. | Acesso indevido a dados sensíveis por usuários sem permissão. | Alta |
| Exposição de dados sensíveis no armazenamento do navegador | Inspecionar localStorage/sessionStorage pelas ferramentas de desenvolvedor após o login. | Nenhum dado sensível (senha, CPF, prontuário) armazenado em texto legível. | Vazamento de dados pessoais e de saúde. | Alta |
| Encerramento e expiração de sessão | Deixar o sistema inativo por um período definido (ex.: 30 minutos) e tentar realizar uma ação. | Sessão expira automaticamente e solicita novo login. | Uso indevido do sistema por terceiros em computador desbloqueado. | Média |
| Proteção contra exclusão indevida de registros | Tentar excluir um paciente ou agendamento pelo ícone correspondente. | Sistema solicita confirmação e respeita as permissões do perfil antes de excluir. | Perda ou manipulação indevida de informações importantes. | Média |

### Usabilidade

| O que verificar | Como verificar | Critério esperado | Risco associado | Prioridade |
|---|---|---|---|---|
| Quantidade de etapas para agendar uma consulta | Contar cliques/telas percorridos do início ao fim do processo de agendamento. | Processo concluído em, no máximo, 4 a 5 etapas. | Demora no atendimento durante a recepção. | Alta |
| Clareza das mensagens de sucesso e erro | Realizar ações válidas e inválidas (ex.: salvar agendamento sem preencher campo obrigatório) e observar o retorno do sistema. | Mensagens específicas, indicando claramente o que ocorreu e, em caso de erro, como corrigir. | Confusão do usuário e cadastros incorretos. | Média |
| Confirmação antes de exclusões | Clicar no ícone de exclusão de um paciente ou agendamento. | Sistema exibe confirmação antes de excluir definitivamente o registro. | Exclusão acidental de dados importantes. | Alta |
| Indicação clara de campos obrigatórios | Abrir o formulário de "Novo registro" e observar a marcação visual dos campos obrigatórios. | Campos obrigatórios destacados visualmente antes do envio do formulário. | Tentativas de envio incompletas e retrabalho. | Média |
| Navegação pelo teclado | Percorrer formulários e menus utilizando apenas Tab/Enter, sem uso do mouse. | Todos os campos e botões acessíveis e operáveis via teclado. | Barreiras de acesso para usuários com limitações motoras. | Baixa |

### Compatibilidade

| O que verificar | Como verificar | Critério esperado | Risco associado | Prioridade |
|---|---|---|---|---|
| Funcionamento em diferentes navegadores | Executar os principais fluxos (login, agendamento, cadastro) no Chrome, Firefox e Edge. | Comportamento e layout consistentes nos três navegadores. | Funcionalidades indisponíveis para parte dos usuários. | Alta |
| Funcionamento em diferentes dispositivos | Acessar o sistema em smartphone, tablet e computador. | Interface se adapta e todas as funções permanecem acessíveis. | Tabelas cortadas ou funções inacessíveis em dispositivos móveis. | Alta |
| Comportamento em diferentes resoluções de tela | Simular resoluções de 360 px, 768 px e 1366 px, testando a tela de Agendamentos. | Menu lateral e tabelas se ajustam sem cortar conteúdo. | Colunas da tabela de agendamentos cortadas em telas pequenas. | Alta |
| Exibição correta de acentos e caracteres especiais | Cadastrar nomes com acentuação (ex.: "João", "Ana Clara Souza") e conferir a exibição em telas e relatórios. | Caracteres exibidos corretamente em todas as telas e impressões. | Caracteres ilegíveis, dificultando a identificação do paciente. | Média |
| Impressão de relatórios | Gerar o Relatório financeiro e utilizar a função de impressão do navegador. | Relatório impresso mantém formatação e dados completos. | Relatórios impressos incompletos ou cortados. | Média |

## Parte 3 — Relatório de observações e possíveis defeitos

- Formato de data fora do padrão local: a coluna "DATA" exibe os valores no formato AAAA-MM-DD (ex.: 2026-07-20), enquanto o padrão mais comum para usuários brasileiros é DD/MM/AAAA. Recomenda-se confirmar com a equipe de negócio se o formato é intencional; caso contrário, é um ponto de melhoria de usabilidade/localização.
- Inconsistência visual entre registros da mesma coluna: o nome "Ana Clara Souza" aparece estilizado em azul (padrão visual de link), enquanto "Mariana Costa" e "Lucas Almeida" aparecem em texto padrão (preto). Recomenda-se verificar se essa diferença corresponde a uma funcionalidade real (ex.: link para detalhes do paciente) aplicada de forma incompleta aos demais registros, ou se é apenas inconsistência de estilo — em ambos os casos, deve ser tratada como defeito de padronização visual.
- Agendamentos com data anterior à data atual do sistema: o cabeçalho indica a data atual como "3 de agosto de 2026", mas os três agendamentos listados são do dia "2026-07-20" (mais de duas semanas antes) e continuam com status ativos ("Confirmado"/"Pendente"), sem indicação visual de que já ocorreram. Vale testar se a tela deveria filtrar, destacar ou mover consultas passadas para outra visão, ou se isso decorre apenas do uso de dados fictícios para fins de demonstração.
- Ausência de paginação e de contagem de registros: a tela não exibe controle de paginação nem o total de agendamentos, o que deve ser testado especificamente à medida que o volume de dados crescer.
