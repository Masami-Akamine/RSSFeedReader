Tarefas: Leitor de Feed RSS – Gerenciamento de Assinaturas
Entrada: Documentos de design em /specs/003-rss-subscriptions/

Pré-requisitos: plan.md, spec.md

Testes: Testes de integração estão incluídos onde as histórias de usuário os exigem explicitamente.

Organização: As tarefas são agrupadas por história de usuário para permitir implementação e validação independentes.

Formato: [ID] [P?] [História] Descrição
[P]: pode ser executado em paralelo
[História]: US1, US2, etc.
Incluir caminhos de arquivo na descrição de cada tarefa
Fase 1: Configuração Inicial (Infraestrutura Compartilhada)
Objetivo: Inicializar a solução e a estrutura compartilhada do projeto. 

T001 Criar a estrutura inicial da solução para backend, frontend e testes
T002 Inicializar o projeto ASP.NET Core Web API em backend/src/RssFeedReader.Api
T003 Inicializar o projeto frontend Blazor WebAssembly em frontend
T004 Configurar a solução .NET e as referências de projetos compartilhados
T005 [P] Adicionar projetos de teste e dependências para xUnit, FluentAssertions e Moq
T006 [P] Configurar formatação de código, analisadores e convenções básicas do projeto
Fase 2: Fundamentos (Pré-requisitos Bloqueantes)
Objetivo: Estabelecer os modelos compartilhados, a validação e a integração de serviços necessários antes do desenvolvimento das funcionalidades. T007 Definir modelos de domínio compartilhados em `backend/src/RssFeedReader.Application/Models`
T008 Definir DTOs para entrada de assinatura e resultados de feed em `backend/src/RssFeedReader.Application/Contracts`
T009 Implementar lógica de validação de URL em `backend/src/RssFeedReader.Application/Services/UrlValidationService.cs`
T010 Implementar lógica de detecção de duplicatas para assinaturas
T011 Configurar injeção de dependência e registro de serviços em `backend/src/RssFeedReader.Api/Program.cs`
T012 Criar o repositório/serviço de assinaturas em memória em `backend/src/RssFeedReader.Application/Services`
T013 Adicionar tratamento centralizado de erros e respostas de validação amigáveis ​​ao usuário em `backend/src/RssFeedReader.Api`
T014 Criar a estrutura inicial (skeleton) do controlador da API para assinaturas em `backend/src/RssFeedReader.Api/Controllers/SubscriptionsController.cs`
T015 Configurar a camada de cliente da API no frontend em `frontend/Services/SubscriptionApiClient.cs`
Ponto de verificação: Base concluída; a História de Usuário 1 pode começar.
Fase 3: História de Usuário 1 - Adicionar assinatura RSS ou Atom (Prioridade: P1) 🎯 MVP
Objetivo: Usuários podem adicionar uma URL de feed RSS ou Atom válida e vê-la aparecer na lista de assinaturas. Teste Independente:

