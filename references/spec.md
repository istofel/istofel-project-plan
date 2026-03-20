# SPEC — Technical Specification

> Gerado após aprovação do PRD. Especificação técnica diretamente implementável.

---

## Estrutura da SPEC

---

### 1. Visão Técnica Geral

**1.1 Arquitetura** — diagrama ASCII mostrando componentes, camadas e protocolos:

```
[Cliente] ←protocolo→ [Serviço Principal]
                            │
                    ┌───────┴────────────┐
                    │  Camada de App      │
                    │  ┌───┐ ┌───┐ ┌───┐ │
                    │  │mod│ │mod│ │mod│ │
                    │  └─┬─┘ └─┬─┘ └─┬─┘ │
                    └────┼─────┼─────┼───┘
                    ┌────┼─────┼─────┼───┐
                    │  Camada de Dados    │
                    │  ┌────┐ ┌───────┐  │
                    │  │ DB │ │ Cache │  │
                    │  └────┘ └───────┘  │
                    └────────────────────┘
                              │
                    [Serviço Externo]
```

**1.2 Convenções globais**

| Aspecto | Convenção |
|---------|-----------|
| Linguagem / versão | |
| Estilo de código | ex: PEP 8, Google Style, Standard JS |
| Formatter | ex: black, prettier |
| Linter | ex: ruff, eslint |
| Docstrings/comentários | ex: Google style, JSDoc |
| Nomes de arquivo | ex: snake_case, kebab-case |
| Ordem de imports | ex: stdlib → third-party → local |
| IDs | ex: UUID v4 como string |
| Timestamps | ex: ISO 8601 UTC |
| Exceções | ex: hierarquia customizada herdando de BaseError |

---

### 2. Estrutura do Projeto

Árvore de diretórios com comentário inline por arquivo/pasta explicando responsabilidade:

```
projeto/
├── pyproject.toml / package.json    # build system e dependências
├── README.md
├── src/
│   ├── core/
│   │   ├── service_a.py             # [responsabilidade]
│   │   └── service_b.py             # [responsabilidade]
│   ├── storage/
│   │   ├── database.py              # [responsabilidade]
│   │   └── repository.py            # [responsabilidade]
│   └── ui/
│       └── app.py                   # [responsabilidade]
└── tests/
    ├── conftest.py                  # fixtures compartilhadas
    └── test_service_a.py
```

> Documentar a responsabilidade de cada arquivo elimina ambiguidade na hora de implementar.

---

### 3. Constantes Globais

Arquivo dedicado para todas as constantes e valores padrão:

```python
# constants.py (ou config.ts, etc.)

DEFAULT_HOST     = "..."      # descrição
DEFAULT_PORT     = 8000       # descrição
DEFAULT_TIMEOUT  = 3          # segundos — descrição
MAX_FILE_SIZE_MB = 50         # descrição
SUPPORTED_TYPES  = [...]      # descrição
```

**Variáveis de ambiente** (lidas em runtime, sobrescrevem constantes):

| Variável | Padrão | Descrição | Obrigatória |
|----------|--------|-----------|-------------|
| `APP_VAR_NAME` | `valor` | descrição | Não |

> Nunca hardcodar valores de configuração fora do arquivo de constantes.

---

### 4. Especificação por Módulo

Para cada módulo/serviço do projeto:

```
Módulo: [nome]
Arquivo: [caminho/arquivo]
Responsabilidade: [uma linha descrevendo o que este módulo faz e o que NÃO faz]

Classes / funções públicas:

  class NomeClasse:
      def método_público(self, param: Tipo) -> RetornoTipo:
          """Docstring: o que faz, args, returns, raises."""

  Dataclasses / tipos:
      @dataclass
      class NomeTipo:
          campo: Tipo    # descrição

Lógica crítica de implementação:
  [pseudocódigo ou código real para algoritmos não-óbvios]

Notas de framework:
  [comportamentos específicos do framework que impactam a implementação]
  [diferenças importantes vs outros frameworks conhecidos]
```

