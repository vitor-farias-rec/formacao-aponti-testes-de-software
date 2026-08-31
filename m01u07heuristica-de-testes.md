# Atividade Avaliativa: Heurísticas de Testes

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 01 — Unidade 7 |
| **Sistema Analisado** | Clinica PSI (Cenário Fictício)|
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Concluído |

---

> **Objetivo da Atividade:**  
> Escolha duas heurísticas de Nilsen e descreva a implementação destas no projeto, listando falhas, riscos identificados, áreas que merecem mais atenção. Justifique.

---

## **Funcionalidade: Botão do Módulo 'Fornecedores e compras'**

### **Visibilidade do Status do Sistema**

<details>
   <summary> <b>Clique para expandir evidência </b></summary>
   <br>
   
<img width="1913" height="943" alt="Captura de tela 2026-08-31 023329" src="https://github.com/user-attachments/assets/b8950833-4c4c-4c4d-aa07-10678dbb23e6" />

   </details>

### **Falhas**

- Dependência exclusiva de cor para diferenciar sucesso/erro sem texto de apoio ao passar ao interagir com o mouse, o que pode gerar problema para usuários com daltonismo.
- Contorno com tonalidade muito próxima da cor de fundo, podendo confundir usuários sobre qual aba está no cursor do mouse e será selecionada ao clicar.

### **Riscos identificados**

- Não atingir a razão de contraste mínima exigida pela WCAG 2.1 entre o contorno e o fundo.
- Leitor de tela não anunciar a mudança de estado, quebrando a visibilidade para usuários de tecnologia assistiva.

### **Áreas que merecem mais atenção**

Testes de acessibilidade: simulação de daltonismo, leitor de tela.
Testes em diferentes navegadores, incluindo modo escuro e temas de alto contraste do sistema operacional.
Consistência do código de cores em todo o produto (azul escuro sempre = fundo/constraste entre elementos, cinza sempre = contorno/destaque em fontes menores, por exemplo).

### **Justificativa**

Essa heurística exige feedback apropriado, portanto o destaque de contorno cumpre, nesse caso, de forma limitada e com falhas: ele não comunica completamente o que é necessário, sendo que, sem elementos de acessibilidade, a transição entre "nada aconteceu ainda" → "seu cursor está aqui" → "resultado da ação", aumenta a incerteza do usuário sobre se o clique funcionou. É um bom exemplo justamente por ligar diretamente uma mudança visual sutil a um evento real do sistema.

### **Estética e Design Minimalista** → Sucesso!

<details>
   <summary> <b>Clique para expandir evidência </b></summary>
   <br>
   
<img width="1915" height="945" alt="Captura de tela 2026-08-25 113922" src="https://github.com/user-attachments/assets/f362137d-5ced-4328-b41b-a6daecf61969" />

   </details>

### **Possíveis Falhas (não ocorreram)**

- Funcionalidades secundárias poderiam estar escondidas demais, prejudicando a visibilidade, mas não é o caso.
- Botões poderiam estar só com ícone, sem rótulo textual nem tooltip, deixando a função ambígua.

### **Hipóteses de Riscos que poderiam ser identificados**

- Curva de aprendizado maior para usuários novos, se ações relevantes estivessem mal distribuídas.
- Baixo contraste ou fonte reduzida, prejudicando leitura para usuários com baixa visão.

### **Áreas que merecem mais atenção**

- Teste exploratórios com usuários novos, sem tutorial prévio, verificando se encontram as funções secundárias.
- Testes de acessibilidade (contraste, tamanho de fonte, área mínima de toque).
- Validação de ícones sem texto por entrevista, confirmando compreensão real do público-alvo.

### **Justificativa**

A heurística define que a interface não deve conter informação irrelevante ou raramente necessária, já que cada unidade extra de informação compete com as relevantes e reduz sua visibilidade. Ao remover ruído visual, elementos decorativos e opções secundárias da tela principal, a interface direciona a atenção para a tarefa central e agiliza a interação: sem eliminar funcionalidades, apenas reorganizando a prioridade de exposição delas.
