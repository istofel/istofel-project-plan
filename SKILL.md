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
  antes de avançar para o próximo: MVP Scope → PRD → SPEC → CLAUDE.md.
---

# istofel_project_plan

Skill profissional para geração de documentação técnica e estratégica de produto digital.

Produz quatro documentos em sequência obrigatória:
1. **MVP Scope** — visão geral técnica e estratégica
2. **PRD** — requisitos detalhados de produto
3. **SPEC** — especificação técnica de implementação
4. **CLAUDE.md** — contexto de sessão personalizado para o agente de IA

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
1.  Receber ideia/prompt do usuário
2.  Analisar lacunas críticas → perguntar APENAS o essencial (máx. 5 perguntas)
3.  Gerar MVP Scope completo
4.  → Oferecer download do MVP Scope como arquivo .md
5.  → "Deseja prosseguir para o PRD?"
6.  Gerar PRD completo
7.  → Oferecer download do PRD como arquivo .md
8.  → "Deseja prosseguir para a SPEC?"
9.  Gerar SPEC completo
10. → Oferecer download da SPEC como arquivo .md
11. → "Deseja prosseguir para o CLAUDE.md?"
12. Gerar CLAUDE.md personalizado com dados extraídos dos documentos anteriores
13. → Oferecer download do CLAUDE.md
14. → Informar que o processo de documentação está completo
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

## Entrega de Arquivos .md

Ao finalizar cada documento, além de exibir o conteúdo na conversa, oferecer o download
do documento como arquivo Markdown. Seguir este comportamento:

**Se a ferramenta `create_file` + `present_files` estiver disponível (Claude com computer use):**
- Salvar o conteúdo gerado em `/mnt/user-data/outputs/[nome-do-documento].md`
  - MVP Scope → `mvp-scope.md`
  - PRD → `prd.md`
  - SPEC → `spec.md`
  - CLAUDE.md → `CLAUDE.md`
- Chamar `present_files` com o caminho do arquivo para disponibilizar o download ao usuário

**Se `create_file` + `present_files` NÃO estiver disponível (Claude.ai padrão):**
- Informar ao usuário que o download direto não está disponível no ambiente atual
- Orientar a copiar o conteúdo do bloco Markdown exibido na conversa e salvar manualmente como `.md`

A oferta de download deve aparecer ANTES da pergunta de continuação para a próxima etapa,
na seguinte ordem ao finalizar cada documento:

```
[conteúdo do documento gerado]

📄 O documento está disponível para download acima (ou copie o conteúdo acima e salve como .md).

Deseja prosseguir para o [próximo documento]?
```

---

## Etapa 1 — MVP Scope

Consulte `/references/mvp-scope.md` para estrutura completa.

Ao finalizar:
1. Oferecer download como `mvp-scope.md`
2. Perguntar — *"Deseja prosseguir para o PRD?"*

---

## Etapa 2 — PRD

> Gerar apenas após aprovação explícita do MVP Scope.

Consulte `/references/prd.md` para estrutura completa.

Ao finalizar:
1. Oferecer download como `prd.md`
2. Perguntar — *"Deseja prosseguir para a SPEC?"*

---

## Etapa 3 — SPEC

> Gerar apenas após aprovação explícita do PRD.

Consulte `/references/spec.md` para estrutura completa.

Ao finalizar:
1. Oferecer download como `spec.md`
2. Perguntar — *"Deseja prosseguir para o CLAUDE.md?"*

---

## Etapa 4 — CLAUDE.md

> Gerar apenas após aprovação explícita da SPEC.
> Não fazer perguntas ao usuário — todas as informações necessárias já foram coletadas nas etapas anteriores.

Consulte `/references/claude-md.md` para estrutura completa.

O CLAUDE.md é gerado automaticamente a partir dos dados dos três documentos anteriores:

| Seção do CLAUDE.md | Fonte |
|--------------------|-------|
| Stack e versões | MVP Scope — seção de stack tecnológica |
| ADRs fechados | SPEC — seção de ADRs |
| Estrutura de diretórios | SPEC — seção de estrutura do projeto |
| Invariantes críticas | SPEC — seção de máquinas de estado e invariantes |
| Regras de autorização | PRD — regras tipadas como Autorização |
| Sequência de build | SPEC — seção de sequência de build |
| Variáveis de ambiente | SPEC — seção de constantes globais |
| Casos extremos | PRD — seção de edge cases e fora de escopo |
| Glossário | PRD — glossário |

**Incluir apenas o que for diretamente acionável pelo agente.** Não duplicar conteúdo dos documentos de origem — referenciar quando necessário.

Ao finalizar:
1. Oferecer download como `CLAUDE.md`
2. Informar que o processo de documentação está completo:
   *"Documentação completa: mvp-scope.md · prd.md · spec.md · CLAUDE.md"*
