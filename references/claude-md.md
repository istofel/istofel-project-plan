# CLAUDE.md — Referência de Geração

> Gerar automaticamente a partir dos três documentos anteriores. Sem perguntas ao usuário.
> Meta: menos de 250 palavras no arquivo gerado. Incluir apenas o que for diretamente acionável.
> Omitir seções sem dados reais (ex: auth em produto single-user, design tokens em CLI, env vars se não houver).

---

## Estrutura do CLAUDE.md Gerado

**Seção 1 — Projeto** *(extrair do MVP Scope)*
- Nome · descrição em uma frase · stack resumida
- Caminhos: `docs/mvp-scope.md` · `docs/prd.md` · `docs/spec.md`

**Seção 2 — Comandos** *(extrair da SPEC — comandos de desenvolvimento)*
- Copiar os comandos literais: setup · dev · build · lint · format · testes (unit/integração/E2E) · migrações
- Formato compacto: `Finalidade: comando` — uma linha cada
- Omitir linhas sem comando definido na SPEC

**Seção 3 — Padrões de Código** *(extrair da SPEC — convenções globais)*
- Linguagem/versão · formatter · linter · nomes de arquivo · padrão de IDs e timestamps · classe base de exceções
- Padrões específicos de DB, auth, API e transporte — somente os que existirem no projeto
- Formato: lista de regras diretas, sem prosa

**Seção 4 — NUNCA Fazer** *(extrair de ADRs + regras de Autorização do PRD)*
- Proibições explícitas prefixadas com `NUNCA`, com referência ao ADR ou INV que a origina
- Máximo 10 itens — apenas o que o agente poderia razoavelmente tentar fazer
- Incluir ao final: `- Quando cometer um erro, registrar aqui a correção`

**Seção 5 — Stack Fixada** *(extrair dos ADRs da SPEC)*
- Tabela: Camada | Tecnologia | ADR
- Nota inline: "Decisões fechadas via ADR. Não propor alternativas."

**Seção 6 — Diretórios** *(extrair da SPEC — estrutura do projeto)*
- Árvore mínima: apenas pastas de primeiro nível com comentário de responsabilidade
- Sem detalhamento de arquivos individuais

**Seção 7 — Design Tokens** *(extrair da SPEC — design tokens; omitir se produto sem UI)*
- Apenas os valores que o agente usaria errado se não soubesse: cores da marca, cores semânticas do domínio, fontes
- Formato: `token: #hex — uso` — uma linha cada
- Incluir onde os tokens vivem no código (arquivo de config)

**Seção 8 — Invariantes** *(extrair da SPEC — INV-XX)*
- Formato compacto por invariante:
  `INV-XX [Tipo] Descrição · Verificar em: [local]`

**Seção 9 — Build** *(extrair da SPEC — sequência de build)*
- Passos numerados com checkpoint em uma linha cada
- Campo: `Passo atual: X`

**Seção 10 — Env Vars** *(extrair da SPEC — constantes globais; omitir se não houver)*
- Tabela: Variável | Padrão | Obrigatória

**Seção 11 — Edge Cases** *(extrair do PRD — casos extremos respondidos)*
- Tabela: Situação | Comportamento esperado
- Máximo 6 itens — apenas os que o agente poderia implementar errado

**Seção 12 — Glossário** *(extrair do PRD — glossário; omitir se trivial)*
- Tabela: Termo | Usar como | NÃO usar

---

## Checklist de qualidade — CLAUDE.md

- [ ] Menos de 250 palavras no arquivo gerado?
- [ ] Comandos de desenvolvimento copiados literalmente da SPEC?
- [ ] Toda informação extraída dos documentos (sem invenção)?
- [ ] Seções sem dados reais foram omitidas?
- [ ] Design tokens incluídos apenas se o produto tem UI?
- [ ] Invariantes em formato compacto com local de verificação?
- [ ] Proibições referenciam o ADR ou INV que as origina?
- [ ] "NUNCA fazer" inclui instrução de registro de erros?
- [ ] Glossário omitido se termos forem autoexplicativos?
