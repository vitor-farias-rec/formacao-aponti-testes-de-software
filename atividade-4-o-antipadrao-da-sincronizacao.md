# Atividade Avaliativa: 4 — O Antipadrão da Sincronização

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

No CI/CD, um elemento demorou a carregar e o teste falhou. Um colega "corrigiu" adicionando:

```js
await page.waitForTimeout(5000);
```

### Pergunta 1

Explique por que o uso do `waitForTimeout()` é classificado como antipadrão. O que acontece com o teste quando a página é rápida e quando ela é muito lenta?

**Resposta:** O `waitForTimeout()` espera um tempo fixo, sem olhar a página, então não tem como saber se o elemento apareceu. Se o carregamento da página for rápido e o elemento surgir em, por exemplo, 1 segundo, o teste fica parado os outros 4 segundos restantes; com a página lenta, ele é insuficiente: se o elemento levar 7 segundos, o tempo acaba antes e o teste falha de novo. E quando o passo seguinte é uma ação ou assert com auto-wait (um `click()`, por exemplo), a espera fixa insuficiente nem chega a quebrar o teste, porque o auto-wait "segura" a execução sozinho, ou seja, o problema fica mascarado. Por isso, o mais recomendado é utilizar asserts como `await expect(locator).toBeVisible()` e `locator.waitFor()`, ou simplesmente nada, se a próxima ação já tiver auto-wait, porque aumentar o número do timeout não resolve, pois continua sendo um chute fixo, só que mais lento.

### Pergunta 2

O Playwright possui um mecanismo de Auto-wait. O que esse mecanismo faz antes de executar uma ação como um `click()`?

**Resposta:** Antes de executar o clique, o Playwright realiza as verificações de actionability e fica repetindo-as até todas passarem ou até estourar o tempo limite (quando então a ação falha com `TimeoutError`): (1) o localizador precisa encontrar exatamente um elemento, porque esse tem que existir na página (se ainda não foi criado, o Playwright continua esperando, o que cobre a antiga checagem de "anexado ao inspecionar elemento") e ser único, pois se encontrar dois ele avisa, acusando o erro "strict mode violation", em vez de clicar em um no chute; (2) o elemento precisa estar visível, isto é, ocupar espaço na tela — escondido com `display: none`, `visibility: hidden` ou de tamanho zero não vale; (3) nada pode estar por cima dele na hora do clique; e (4) precisa estar habilitado, sem `disabled`. Só quando as quatro condições são verdadeiras o clique acontece — é por isso que, na maioria dos casos, não é preciso escrever nenhuma espera manual.

### Pergunta 3

Como você reescreveria essa espera de forma inteligente, utilizando `expect().toBeVisible()` para aguardar o elemento `.tabela-resultados`?

**Resposta:**

```js
await expect(page.locator('.tabela-resultados')).toBeVisible();
```
