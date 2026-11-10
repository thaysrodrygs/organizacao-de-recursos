# PRD — Organização de Recursos

## 1. Visão do produto

Desenvolver uma aplicação confiável para alocar recursos — como salas, professores e materiais — sem conflitos de horários. A aplicação deve oferecer consulta, reserva e gestão de recursos, respeitando os perfis de usuário e as regras de negócio definidas na [especificação do projeto](./especificacao-projeto.md).

## 2. Usuários

- **Solicitante (professor ou coordenador):** consulta disponibilidade e cria, altera ou cancela as próprias reservas.
- **Responsável:** aprova solicitações especiais, valida alocação de professores e acompanha retirada e devolução de materiais e equipamentos.
- **Administrador:** gerencia salas, professores, materiais, usuários, bloqueios e períodos de manutenção.

Detalhes das necessidades, permissões e pontos em aberto de cada perfil estão em [personas.md](./personas.md).

## 3. Escopo funcional

| ID | Requisito |
|---|---|
| RF-01 | Autenticar usuários e autorizar ações de acordo com o perfil. |
| RF-02 | Permitir CRUD e consulta de salas, professores e materiais. |
| RF-03 | Permitir busca por tipo, capacidade, localização, competência e disponibilidade. |
| RF-04 | Permitir criar, alterar e cancelar reservas. |
| RF-05 | Detectar conflitos de horário, inclusive na agenda dos professores. |
| RF-06 | Impedir reservas duplicadas simultâneas para o mesmo recurso e horário. |
| RF-07 | Submeter recursos restritos à aprovação do Responsável. |
| RF-08 | Gerenciar bloqueios de manutenção e impedir reservas de recursos em manutenção. |
| RF-09 | Controlar retirada e devolução de materiais e equipamentos. |
| RF-10 | Manter histórico auditável de alterações e registrar toda mudança de estado de reserva. |
| RF-11 | Oferecer notificações simuladas ou integração com API externa. |
| RF-12 | Disponibilizar relatórios de utilização por recurso, horas alocadas e conflitos evitados. |
| RF-13 | Oferecer interface responsiva e mensagens de erro compreensíveis. |
| RF-14 | Documentar a API e os fluxos públicos. |

O ciclo de estados previsto é **SOLICITADA → APROVADA → EM_USO → CONCLUÍDA**. Também devem ser tratados estados de alternativas rejeitadas, canceladas e não comparecimento. Os detalhes estão em [regras-negocio.md](./regras-negocio.md).

## 4. Requisitos de qualidade e validação

- Testes automatizados com JUnit 5: unitários, parametrizados, de integração, API black-box e E2E.
- Banco de dados em contêiner com Testcontainers; WireMock para integrações externas.
- Testes automatizados de concorrência.
- TDD e BDD aplicados a pelo menos uma funcionalidade.
- GitHub Actions para CI em Pull Requests.
- JaCoCo e SonarCloud; JMeter para testes de carga.
- Testes de autorização, validação de entradas e erros seguros.
- Mocks somente em testes unitários isolados; persistência e integrações críticas devem usar ambiente real de teste, Testcontainers e WireMock.
- Metas: 80% de cobertura de linhas, 70% de cobertura de branches, 100% dos requisitos críticos cobertos na RTM e zero bugs ou vulnerabilidades críticas.

## 5. Entregáveis do projeto

- Repositório com histórico de alterações e revisão por pares.
- Aplicação executável e instruções de instalação.
- README e documentação dos fluxos e da API.
- RTM relacionando requisitos, riscos, testes e evidências.
- Diagramas de contexto, componentes e sequências críticas.
- Plano de testes, suíte automatizada e relatório.
- Pipeline de CI, relatórios JaCoCo e SonarCloud.
- Plano e resultados de testes de carga, registro de defeitos e relatório de qualidade.
- Apresentação e demonstração da aplicação.

## 6. Critérios impeditivos

São impeditivos: a aplicação não executar, ausência de pipeline, falha de autorização ou possibilidade de reserva duplicada.

## 7. Pontos que precisam de esclarecimento

- “Solicitações especiais”, “recursos restritos” e “alternativas rejeitadas” não têm critérios, definições ou fluxo detalhado.
- Não estão definidos os campos e operações de CRUD, nem se “CRUD” se aplica também a usuários ou apenas às salas, professores e materiais.
- Não estão definidos a relação entre os recursos pesquisáveis e as categorias “competência” e “localização”, nem os filtros combináveis.
- Não estão detalhados os estados de cancelamento, rejeição e não comparecimento, nem as transições permitidas entre estados.
- Não estão definidos a política de notificações (simulação ou integração), eventos, destinatários ou conteúdo.
- Não há limiares de desempenho, volume ou duração para os testes de carga.
- “Fluxos públicos” não está definido, assim como o alcance da documentação da API.
- Não estão definidos escopo, formato e conteúdo dos relatórios.
- Não está especificado o mecanismo de geração e aprovação da RTM nem quais requisitos serão classificados como críticos.
