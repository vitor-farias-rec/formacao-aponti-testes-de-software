# Atividade Avaliativa: 2 — Simulando Fluxos com Ações

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

### Pergunta

Escreva um pequeno trecho de código simulando o preenchimento de um formulário de filtro de produtos, seguindo as boas práticas da aula:

1. Preencher o campo de busca (`#busca`) com "Notebook" (usando `fill` ao invés de `type`).
2. Selecionar, no dropdown com `data-test="filtro-preco"`, a opção com value `"menor-preco"`.
3. Clicar no botão de buscar usando `getByRole` para um botão com o nome "Buscar".

**Resposta:**

```js
// Preencher o campo de busca usando fill
await page.locator('#busca').fill('Notebook');

// Selecionar a opção do dropdown pelo value
await page.locator('[data-test="filtro-preco"]').selectOption({ value: 'menor-preco' });

// Clicar no botão usando localizador semântico
await page.getByRole('button', { name: 'Buscar', exact: true }).click();
```
