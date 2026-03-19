# MVP Scope

> Primeiro documento do fluxo. Define visão geral técnica e estratégica do produto.

---

## Estrutura do MVP Scope

---

### 1. Visão Geral do Produto

- Nome, tagline e descrição em 2–3 frases
- Problema que resolve
- Proposta de valor central
- Diferenciais vs. concorrentes

---

### 2. Pesquisa de Mercado e Público-Alvo

**2.1 Tamanho de mercado**
- TAM / SAM / SOM quando possível
- Fontes com ano e região; se ausentes, indicar **estimativa**

**2.2 Tendências relevantes**
- Máx. 5 tendências com impacto direto no produto

**2.3 Mapa competitivo**

| Concorrente | Pontos fortes | Lacunas | Posicionamento |
|-------------|---------------|---------|----------------|

**2.4 Personas**

*Persona primária*: nome fictício, cargo, contexto, dores, jobs-to-be-done

*Persona secundária*: idem

**2.5 Oportunidade de posicionamento**
- Gap identificado e como o produto se posiciona para capturá-lo

---

### 3. Stack Tecnológica Recomendada

Para cada decisão: escolha recomendada + alternativa viável + trade-offs.

| Camada | Escolha | Alternativa | Trade-off |
|--------|---------|-------------|-----------|
| Frontend | | | |
| Backend | | | |
| Banco de dados | | | |
| Auth | | | |
| Infra/Deploy | | | |
| IA/LLM (se aplicável) | | | |

Justificativa curta obrigatória para cada escolha.

---

### 4. APIs e Integrações Externas

| Serviço | Finalidade | Justificativa |
|---------|------------|---------------|

Cobrir: produto, pagamentos, email, storage, analytics, observabilidade.

---

### 5. Arquitetura do Sistema

- Diagrama ASCII da arquitetura (camadas, serviços, fluxo de dados)
- Padrões adotados e justificativa
- Gargalos e riscos técnicos para o MVP

```
[componente] ←protocolo→ [componente]
                │
         ┌──────┴──────┐
         │   camada    │
         └─────────────┘
```

---

### 6. Modelagem de Dados (Prévia)

Para cada entidade principal:

| Campo | Tipo | Descrição |
|-------|------|-----------|

Incluir: relacionamentos, estratégia de acesso, segurança (RLS se aplicável).

---

### 7. Regras de Negócio Centrais

- Regras críticas de funcionamento
- Fluxo de onboarding
- Fluxo principal de uso
- Fluxo de cancelamento/churn
- Edge cases que o MVP já deve tratar

---

### 8. Monetização e Modelo de Negócio

- Modelo recomendado (freemium, SaaS, pay-per-use, licença, etc.)
- Tiers sugeridos para o MVP com preços (benchmark ou **estimativa**)
- Estratégia de conversão free → pago

---

### 9. Roadmap de Features

| Feature | Prioridade | Complexidade | Dependências |
|---------|------------|--------------|--------------|
| | Essencial MVP / Pós-MVP v1 / Futuro | Baixa / Média / Alta | |

---

### 10. Fora do Escopo do MVP

| Item | Motivo da exclusão |
|------|--------------------|

---

### 11. Evolução da Arquitetura

Cenários: 10k usuários → 1M usuários → multi-região.
Indicar quais partes evoluem e como.

---

### 12. Riscos e Pontos de Atenção

| Risco | Categoria | Impacto | Mitigação |
|-------|-----------|---------|-----------|
| | Técnico / Mercado / Regulatório / Dependência | Alto / Médio / Baixo | |

---

### Resumo Executivo

Até 5 parágrafos cobrindo: visão do produto, oportunidade, stack, arquitetura, próximo passo.

---

### Checklist de qualidade — MVP Scope

- [ ] Todas as premissas estão marcadas explicitamente?
- [ ] Todos os dados têm fonte ou estão marcados como estimativa?
- [ ] Cada decisão tecnológica tem justificativa e alternativa?
- [ ] O roadmap está sequenciado com dependências?
- [ ] O "fora do escopo" está explícito com motivo?
- [ ] O resumo executivo é coeso e direto?
- [ ] Sugestões proativas de pontos omitidos foram sinalizadas?

---

**Ao finalizar:** perguntar — *"Deseja prosseguir para o PRD?"*