> Especificar cada módulo com assinaturas tipadas elimina ambiguidade e acelera implementação.

---

### 5. Estado de Sessão

Para frameworks com estado de sessão (Streamlit, Chainlit, Redux, Zustand, etc.):

| Key / Variável | Tipo | Valor inicial | Descrição |
|----------------|------|---------------|-----------|
| `current_model` | string | primeiro disponível | Modelo ativo na sessão |
| `session_id` | string \| null | null | ID da sessão/conversa ativa |

> Documentar o estado de sessão explicitamente evita bugs de estado compartilhado entre usuários ou abas.

---

### 6. Modelagem de Dados Completa

**Schema SQL (ou equivalente):**

```sql
CREATE TABLE [entidade] (
    id   TEXT PRIMARY KEY,
    -- campos de negócio com CHECK constraints
    status TEXT NOT NULL CHECK(status IN ('ativo', 'inativo')),
    tipo   TEXT NOT NULL CHECK(tipo IN ('a', 'b', 'c')),
    -- timestamps
    created_at TEXT NOT NULL,   -- ISO 8601 UTC
    updated_at TEXT NOT NULL    -- ISO 8601 UTC
);

CREATE INDEX IF NOT EXISTS idx_[entidade]_[campo] ON [entidade]([campo] DESC);
```

**Relacionamentos:** cardinalidade + FK + comportamento ON DELETE.

**Enums:** listar todos os valores possíveis de campos de status/tipo.

**Soft delete:** se aplicável, estratégia e campos usados.

**Bancos secundários** (vetorial, cache, etc.):

```
Collection: [nome]
Campos: id, document (text), embedding (vetor), metadata ({campo: tipo})
ID pattern: [padrão de geração de IDs]
```

**Estratégia de migração:**
- MVP: `CREATE TABLE IF NOT EXISTS` (idempotente)
- Evolução: implementar tabela `schema_version` + migrações sequenciais numeradas quando houver ALTER TABLE

---

### 7. Máquinas de Estado e Invariantes

*(Incluir quando o produto tiver entidades com ciclo de vida complexo — ex: pedidos, assinaturas, documentos, tarefas. Omitir para entidades simples sem transições de estado.)*

Para cada entidade com estado relevante:

**7.1 Máquina de Estado**

```
Estados possíveis: [estado_a] | [estado_b] | [estado_c] | [estado_d]

Transições:
  [estado_a] ──condição──→ [estado_b]
      efeito colateral: [o que acontece ao transitar]

  [estado_b] ──condição──→ [estado_c]
      efeito colateral: [o que acontece ao transitar]

  [estado_b] ──condição──→ [estado_a]   ← reversível se aplicável
      efeito colateral: [o que acontece ao transitar]

  [estado_c] ──condição──→ [estado_d]   ← estado terminal
      efeito colateral: [o que acontece ao transitar]

Estados terminais: [estado_d] — não permite transição de saída
```

> Documentar estados terminais explicitamente evita bugs onde o sistema tenta transitar uma entidade já encerrada.

**7.2 Invariantes do Domínio**

Invariantes são verdades absolutas do domínio — nunca podem ser violadas, independentemente de como o sistema chegou àquele estado. Numerar para referência cruzada com testes e regras de negócio.

```
INV-01: [descrição da invariante — verdade que sempre deve ser verdadeira]
  Exemplo: "Um pedido no estado 'pago' nunca pode ter valor total = 0"
  Verificar em: [onde no código esta invariante deve ser checada]

INV-02: [descrição]
  Exemplo: "Um usuário com plano 'free' nunca pode ter mais de X projetos ativos"
  Verificar em: [onde no código]

INV-03: [descrição]
  Exemplo: "Uma assinatura cancelada não pode ser reativada sem novo pagamento"
  Verificar em: [onde no código]
```

