# CLAUDE.md — Referência de Geração

> Gerar automaticamente a partir dos três documentos anteriores. Sem perguntas ao usuário.
> Meta: menos de 200 palavras no arquivo gerado. Incluir apenas o que for diretamente acionável.
> Omitir seções sem dados reais (ex: auth em produto single-user, env vars se não houver).

---

## Estrutura do CLAUDE.md Gerado

**Seção 1 — Projeto** *(extrair do MVP Scope)*
- Nome · descrição em uma frase · stack resumida
- Caminhos: `docs/mvp-scope.md` · `docs/prd.md` · `docs/spec.md`

**Seção 2 — Padrões de Código** *(extrair da SPEC — convenções globais)*
- Linguagem/versão · formatter · linter · padrão de IDs e timestamps · classe base de exceções
- Padrões específicos de DB, auth e API — somente os que existirem no projeto
- Formato: lista de regras diretas, sem prosa

**Seção 3 — NUNCA Fazer** *(extrair de ADRs + regras de Autorização do PRD)*
- Proibições explícitas prefixadas com `NUNCA`
- Máximo 10 itens — apenas o que o agente poderia razoavelmente tentar fazer
- Incluir ao final: `- Quando cometer um erro, registrar aqui a correção`

**Seção 4 — Stack Fixada** *(extrair dos ADRs da SPEC)*
- Tabela: Camada | Tecnologia | Versão mínima | ADR
- Nota inline: "Decisões fechadas via ADR. Não propor alternativas."

**Seção 5 — Diretórios** *(extrair da SPEC — estrutura do projeto)*
- Árvore mínima: apenas pastas de primeiro nível com comentário de responsabilidade
- Sem detalhamento de arquivos individuais

**Seção 6 — Invariantes** *(extrair da SPEC — INV-XX)*
- Formato compacto por invariante:
  `INV-XX [Tipo] Descrição · Verificar em: [local]`

**Seção 7 — Build** *(extrair da SPEC — sequência de build)*
- Passos numerados com checkpoint em uma linha cada
- Campo: `Passo atual: X`

**Seção 8 — Env Vars** *(extrair da SPEC — constantes globais; omitir se não houver)*
- Tabela: Variável | Padrão | Obrigatória

**Seção 9 — Edge Cases** *(extrair do PRD — casos extremos respondidos)*
- Tabela: Situação | Comportamento esperado
- Máximo 6 itens — apenas os que o agente poderia implementar errado

**Seção 10 — Glossário** *(extrair do PRD — glossário; omitir se trivial)*
- Tabela: Termo | Usar como | NÃO usar

---

## Checklist de qualidade — CLAUDE.md

- [ ] Menos de 200 palavras no arquivo gerado?
- [ ] Toda informação extraída dos documentos (sem invenção)?
- [ ] Seções sem dados reais foram omitidas?
- [ ] Invariantes em formato compacto com local de verificação?
- [ ] "NUNCA fazer" inclui instrução de registro de erros?
- [ ] Glossário omitido se termos forem autoexplicativos?
