# PRD — Product Requirements Document

> Gerado após aprovação do MVP Scope.

---

## Estrutura do PRD

---

### 1. Visão Geral do Produto

- O que é (descrição em 2–3 frases)
- Para quem
- Problema que resolve
- Proposta de valor central
- **Princípios de design** (3–5 princípios que guiam decisões de produto e servem de critério de desempate)

---

### 2. Objetivos e Métricas de Sucesso

**Objetivos do MVP**

| Objetivo | Descrição |
|----------|-----------|

**Métricas de sucesso**

| Métrica | Alvo MVP | Como medir |
|---------|----------|------------|

> Se não houver dados de mercado, indicar como **estimativa**.

---

### 3. Personas e Casos de Uso

Para cada persona definida no MVP Scope:

**Perfil:** nome fictício, idade, cargo, OS/hardware, contexto profissional

**Casos de uso:**
- UC-XX: [descrição curta da ação]

**Mapa de Casos de Uso**

| ID | Caso de Uso | Persona | Feature Relacionada | Prioridade |
|----|-------------|---------|---------------------|------------|

> Preencher o mapa antes de escrever os requisitos funcionais — é o contrato entre personas e features.

---

### 4. Requisitos Funcionais

Para cada RF:

```
RF-XX: [Nome da Feature]

Descrição: [o que o sistema deve fazer]

Critérios de aceite:
- [condição verificável]
- [condição verificável]

Regras:
- [descrição da regra] | Tipo: Validação
- [descrição da regra] | Tipo: Invariante
- [descrição da regra] | Tipo: Transição de Estado
- [descrição da regra] | Tipo: Autorização

Tratamento de erros:
- [situação de falha] → [comportamento esperado]
```

**Tipos de regra de negócio — definições:**

| Tipo | Definição | Onde verificar no código |
|------|-----------|--------------------------|
| **Validação** | Condição que deve ser verdadeira *antes* de uma ação acontecer. Se não atendida, a ação é bloqueada. | Na entrada da ação / use case |
| **Invariante** | Verdade que deve ser válida *a qualquer momento*, independente de qual ação chegou lá. Se falsa, há um bug. | Pode ser verificada no banco a qualquer hora |
| **Transição de Estado** | Define quando um objeto muda de estado e o que acontece como consequência dessa mudança. | Na camada de domínio / state machine |
| **Autorização** | Define quem pode fazer o quê, sob quais condições — verifica o ator e seu poder, não o estado dos dados. | Middleware / camada de autorização |

> Classificar cada regra por tipo elimina ambiguidade para o agente: ele sabe onde no código a regra deve ser implementada e verificada.

> Critérios de aceite devem ser verificáveis — evitar descrições vagas.

---

### 5. Requisitos Não-Funcionais

| Categoria | Requisito | Métrica | Prioridade |
|-----------|-----------|---------|------------|
| Performance | Tempo de inicialização/resposta | ≤ Xs | Alta |
| Compatibilidade | Plataformas / versões suportadas | [lista] | Alta |
| Armazenamento | Estimativa de uso em disco/memória | ~X MB | Média |
| Segurança/Privacidade | Comunicação externa, dados sensíveis | [política] | Alta |
| Usabilidade | Ações principais acessíveis em | ≤ 2 cliques | Média |
| Manutenibilidade | Cobertura de testes mínima | ≥ X% | Média |

---

### 6. Arquitetura de Informação e Navegação

**6.1 Layout principal** — ASCII art da estrutura visual:

```
┌─────────────────────────────────────────────┐
│  ÁREA A               │  ÁREA B              │
│  [elementos]          │  [elementos]         │
└─────────────────────────────────────────────┘
```

**6.2 Hierarquia de páginas/rotas**

| Área | Conteúdo | Tipo |
|------|----------|------|
| | | Single-page / Multi-page / Modal |

**6.3 Estados da tela**

| Estado | Gatilho | O que aparece |
|--------|---------|---------------|
| Offline/Erro | Serviço indisponível | Tela de orientação com CTA |
| Vazio | Sem dados ainda | Empty state com instrução |
| Pronto | Estado normal | Interface completa |
| Carregando | Operação em andamento | Spinner / barra de progresso |
| Erro de operação | Falha em ação | Banner com mensagem descritiva + como resolver |

> Documentar todos os estados evita lacunas de UI que só aparecem em produção.

---

### 7. User Flows Detalhados

Para cada fluxo crítico, usar pseudofluxograma em texto:

```
[Ponto de entrada]
    │
    ▼
[Ação do usuário]
    │
    ▼
[Decisão ou validação] ──── Falha ──── [Comportamento de erro]
    │
    │ Sucesso
    ▼
[Próximo passo]
    │
    ▼
[Resultado final / estado pós-fluxo]
```

