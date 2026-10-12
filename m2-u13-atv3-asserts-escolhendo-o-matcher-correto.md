# Atividade Avaliativa: 3 — Asserts, Escolhendo o Matcher Correto

> Documento referente à atividade da **Trilha de Testes de Software** na **4ª Edição da Formação Acelerada em Programação (FAP)**.

---

| Informação | Detalhe |
| :--- | :--- |
| **Programa** | FAP 4ª Edição — SOFTEX PE (Coordenação) / Aponti (Execução) |
| **Trilha** | Testes de Software |
| **Módulo / Unidade** | Módulo 02 — Unidade 13 |
| **Turma / Período** | 4 / Manhã |
| **Autor** | Vitor Pontes de Farias ([@vitor-farias-rec](https://github.com/vitor-farias-rec)) |
| **Status** | Concluído |

---

## Contexto

Após cadastrar um usuário, o sistema exibe: "Cadastro realizado com sucesso! Protocolo: 987654". O número do protocolo é gerado dinamicamente.

### Pergunta 1

Qual dos dois asserts (`toHaveText` ou `toContainText`) você deve utilizar para validar essa mensagem?

**Resposta:** O correto é o `toContainText`, porque esse valida apenas um trecho da mensagem, a parte fixa "Cadastro realizado com sucesso!", e ignora o número do protocolo, que muda a cada execução; já o `toHaveText` compara o texto por inteiro e com o protocolo estabelecido no código, o que reprovaria assim que o sistema gerasse um número diferente do esperado.

### Pergunta 2

Justifique sua escolha com base nas características de cada comando mostradas no material.

**Resposta:** O `toHaveText()` verifica o texto do elemento como um todo: com um valor em string, exige igualdade com o conteúdo e, como o protocolo é dinâmico ("987654" hoje, outro número amanhã), fixar o texto completo tornaria a reprovação inevitável; já o `toContainText()` verifica se o elemento contém o trecho esperado, aceitando tanto uma substring quanto uma expressão regular, o que permite validar o que é estável e deixar o dinâmico fora dos pontos de checagem (os asserts).

### Pergunta 3

Escreva como ficaria a linha de código do assert, assumindo que a mensagem aparece em um elemento com a classe `.toast-message`.

**Resposta:**

```js
await expect(page.locator('.toast-message')).toContainText(/Protocolo: \d+/);
```
