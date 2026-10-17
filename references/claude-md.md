# CLAUDE.md — Referência de Geração

> Gerar automaticamente a partir dos três documentos anteriores. Sem perguntas ao usuário.
> Alvo: ~150 palavras. Teto: 200 palavras, incluindo a seção fixa de Execução.
> Omitir qualquer seção sem dados reais.

---

## Princípio: só o que o agente não consegue descobrir

O CLAUDE.md é carregado em toda sessão e o agente segue o que está nele. Cada linha custa tokens
e induz passos extras de exploração. Pesquisa da ETH Zürich (Gloaguen et al., 2026, "Evaluating
AGENTS.md") mostrou que arquivos de contexto gerados por LLM reduziram a taxa de sucesso e
aumentaram o custo de inferência em mais de 20%. As causas: visões gerais do repositório não
ajudaram o agente a encontrar arquivos mais rápido, e o conteúdo era redundante com a
documentação já existente.

**Teste de cada linha:** o agente erraria sem ela, mesmo podendo ler o código e a pasta `docs/`?
- Se **não** → cortar. Está no código, nos arquivos de config ou em `docs/spec.md`.
- Se **sim** → manter. É uma decisão, restrição ou exceção que não aparece em lugar nenhum.

| Manter (não descobrível) | Cortar (descobrível) |
|--------------------------|----------------------|
| Comandos fora do padrão do ecossistema | Comandos padrão (`npm run dev`, `pytest`, `npm run build`) |
| Proibições com origem em ADR ou INV | Árvore de diretórios |
| Decisões de stack que o agente tentaria reverter | Tabela de stack (visível nas dependências) |
| Convenções que contradizem o padrão do framework | Formatter, linter, estilo (visíveis nos arquivos de config) |
| Invariantes de domínio | Lista de variáveis de ambiente (está no `.env.example`) |
| Casos extremos com comportamento não óbvio | Design tokens (estão na SPEC e no config de estilo) |
| Passo atual da sequência de build | Glossário de termos autoexplicativos |

> Os documentos em `docs/` continuam completos — a SPEC mantém diretórios, stack, tokens e
> variáveis. O CLAUDE.md não os repete; aponta para onde estão.

---

## Estrutura do CLAUDE.md Gerado

**Seção 1 — Projeto** *(extrair do MVP Scope)*
- Nome e descrição em uma frase
- Uma linha de ponteiros: `Docs: docs/prd.md (regras) · docs/spec.md (arquitetura, tokens §7, estrutura §3) · .env.example`
- Não listar stack — fica nas dependências e nos ADRs

**Seção 2 — Comandos** *(extrair da SPEC — comandos de desenvolvimento)*
- Incluir **apenas** comandos que o agente não adivinharia:
  - comandos com flags obrigatórias ou ordem específica
  - setup em múltiplos passos (ex: subir Docker antes do dev server)
  - testes fora do padrão (ex: testes de RLS, pgTAP, E2E com ambiente próprio)
  - migrações e seeds
  - comandos que contradizem o padrão (ex: `pnpm` em vez de `npm`, `uv` em vez de `pip`)
- Omitir os que seguem a convenção do ecossistema
- Formato: `Finalidade: comando` — uma linha cada

**Seção 3 — Regras Não Óbvias** *(extrair da SPEC — convenções globais e ADRs)*
- Apenas convenções que contradizem o padrão do framework ou que não aparecem em nenhum config
  - Ex: "Server Actions como transporte padrão; Route Handlers só para cron e webhooks"
  - Ex: "Código em inglês, textos de interface em português"
- Máximo 5 itens. Omitir a seção se não houver

**Seção 4 — NUNCA** *(extrair de ADRs + regras de Autorização do PRD)*
- Proibições prefixadas com `NUNCA`, cada uma com o ADR ou INV de origem
- Incluir aqui as decisões de stack que o agente poderia tentar reverter
  (ex: `NUNCA trocar dnd-kit por outra lib de drag-and-drop (ADR-11)`) — substitui a tabela de stack
- Máximo 10 itens — apenas o que o agente poderia razoavelmente tentar fazer
- Última linha sempre: `- Quando cometer um erro, registrar aqui a correção`

**Seção 5 — Invariantes** *(extrair da SPEC — INV-XX)*
- Uma linha por invariante: `INV-XX [Tipo] Descrição · onde verificar`
- Copiar o sentido da SPEC — nunca reescrever a regra

**Seção 6 — Casos Extremos** *(extrair do PRD — casos extremos respondidos)*
- Formato: `Situação → comportamento`
- Máximo 6 itens — apenas os que o agente implementaria errado

**Seção 7 — Build** *(extrair da SPEC — sequência de build)*
- Passos numerados com checkpoint em uma linha cada
- Campo: `Passo atual: X`

**Seção 8 — Glossário** *(extrair do PRD — glossário; omitir na maioria dos projetos)*
- Apenas termos em que o nome ingênuo está errado: `Termo → usar X · não usar Y`
- Máximo 4 itens

**Seção 9 — Execução** *(seção fixa — copiar literalmente em todo projeto)*

```
## Execução
- Toque apenas no que o pedido exige — não refatore, reformate nem "melhore" código adjacente
- Remova só os órfãos que suas mudanças criaram; código morto pré-existente, apenas mencione
- Ambiguidade: declare o que está confuso e pergunte antes de implementar
- Interpretações múltiplas: apresente-as, não escolha em silêncio
- Combine com o estilo existente do arquivo, mesmo que você faria diferente
```

> Única seção não extraída dos documentos. Corrige falhas de comportamento recorrentes em agentes
> de código e vale para qualquer projeto.

---

## Como o arquivo deve evoluir

O CLAUDE.md gerado é o ponto de partida, não a versão final. Ele cresce por falha observada:
quando o agente erra algo que este arquivo poderia ter evitado, adicionar a correção como uma
linha em NUNCA ou Regras Não Óbvias. Linhas que nunca evitaram um erro são candidatas a remoção.

---

## Checklist de qualidade — CLAUDE.md

- [ ] Até 200 palavras, incluindo a seção de Execução?
- [ ] Cada linha passa no teste "o agente erraria sem ela"?
- [ ] Nenhuma árvore de diretórios, tabela de stack, lista de env vars ou design tokens?
- [ ] Comandos restritos aos que fogem do padrão do ecossistema?
- [ ] Decisões de stack reversíveis expressas como proibições com ADR de origem?
- [ ] Toda informação extraída dos documentos (sem invenção)?
- [ ] Invariantes com o mesmo sentido da SPEC e local de verificação?
- [ ] "NUNCA" termina com a instrução de registro de erros?
- [ ] Seção fixa de Execução copiada literalmente?
- [ ] Seções sem dados reais omitidas?
