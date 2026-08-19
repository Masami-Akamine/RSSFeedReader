# RSS Feed Reader Constitution

## Core Principles

### 1. Clean Architecture
Implementamos padrões de Clean Architecture para separar responsabilidades em camadas bem definidas: apresentação (API), aplicação (casos de uso), domínio (lógica de negócio) e infraestrutura (dados e serviços externos). Cada camada é independente, testável e substituível. Nenhuma camada interna conhece detalhes das camadas externas.

### 2. Excelência em Testes
Testes automatizados são obrigatórios em toda mudança de código. Cada feature, correção e refatoração deve incluir testes unitários, de integração e de aceitação conforme aplicável. Testes devem ser determinísticos, rápidos e isolados. Falha nos testes bloqueia qualquer merge.

### 3. Segurança por Design
Validação de entrada é implementada em todas as camadas (API, aplicação, domínio). Seguimos OWASP Top 10. Dados sensíveis são criptografados em repouso e em trânsito. Autenticação e autorização são centralizadas. Nenhum código produção vai ao ar sem revisão de segurança.

### 4. Documentação Obrigatória
Todo código público (classes, métodos, APIs) é documentado com XML comments. Especificações técnicas complexas têm READMEs e diagramas. Decisões arquiteturais são registradas em ADRs (Architecture Decision Records). A documentação é viva e sincronizada com o código.

### 5. Manutenibilidade e Revisabilidade
Código deve ser legível na primeira leitura. Nomes de variáveis e funções são explícitos e em inglês. Máximo 150 linhas por método. Complexidade ciclomática não deve exceder 10. Todas as mudanças passam por code review com pelo menos 2 aprovações. Feedback é construtivo e focado em melhorar a qualidade.

## Stack Tecnológico

- **Linguagem**: C# 12+
- **Framework**: ASP.NET Core 8
- **Frontend**: Blazor WebAssembly
- **Banco de Dados**: SQL Server (Extended-MVP em diante)
- **Testes**: xUnit, Moq, FluentAssertions
- **Qualidade**: SonarQube, StyleCop, FxCop
- **CI/CD**: GitHub Actions
- **Versionamento**: Semantic Versioning

## Fases de Desenvolvimento

### MVP (Proof-of-Concept)

**Escopo Técnico**
- Armazenamento: In-memory (List<T>)
- Feed Fetching: Não necessário
- Persistência: Nenhuma (dados perdidos ao fechar)
- Autenticação: Não requerida
- Banco de Dados: Não necessário

**Padrões MVP**
- Cobertura de testes: 70% (suficiente para POC)
- Validação de entrada: Assumir entrada válida (URL válida fornecida)
- Error handling: Mínimo (sem operações de rede)
- Performance: Operações locais apenas
- Segurança: Básica (HTTPS local não crítico)

**Code Review MVP**
- ✅ 1 aprovação necessária (workflow mais ágil)
- ✅ Build bem-sucedido
- ✅ Testes passando (70%+ cobertura)
- ✅ Código legível e documentado

### Extended-MVP (Feed Fetching & Display)

**Escopo Técnico**
- Armazenamento: SQL Server
- Feed Fetching: Suportado (manual)
- Persistência: Subscriptions e items
- Autenticação: Preparado (não obrigatório)
- Background Jobs: Não necessário (refresh manual apenas)

**Padrões Extended-MVP**
- Cobertura de testes: 80%+
- Validação de entrada: 100% dos endpoints
- Error handling: Detalhado (feed fetch failures)
- Performance: < 500ms para operações comuns
- Segurança: SQL Injection prevention, dados sensíveis não em logs

**Code Review Extended-MVP**
- ✅ 2 aprovações necessárias
- ✅ Build e testes 100%
- ✅ Cobertura não declinando
- ✅ SonarQube sem avisos críticos
- ✅ Documentação completa

### Production (Full-Featured)

**Escopo Técnico**
- Armazenamento: SQL Server com redundância
- Feed Fetching: Manual + automático (background jobs)
- Persistência: Completa (subscriptions, items, read/unread)
- Autenticação: Obrigatória (OAuth, JWT)
- Performance: < 200ms P95 percentile

**Padrões Production**
- Cobertura de testes: 85%+
- Validação: OWASP Top 10 compliance
- Error handling: Fallback strategies
- Performance: Caching, paginação, índices
- Segurança: Encryption at rest, TLS 1.2+, secrets management

**Code Review Production**
- ✅ 2+ aprovações + security review
- ✅ 100% testes passando
- ✅ Sem warnings do compilador
- ✅ Conformidade com Constitution completa

## Padrões de Código

### Source Control
- Commits pequenos e focados
- Uma feature por branch
- Pull Request obrigatório
- Conventional Commits recomendados

### Estrutura de Projeto


### RSS Domain Principles
- Feeds RSS e Atom devem ser suportados (Extended-MVP+)
- Falhas de sincronização não devem interromper o processamento de outros feeds
- Artigos duplicados devem ser identificados e ignorados
- O sistema deve preservar histórico de leitura (Extended-MVP+)
- O sistema deve armazenar metadados originais do feed

### Conventions
- **Naming**: PascalCase para classes/métodos, camelCase para variáveis locais
- **Async**: Toda I/O deve ser assíncrona. Métodos async devem terminar em `Async`
- **Exceptions**: Use tipos específicos de exceção. Nunca capture genérico `Exception`
- **Logging**: Use ILogger. Log em níveis apropriados (Error, Warning, Information)
- **Validação**: Use FluentValidation ou data annotations
- **Null Safety**: Use nullable reference types. Nunca confie em valores nulos
- **Blazor Components**: Componentes reutilizáveis, props documentadas, sem lógica complexa