> Invariantes respondem perguntas como "o que acontece com os registros quando um usuário faz downgrade?" — a resposta deve estar aqui, não ser inferida pelo agente ou desenvolvedor.

---

### 8. Contratos de API

**8.1 Convenções gerais**
- Base URL, versionamento, autenticação
- Paginação padrão
- Formato de erros

**8.2 Endpoints internos** (se o produto expõe API):

```
[MÉTODO] /v1/[recurso]
Descrição: [o que faz]
Auth: [requerida / pública / admin]
Request:
  body: { "campo": tipo — obrigatório/opcional }
Response 200:
  { "campo": tipo }
Errors:
  400 — [condição]
  401 — [condição]
  404 — [condição]
  422 — [condição]
  429 — rate limit
```

**8.3 APIs externas consumidas** — contratos detalhados com request/response/campos de métricas:

Para cada endpoint externo relevante:

```
[MÉTODO] [URL completa]
Propósito: [o que faz no contexto do produto]
Request: { ... }
Response: { ... }
Campos críticos:
  campo_x — unidade — descrição
  campo_y — unidade — descrição
Tratamento de erro: [comportamento em falha / timeout]
```

---

### 9. Autenticação e Autorização

*(omitir se produto single-user local)*

- Fluxo de autenticação passo a passo
- Estrutura do token (claims JWT ou equivalente)
- Expiração e refresh
- Matriz de permissões por role × recurso × ação
- Middleware de autorização

---

### 10. Lógica de Negócio — Implementação

Para cada regra crítica do PRD:

```
Regra: [RN-ID]
Trigger: [o que dispara]
Validações:
  - [condição] → [erro ou ação]
Processo:
  1. [passo]
  2. [passo]
Side effects:
  - [evento, notificação, log, atualização de estado]
Rollback: [se transacional, como reverter em caso de falha]
```

---

### 11. Integrações — Implementação Técnica

Para cada integração:

- SDK/biblioteca utilizada e versão
- Credenciais: referência a env vars (nunca valores reais)
- Fluxo de chamada com retry, timeout e backoff
- Tratamento de erros e fallback
- Webhooks (se aplicável): validação de assinatura, idempotência, processamento

---

### 12. Processamento Assíncrono

*(omitir se não aplicável)*

| Job/Worker | Trigger | Frequência | Retry | Timeout |
|------------|---------|------------|-------|---------|

---

### 13. Gestão de Erros

**Hierarquia de exceções:**

```python
class AppBaseError(Exception):
    """Exceção base."""

class ServiceUnavailableError(AppBaseError):
    """Serviço externo inacessível."""

class ValidationError(AppBaseError):
    """Dado inválido."""

class NotFoundError(AppBaseError):
    """Recurso não encontrado."""
```

**Tratamento por exceção:**

| Exceção | Ação na UI / resposta | Log level |
|---------|----------------------|-----------|
| ServiceUnavailableError | Tela de erro com retry | error |
| ValidationError | Mensagem inline no campo | warn |
| NotFoundError | 404 / empty state | info |

---

### 14. Segurança

- Sanitização de inputs (camadas e abordagem)
- Proteção contra injeção (SQL, XSS, CSRF — conforme stack)
- Rate limiting por endpoint ou operação
- Secrets management (env vars, vault, never in code)
- Auditoria e logging de ações sensíveis
- CORS: origens permitidas
- Headers de segurança obrigatórios
- Comunicação externa: listar todos os domínios que o app contata

---

### 15. Observabilidade

**Logging**
- Formato (JSON estruturado recomendado)
- Níveis e quando usar cada um
- Campos obrigatórios por log: timestamp, level, module, message + campos de contexto

**Métricas**
- Métricas de infra: latência, throughput, uso de memória/CPU
- Métricas de negócio: conversões, uso de features, erros de negócio

**Alertas**

