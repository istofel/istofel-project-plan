# CLAUDE.md — Referência de Geração

> Este arquivo define a estrutura do CLAUDE.md personalizado gerado ao final do processo.
> O conteúdo é extraído automaticamente dos documentos MVP Scope, PRD e SPEC já gerados.
> Não fazer perguntas ao usuário — preencher com dados reais do projeto.

---

## Regra de Geração

Incluir apenas informações **diretamente acionáveis pelo agente** em cada sessão de trabalho.
Não duplicar conteúdo completo dos documentos de origem — referenciar quando necessário.
Omitir seções que não se aplicam ao projeto (ex: seção de auth em produto single-user).

---

## Estrutura do CLAUDE.md Gerado

---

### 1. Visão Geral do Projeto

Extrair do MVP Scope:
- Nome do produto
- Descrição em uma frase (o que faz e para quem)
- Stack principal (linguagem + framework + banco)
- Referências aos três documentos de origem com caminhos relativos

---

### 2. Padrões Obrigatórios de Código

Extrair da SPEC — seções de convenções globais e especificação por módulo:

- Linguagem e versão mínima
- Formatter e linter com configuração
- Padrão de docstrings
- Convenção de IDs e timestamps
- Hierarquia de exceções (classe base)
- Ordem de imports
- Padrões específicos de banco de dados (como acessar, nunca fazer raw queries, etc.)
- Padrões específicos de autenticação (se aplicável)
- Padrões específicos de API/endpoints (se aplicável)

Formatar como lista de regras diretas, não como prosa.

---

### 3. O Que NUNCA Fazer

Extrair de:
- SPEC — ADRs (consequências: o que não fazer)
- PRD — regras de Autorização
- SPEC — notas de framework

Listar como proibições explícitas com prefixo `NUNCA`.
Incluir apenas proibições que um agente poderia razoavelmente tentar fazer.
Máximo 10 itens — priorizar as mais críticas.

---

### 4. Stack e Versões Fixadas

Extrair da SPEC — seção de ADRs e constantes globais:

Tabela com: Camada | Tecnologia | Versão mínima | ADR de referência

Incluir nota: "Estas decisões estão fechadas via ADR. Não propor alternativas."

---

### 5. Estrutura de Diretórios

Extrair da SPEC — seção de estrutura do projeto.

Reproduzir a árvore de diretórios com comentários de responsabilidade por pasta/arquivo.
Incluir apenas o nível necessário para o agente saber onde criar arquivos.

---

### 6. Invariantes Críticas

Extrair da SPEC — seção de máquinas de estado e invariantes (INV-XX).

Incluir todas as invariantes numeradas com:
- Tipo (Invariante / Validação / Transição de Estado / Autorização)
- Descrição
- Onde verificar no código

Formato compacto — uma invariante por bloco.

---

### 7. Sequência de Build

Extrair da SPEC — seção de sequência de build.

Reproduzir os passos numerados com checkpoint de validação.
Adicionar campo "Passo atual:" para o desenvolvedor preencher manualmente conforme avança.

---

### 8. Variáveis de Ambiente

Extrair da SPEC — seção de constantes globais.

Tabela com: Variável | Descrição | Obrigatória | Padrão

Incluir apenas variáveis de ambiente (não constantes internas do código).

---

### 9. Casos Extremos Respondidos

Extrair do PRD — edge cases, fora de escopo, e regras de negócio com comportamento em falha.

Tabela com: Situação | Comportamento esperado

Incluir apenas casos que o agente poderia implementar erroneamente sem esta orientação.
Máximo 8 itens.

---

### 10. Checklist Pré-Entrega

Lista fixa aplicável a qualquer projeto, complementada com itens específicos extraídos da SPEC:

- [ ] Type hints em todas as funções públicas novas?
- [ ] Docstrings em todas as classes e funções públicas novas?
- [ ] Testes para lógica de negócio nova?
- [ ] Nenhuma invariante violada?
- [ ] Nenhuma variável de ambiente hardcodada?
- [ ] Nenhum erro silenciado (`except: pass`)?
- [ ] Arquivo criado na pasta correta conforme estrutura de diretórios?
- [ ] Linter passa com zero warnings?
- [ ] Checkpoint do passo atual da sequência de build confirmado?
- [Adicionar itens específicos extraídos da SPEC do projeto]

---

### 11. Glossário do Domínio

Extrair do PRD — seção de glossário.

Tabela com: Termo no domínio | Usar no código como | NÃO usar

Incluir apenas termos que poderiam ser implementados de formas inconsistentes.

---

## Checklist de qualidade — CLAUDE.md

- [ ] Todas as informações foram extraídas dos documentos gerados (sem invenção)?
- [ ] Nenhuma seção inaplicável ao projeto foi incluída?
- [ ] As invariantes estão numeradas e com local de verificação?
- [ ] Os padrões de código são específicos o suficiente para serem acionáveis?
- [ ] A lista "NUNCA fazer" cobre os riscos reais do projeto?
- [ ] A sequência de build reflete exatamente a da SPEC?
- [ ] O glossário usa os mesmos termos do PRD?
