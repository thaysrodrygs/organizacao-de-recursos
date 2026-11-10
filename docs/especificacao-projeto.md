# Especificação do Projeto — Organização de Recursos

## 1. Objetivo

Desenvolver uma aplicação confiável para alocação de recursos, como salas, professores e materiais, sem conflitos de horários.

## 2. Perfis de usuário

### Solicitante (Professor ou Coordenador)
- Consultar disponibilidade.
- Criar, alterar e cancelar suas próprias reservas.

### Responsável
- Aprovar solicitações especiais.
- Validar alocação de professores.
- Acompanhar retirada e devolução de materiais e equipamentos.

### Administrador
- Gerenciar salas, professores, materiais e usuários.
- Gerenciar bloqueios e períodos de manutenção.

## 3. Funcionalidades obrigatórias

- Autenticação e autorização por perfil.
- CRUD e consulta de salas, professores e materiais.
- Busca por tipo, capacidade, localização, competência e disponibilidade.
- Criar, alterar e cancelar reservas.
- Detectar conflitos de horários, inclusive na agenda dos professores.
- Impedir reservas duplicadas simultâneas.
- Aprovação para recursos restritos.
- Bloqueios de manutenção.
- Controle de retirada e devolução de materiais e equipamentos.
- Histórico auditável de alterações.
- Notificação simulada ou integração com API externa.
- Relatórios de utilização por recurso, horas alocadas e conflitos evitados.
- Interface responsiva e mensagens de erro compreensíveis.
- Documentação da API e dos fluxos públicos.

## 4. Regras de negócio

1. O horário final deve ser posterior ao inicial.
2. O mesmo recurso não pode ter reservas com horários sobrepostos.
3. Um professor não pode ter reservas simultâneas.
4. Solicitações simultâneas para o mesmo recurso e horário devem resultar em apenas uma reserva aceita.
5. Recursos em manutenção não podem ser reservados.
6. Estados da reserva: SOLICITADA → APROVADA → EM_USO → CONCLUÍDA.
7. Também devem ser tratados os estados de alternativas rejeitadas, canceladas e não comparecimento.
8. Somente o Responsável aprova recursos restritos.
9. Reservas iniciadas não podem ser excluídas.
10. Toda mudança de estado deve ser registrada no histórico.

## 5. Requisitos de qualidade e testes

- JUnit 5: testes unitários, parametrizados, de integração, API black-box e E2E.
- Banco de dados em contêiner com Testcontainers.
- WireMock para integrações externas.
- Teste automatizado de concorrência.
- TDD e BDD para pelo menos uma funcionalidade.
- GitHub Actions para CI em Pull Requests.
- JaCoCo e SonarCloud.
- JMeter para testes de carga.
- Testes de autorização, validação de entradas e erros seguros.
- Mocks somente para testes unitários isolados. Persistência e integrações críticas devem usar ambiente real de teste, Testcontainers e WireMock.

## 6. Metas de qualidade

- 80% de cobertura de linhas.
- 70% de cobertura de branches.
- 100% dos requisitos críticos cobertos na RTM.
- Zero bugs ou vulnerabilidades críticas.

## 7. Entregáveis

- Repositório com histórico de alterações e revisão por pares.
- Aplicação executável e instruções de instalação.
- README e documentação dos fluxos e da API.
- RTM.md relacionando requisitos, riscos, testes e evidências.
- Diagramas de contexto, componentes e sequências críticas.
- Plano de testes, suíte automatizada e relatório.
- Pipeline de CI.
- Relatórios JaCoCo e SonarCloud.
- Plano e resultados dos testes de carga.
- Registro de defeitos.
- Relatório de qualidade.
- Apresentação e demonstração da aplicação.

## 8. Critérios impeditivos

- A aplicação não executar.
- Pipeline ausente.
- Falha de autorização.
- Possibilidade de reserva duplicada.

## 9. Etapas previstas

1. Diagnóstico.
2. Requisitos, riscos e RTM.
3. Testes e integrações.
4. CI e SonarCloud.
5. TDD, BDD, WireMock e testes de carga.
6. Entrega, feedback e retrospectiva.