| Condição | Threshold | Ação |
|----------|-----------|------|

---

### 16. Estratégia de Testes

**16.1 Categorias e ferramentas**

| Categoria | Escopo | Ferramenta | Requer infra externa? |
|-----------|--------|------------|----------------------|
| Unit | Módulos isolados | pytest / jest | Não (mock) |
| Integration | Pipeline completo | pytest / jest | Sim |
| Smoke | App inicia sem erros | subprocess / playwright | Sim |

**16.2 Fixtures e mocks compartilhados** (`conftest.py` ou equivalente):

```python
# Padrão de mock para dependências externas
@fixture
def mock_servico_externo():
    # retorna mock com comportamento esperado
```

**16.3 Testes críticos por módulo:**

| Teste | Módulo | O que verifica |
|-------|--------|----------------|
| test_[nome] | [módulo] | [comportamento verificado] |

**16.4 Cobertura mínima por módulo:**

| Módulo | Alvo |
|--------|------|
| core/ | 80% |
| storage/ | 80% |
| ui/ | 30% |
| **Global** | **60%** |

---

### 17. Deploy e Infraestrutura

**17.1 Ambientes**

| Ambiente | Finalidade | Branch | Auto-deploy |
|----------|------------|--------|-------------|
| dev | Desenvolvimento | feature/* | Sim |
| staging | Homologação | main | Sim |
| prod | Produção | tags/v* | Manual |

**17.2 CI/CD Pipeline**
- Etapas: lint → test → build → push → deploy
- Ferramentas
- Rollback strategy

**17.3 Infra como código** *(omitir se não aplicável)*
- Ferramenta (Terraform, Pulumi, CDK)
- Recursos provisionados

---

### 18. Plano de Rollout

- Feature flags: quais features ficam atrás de flag no lançamento
- Estratégia de rollout gradual (se aplicável)
- Critérios para rollback

---

### 19. Diagramas de Sequência

Para cada fluxo crítico do PRD, diagrama ASCII com atores, setas e ordem temporal:

```
AtorA        Módulo1      Módulo2      ServiçoExt    DB
  │            │             │             │          │
  │─ ação ───→│             │             │          │
  │            │─ chama ───→│             │          │
  │            │             │─ request ──→│          │
  │            │             │←── resp ───│          │
  │            │             │─ persiste ──────────→│
  │            │←── retorno ─│             │          │
  │←── resp ──│             │             │          │
```

Diagramas obrigatórios:
- SEQ-01: Fluxo principal de uso end-to-end
- SEQ-02: Fluxo de processamento de dado externo (upload, webhook, etc.) — se aplicável
- SEQ-03: Fluxo de autenticação — se aplicável

> Diagramas de sequência são a ponte entre PRD e código — mostram a ordem exata de chamadas entre módulos.

---

### Checklist de qualidade — SPEC

- [ ] Diagrama de arquitetura ASCII cobre todos os componentes?
- [ ] Estrutura de diretórios com responsabilidade por arquivo?
- [ ] Constantes globais em arquivo dedicado com variáveis de ambiente?
- [ ] Cada módulo especificado com assinaturas tipadas e lógica crítica?
- [ ] Estado de sessão documentado (se aplicável ao framework)?
- [ ] Schema de dados completo com CHECK constraints e estratégia de migração?
- [ ] Máquinas de estado documentadas para entidades com ciclo de vida complexo?
- [ ] Invariantes do domínio numeradas (INV-XX) com local de verificação?
- [ ] Contratos de APIs externas com campos de request/response?
- [ ] Hierarquia de exceções definida com tratamento por tipo?
- [ ] Segurança coberta (sanitização, rate limit, secrets)?
- [ ] Fixtures e testes críticos por módulo definidos?
- [ ] Cobertura mínima por módulo estabelecida?
- [ ] Diagramas de sequência para fluxos críticos?
- [ ] Pipeline CI/CD e ambientes documentados?
