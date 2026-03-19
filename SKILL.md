---
name: istofel-project-plan
description: >
  Guia profissional para criação de documentação completa de produto digital: MVP Scope, PRD e SPEC.
  Use este skill sempre que o usuário quiser definir, planejar ou documentar um produto digital,
  ideia de startup, feature ou sistema — mesmo que mencione apenas "quero criar um produto",
  "me ajuda a planejar isso", "preciso de um PRD", "quero escrever uma spec", "tenho uma ideia
  de app", "como estruturo isso", "me ajuda a pensar no escopo", ou qualquer variação.
  Aplique também quando o usuário apresentar um prompt de produto e pedir refinamento, análise
  crítica ou expansão. Este skill produz documentos técnicos profissionais com análise de mercado,
  decisões de stack, modelagem de dados, regras de negócio, roadmap, user flows, especificação
  por módulo e diagramas de sequência. Sempre gera um documento por vez, solicitando aprovação
  antes de avançar para o próximo: MVP Scope → PRD → SPEC.
---

# istofel_project_plan

Skill profissional para geração de documentação técnica e estratégica de produto digital.

Produz três documentos em sequência obrigatória:
1. **MVP Scope** — visão geral técnica e estratégica
2. **PRD** — requisitos detalhados de produto
3. **SPEC** — especificação técnica de implementação

**Regra de ouro:** Gerar um documento por vez. Ao finalizar cada um, perguntar explicitamente se o usuário deseja prosseguir para o próximo. Nunca pular etapas.

---

## Princípios Gerais

Aplicar em todos os documentos:

- **Pragmatismo de MVP**: velocidade, simplicidade, baixo custo, risco controlado
- **Zero overengineering**: soluções consolidadas, bibliotecas maduras, padrões validados
- **Premissas explícitas**: marcar como `**Premissa**` toda suposição
- **Dados com fonte**: incluir fonte, ano e região; se não existir, indicar `**estimativa**`
- **Perguntas antes do documento**: se houver ambiguidade estrutural, perguntar antes de gerar
- **Sugestão proativa**: identificar e sinalizar pontos omitidos que impactam o produto, marcando com `> 💡 **Sugestão:** [texto]`

---

## Fluxo de Execução

```
1. Receber ideia/prompt do usuário
2. Analisar lacunas críticas → perguntar APENAS o essencial (máx. 5 perguntas)
3. Gerar MVP Scope completo
4. → "Deseja prosseguir para o PRD?"
5. Gerar PRD completo
6. → "Deseja prosseguir para a SPEC?"
7. Gerar SPEC completo
```

---

## Gatilho de Perguntas

Antes de gerar qualquer documento, verificar se faltam informações críticas:

| Área | Perguntar se ausente |
|------|----------------------|
| Público-alvo | Quem usa? Contexto profissional/demográfico? |
| Problema central | Qual dor específica resolve? |
| Modelo de negócio | Como monetiza? B2B, B2C, freemium, SaaS? |
| Distribuição | Web, mobile, desktop, CLI, API? |
| Restrições técnicas | Linguagem preferida, infra existente, tamanho do time? |

Formular no máximo 5 perguntas, agrupadas. Não perguntar o que pode ser inferido com segurança.

---

## Regras de Sugestão Proativa

Sempre que o usuário não mencionar os itens abaixo, sinalizar no documento:

| Item omitido | Ação |
|--------------|------|
| Estratégia de retenção | Sugerir mecanismos de engajamento |
| Segurança de dados / privacidade | Sugerir LGPD/GDPR se produto brasileiro/europeu |
| Observabilidade | Sugerir logging, error tracking, métricas |
| Internacionalização | Perguntar se é relevante |
| Acessibilidade | Mencionar WCAG se produto público |
| Testes | Sugerir estratégia mínima |
| Licença | Mencionar necessidade de definição |
| Rollback/fallback | Sinalizar se há dependências críticas de terceiros |

---

## Etapa 1 — MVP Scope

Consulte `/references/mvp-scope.md` para estrutura completa.

---

## Etapa 2 — PRD

> Gerar apenas após aprovação explícita do MVP Scope.

Consulte `/references/prd.md` para estrutura completa.

---

## Etapa 3 — SPEC

> Gerar apenas após aprovação explícita do PRD.

Consulte `/references/spec.md` para estrutura completa.