### Cobertura de Testes

**MVP**
- Cobertura Global: 70%
- Domain: 75%+
- Application: 70%+

**Extended-MVP**
- Cobertura Global: 80%
- Domain: 85%+
- Application: 85%+
- Infrastructure: 75%+
- API: 75%+

**Production**
- Cobertura Global: 85%+
- Domain: 90%+
- Application: 85%+
- Infrastructure: 80%+
- API: 80%+

## Requisitos de Qualidade

### Code Review

**MVP**
- ✅ 1 aprovação mínima
- ✅ Sem comentários críticos pendentes
- ✅ Testes passando (70%+)
- ✅ Build bem-sucedido

**Extended-MVP+**
- ✅ 2 aprovações de reviewers diferentes
- ✅ Sem comentários pendentes
- ✅ Testes passando 100%
- ✅ Cobertura não declinando
- ✅ Sem avisos de SonarQube críticos/altos
- ✅ Documentação completa para APIs públicas

### Antes do Merge

**MVP**
- ✅ Build local bem-sucedido
- ✅ Testes passando (70%+)

**Extended-MVP+**
- ✅ Build local bem-sucedido
- ✅ Todos os testes passando
- ✅ Cobertura de testes atende ao mínimo
- ✅ Code style analysis sem erros
- ✅ Sem warnings do compilador
- ✅ Mensagem de commit clara e descritiva

### Segurança

**MVP**
- Nenhum requisito crítico (POC local)

**Extended-MVP+**
- ✅ Validação de entrada em 100% dos endpoints
- ✅ Sem hardcoding de secrets
- ✅ SQL injection prevenido com ORM/parametrização
- ✅ Dados sensíveis não em logs
- ✅ CORS configurado adequadamente

**Production**
- ✅ Autenticação/autorização implementadas
- ✅ Encryption at rest (dados sensíveis)
- ✅ TLS 1.2+ para trânsito
- ✅ Secrets management com Azure Key Vault
- ✅ Rate limiting em APIs públicas
- ✅ Audit logging para operações críticas

### Performance

**MVP**
- Não aplicável (operações locais)

**Extended-MVP**
- Tempo de resposta < 500ms para operações comuns
- Consultas devem ser paginadas
- Operações de sincronização em background (se implementado)

**Production**
- P95 latency < 200ms
- P99 latency < 500ms
- Suporte a 100+ feeds por usuário
- Caching de feeds (não atualizar > 1x por hora)

## Architecture Decision Records (ADRs)

Todas as decisões arquiteturais importantes devem ser documentadas em ADRs localizadas em `docs/adr/`.

**ADRs Críticas para Projeto**
- ADR-001: Por que in-memory para MVP?
- ADR-002: Quando e como migrar para SQL Server?
- ADR-003: Blazor WebAssembly vs Server?
- ADR-004: Feed fetching strategy (sync vs async)?
- ADR-005: Deduplicação de artigos - regras?

Formato: [MADR 3.0](https://adr.github.io/madr/)

## Governance

### Roles
- **Product Owner**: Define escopo e prioridades das fases
- **Arquiteto**: Revisa decisões arquiteturais e ADRs
- **Tech Lead**: Garante conformidade com Constitution (ajustada por fase)
- **Reviewers**: Todos os desenvolvedores (code review obrigatório)
- **QA**: Valida requisitos funcionais e segurança

### Decisões

**MVP**
- Mudanças de escopo: Discussão rápida em equipe
- Novas dependências: Tech Lead aprova

**Extended-MVP+**
- Mudanças arquiteturais: Discussão em equipe + ADR
- Novas dependências: Aprovação do Tech Lead + ADR
- Changes de segurança: Análise obrigatória
- Refatorações maiores: Planning + Revisão arquitetural

### Violações

**MVP**
- Merge sem testes (70%): Bloqueado
- Merge sem review: Bloqueado

**Extended-MVP+**
- Merge sem testes (80%+): Bloqueado automaticamente
- Merge com cobertura declinada: Bloqueado
- Merge sem 2 aprovações: Bloqueado
- Merge com código inseguro: Revertido + investigação

## Product Vision

RSS Feed Reader é uma aplicação web desenvolvida em ASP.NET Core que permite aos usuários gerenciar múltiplos feeds RSS e Atom, sincronizar conteúdo periodicamente, pesquisar artigos, organizar favoritos e acompanhar histórico de leitura.

**MVP**: Foco em gerenciamento de subscriptions (simples, em memória)

**Extended-MVP**: Adicionar feed fetching e exibição de artigos (persistência básica)

**Production**: Sistema completo com background polling, autenticação, análise e recomendações

O sistema prioriza **simplicidade → funcionalidade → performance**, seguindo princípios de Clean Architecture e desenvolvimento orientado por testes, com adaptação de rigor conforme a fase.

---

**Versão**: 1.1 | **Ratificada**: 2026-08-19 | **Próxima Revisão**: 2026-11-19

**Mudanças v1.0 → v1.1**: Estruturado por fases (MVP/Extended-MVP/Production), clarificadas métricas de cobertura e segurança por fase, removido duplicate título, adicionadas orientações ADR