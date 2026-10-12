# Atividade Avaliativa: 1 — Refatorando Localizadores Frágeis

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

No material, vimos que a escolha do localizador determina se o seu teste vai quebrar facilmente quando o layout da página mudar. Imagine que você pegou um teste legado para revisar e encontrou a seguinte linha de código:

```js
await page.locator('.container .form-group:nth-child(3) button').click();
```

### Pergunta 1

Explique, com suas palavras, por que o uso de seletores CSS estruturais (como `nth-child` ou classes genéricas) é considerado frágil.

**Resposta:** O seletor `.container .form-group:nth-child(3) button` é frágil porque descreve onde o botão está na página, e não o que ele é, isto é, o trecho informa que o botão está dentro do terceiro grupo do formulário, mas não especifica que este é o botão "Enviar". Além disso, as classes `.container` e `.form-group` pertencem ao design e podem trocar de nome ou ser reorganizadas quebrando os testes, o que resulta em falsos alarmes, por isso cadeias longas de CSS são um exemplo de má prática que pode gerar testes instáveis. Portanto, o problema em si não é "usar CSS", e sim depender de posição e estrutura: um `#id` estável é aceitável; um caminho que acompanha o layout, não.

### Pergunta 2

Qual é a estratégia de localização mais robusta recomendada pelo material para automação?

**Resposta:** Utilizar `getByTestId` porque ele separa o comportamento do teste das alterações de layout e garante que irá continuar funcionando independentemente do botão mudar de cor, de posição ou de texto, porque o `data-testid` permanece o mesmo. Depois dele `page.getByRole()` porque esse localizador encontra o elemento pelo papel dele e pelo nome (ex.: `getByRole('button', { name: 'Enviar' })`) e funciona porque também não é refém de mudanças de layout, classe e cor; também dá para utilizar `getByLabel`, `getByPlaceholder`, `getByText`, `getByAltText` e `getByTitle`, pois são baseados em texto que o usuário realmente vê. CSS e XPath ficam por último, aceitáveis apenas quando nada acima resolve, e sempre evitando cadeias amarradas à estrutura da inspeção de elementos da página.

### Pergunta 3

Reescreva essa linha de código utilizando a melhor prática recomendada, assumindo que o desenvolvedor adicione o atributo `data-test="btn-enviar"` no botão.

**Resposta:** Assumindo que foi configurado `use: { testIdAttribute: 'data-test' }` no `playwright.config.ts` ou foi chamado `selectors.setTestIdAttribute('data-test')` a forma mais limpa de reescrever a linha de código é:

```js
await page.getByTestId('btn-enviar').click();
```

Também dá para usar `await page.locator('[data-test="btn-enviar"]').click();` justamente porque essa opção não depende do pré-ajuste no config.