Fluxos obrigatórios:
- UF-01: Primeiro uso / Onboarding
- UF-02: Fluxo principal de uso (happy path)
- UF-03: Fluxo com dado/arquivo externo (se aplicável)
- UF-04: Fluxo de configuração ou troca de contexto

---

### 8. Especificação de Features

Para cada feature relevante, detalhar além dos RFs:

```
Feature: [nome]

Descrição: [o que faz e por quê]

Componentes internos:
- [componente / classe / serviço]
- [componente / classe / serviço]

Regras:
- [regra de negócio específica] | Tipo: [Validação / Invariante / Transição de Estado / Autorização]
- [limite ou restrição] | Tipo: [Validação / Invariante / Transição de Estado / Autorização]

Tratamento de erros:
- [condição] → [comportamento]

Limitações do MVP:
- [o que não será coberto nesta versão e por quê]
```

---

### 9. Modelo de Dados

**Schema do banco principal:**

```sql
CREATE TABLE [entidade] (
    id         TEXT PRIMARY KEY,   -- UUID v4
    -- campos de negócio
    created_at TEXT NOT NULL,      -- ISO 8601 UTC
    updated_at TEXT NOT NULL       -- ISO 8601 UTC
);

CREATE INDEX idx_[entidade]_[campo] ON [entidade]([campo]);
```

**Constraints e enums:** documentar CHECK constraints e valores possíveis de campos de status/tipo.

**Relacionamentos:** descrever cardinalidade e FKs com comportamento ON DELETE.

**Bancos secundários** (vetorial, cache, etc.): documentar estrutura de coleções/índices separadamente.

**Estratégia de migração:** descrever abordagem atual (ex: `IF NOT EXISTS` idempotente) e quando evoluir para versionamento sequencial.

---

### 10. Integrações e Dependências

**Dependências de runtime**

| Dependência | Versão Mínima | Tipo | Propósito |
|-------------|---------------|------|-----------|

**Endpoints/APIs consumidas**

| Endpoint | Método | Propósito | Quando usado |
|----------|--------|-----------|--------------|

---

### 11. Requisitos de Instalação e Distribuição

> Relevante para produtos distribuíveis (CLI, desktop, biblioteca). Omitir para SaaS puro.

- Pré-requisitos do usuário final
- Comando de instalação recomendado
- Estrutura de diretórios do projeto (árvore com comentário por arquivo/pasta)
- Entry point e como é invocado
- Diretório de dados do usuário (configs, banco, uploads)

---

### 12. Roadmap e Priorização

**Sprint plan**

| Sprint | Semanas | Features (RF-IDs) | Entregável |
|--------|---------|-------------------|------------|

**Pós-MVP v1**

| Feature | Prioridade |
|---------|------------|

---

### 13. Fora de Escopo (MVP)

| Item | Motivo da exclusão |
|------|--------------------|

---

### 14. Riscos e Mitigações

| # | Risco | Prob. | Impacto | Mitigação |
|---|-------|-------|---------|-----------|

---

### 15. Critérios de Aceite Globais

O MVP é considerado completo quando:

```
1. ✅ [critério verificável — instalação/setup]
2. ✅ [critério verificável — fluxo principal]
3. ✅ [critério verificável — feature core 1]
4. ✅ [critério verificável — feature core 2]
5. ✅ [critério verificável — privacidade/segurança]
6. ✅ [critério verificável — documentação]
```

> Critérios globais são o "definition of done" do MVP — binários (passou / não passou).

---

### 16. Glossário

| Termo | Definição |
|-------|-----------|

---

### Checklist de qualidade — PRD

- [ ] Princípios de design declarados?
- [ ] Mapa de casos de uso liga personas a features?
- [ ] Todos os RFs têm critérios de aceite verificáveis?
- [ ] Todas as regras de negócio dos RFs estão classificadas por tipo (Validação / Invariante / Transição / Autorização)?
- [ ] Tratamento de erros definido por feature?
- [ ] Estados da tela documentados (incluindo vazios e erros)?
- [ ] User flows cobrem onboarding e happy path com ramificações de erro?
- [ ] Especificação de features detalha componentes, regras tipadas e limitações do MVP?
- [ ] Schema de dados com índices, constraints e estratégia de migração?
- [ ] Requisitos de instalação/distribuição cobertos (se aplicável)?
- [ ] Critérios de aceite globais são binários e verificáveis?
- [ ] Fora de escopo explicitado com motivo?
- [ ] Roadmap em sprints com entregáveis claros?

---

**Ao finalizar:** perguntar — *"Deseja prosseguir para a SPEC?"*