Adicionar URL válida → armazenada e exibida
Adicionar URL em branco, malformada, duplicada ou muito longa → erro de validação exibido
Lista existente permanece inalterada quando a validação falha
Testes para a História de Usuário 1
T016 [P] [US1] Adicionar teste de integração para criação bem-sucedida de assinatura em backend/tests/RssFeedReader.Api.Tests
T017 [P] [US1] Adicionar teste de integração para rejeição de URL inválida em backend/tests/RssFeedReader.Api.Tests
T018 [P] [US1] Adicionar teste de integração para prevenção de URL duplicada em backend/tests/RssFeedReader.Api.Tests
Implementação para a História de Usuário 1
T019 [P] [US1] Criar modelo Subscription em backend/src/RssFeedReader.Application/Models/Subscription.cs
T020 [P] [US1] Criar modelo FeedItem em backend/src/RssFeedReader.Application/Models/FeedItem.cs
T021 [US1] Implementar caso de uso de criação de assinatura em backend/src/RssFeedReader.Application/Services/SubscriptionService.cs
T022 [US1] Implementar endpoints de criação e listagem em backend/src/RssFeedReader.Api/Controllers/SubscriptionsController.cs
T023 [US1] Implementar formulário de frontend para inserção de URLs de feed em frontend/Pages/Subscriptions.razor
T024 [US1] Implementar a renderização da lista de assinaturas em frontend/Pages/Subscriptions.razor
T025 [US1] Adicionar validação de UI e mensagens de erro amigáveis ​​em frontend/Services/SubscriptionApiClient.cs
T026 [US1] Garantir que valores vazios, malformados, duplicados e muito longos sejam rejeitados antes da persistência
Ponto de verificação: A História de Usuário 1 está funcional e pode ser testada de forma independente.
Fase 4: História de Usuário 2 - Atualizar feeds manualmente e exibir itens (Prioridade: P2)
Objetivo: Usuários podem atualizar uma assinatura, processar os itens mais recentes do feed e manter o conteúdo anterior caso a atualização falhe. Teste Independente:
A atualização válida do feed é bem-sucedida e atualiza a lista de itens
A falha na atualização com um feed inválido ou inacessível não apaga os itens exibidos anteriormente
Formato de feed não suportado exibe um erro explícito
Testes para a User Story 2
T027 [P] [US2] Adicionar teste de integração para atualização bem-sucedida do feed em backend/tests/RssFeedReader.Api.Tests
T028 [P] [US2] Adicionar teste de integração para falha na atualização preservando itens anteriores em backend/tests/RssFeedReader.Api.Tests
T029 [P] [US2] Adicionar teste de integração para tratamento de XML não suportado ou formato de feed inválido em backend/tests/RssFeedReader.Api.Tests
Implementação para a User Story 2
T030 [P] [US2] Implementar lógica de parsing de RSS e Atom em backend/src/RssFeedReader.Application/Services/FeedParserService.cs
T031 [US2] Construir a orquestração de atualização do feed em backend/src/RssFeedReader.Application/Services/FeedSyncService.cs
T032 [US2] Adicionar endpoint de atualização em backend/src/RssFeedReader.Api/Controllers/SubscriptionsController.cs
T033 [US2] Exibir metadados do item do feed em frontend/Pages/Subscriptions.razor, incluindo título, data, resumo e link
T034 [US2] Adicionar ação de atualização a cada entrada de assinatura na lista do frontend
T035 [US2] Tratar falhas de acesso ao feed e manter os itens exibidos anteriormente sem limpá-los
T036 [US2] Adicionar mensagens amigáveis ​​para feeds indisponíveis, malformados ou vazios
Ponto de verificação: As Histórias de Usuário 1 e 2 estão funcionando de forma independente e podem ser validadas de ponta a ponta.

Fase 5: Refinamento e Aspectos Transversais
Objetivo: Melhorar a qualidade, a resiliência e a UX após o funcionamento das histórias principais.

T037 [P] Revisar e melhorar a consistência de nomenclatura entre backend e frontend
T038 [P] Eliminar dívida técnica e refatorar código compartilhado de validação/serviço
T039 [P] Adicionar testes unitários finais para casos de borda relacionados ao processamento (parsing) de feeds e validação de URL
T040 Validar a UX para estados de vazio, carregamento e erro no frontend
T041 Revisar a consistência do contrato da API e garantir que os payloads de resposta sejam previsíveis
T042 Executar validação completa de regressão nos cenários de aceitação
T043 Atualizar a documentação em docs/ e as notas do projeto, se necessário
T044 Verificação final de qualidade de código, cobertura de testes e prontidão para implantação
Dependências e Ordem de Execução
Dependências de Fase
Configuração (Setup): sem dependências
Fundação: depende da Configuração
História de Usuário 1: depende da Fundação
História de Usuário 2: depende da Fundação
Refinamento: depende das Histórias de Usuário 1 e 2
Oportunidades de Paralelismo
T005, T006 e outras tarefas de configuração podem ser executadas em paralelo
A História de Usuário 1 e a História de Usuário 2 podem ser desenvolvidas em paralelo se a equipe tiver capacidade suficiente
A maioria das tarefas de teste marcadas com [P] pode ser executada simultaneamente após a conclusão da implementação da qual dependem