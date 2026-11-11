# Arquitetura

## Seção inicial — contexto e drivers arquiteturais

Esta seção consolida os fatos documentados, sem escolher tecnologias ou completar regras ainda não definidas. A priorização dos drivers é provisória e constitui uma leitura arquitetural: considera primeiro os critérios impeditivos explícitos, depois os requisitos que protegem a alocação e, em seguida, os demais requisitos funcionais e de qualidade. Essa ordem não é uma prioridade confirmada do produto nem altera a prioridade normativa dos requisitos.

### Contexto e objetivo

O produto é uma aplicação para consultar, alocar e gerir recursos — como salas, professores e materiais — sem conflitos de horários. Equipamentos são mencionados na especificação no contexto do controle de retirada e devolução, não como categoria explicitamente definida de recurso alocável. A aplicação deve atender aos perfis e às regras de reserva descritos na [especificação do projeto](./especificacao-projeto.md#1-objetivo) e no [PRD](./prd.md#1-visao-do-produto). A especificação lista como critérios impeditivos a aplicação não executar, a ausência de pipeline, falha de autorização e a possibilidade de reserva duplicada ([especificação, §8](./especificacao-projeto.md#8-criterios-impeditivos); [PRD, §6](./prd.md#6-criterios-impeditivos)).

### Stakeholders e personas

Os documentos descrevem os seguintes perfis; não identificam outros stakeholders nominais:

| Persona | Objetivos e atividades documentados | Fonte |
|---|---|---|
| Solicitante (professor ou coordenador) | Consultar disponibilidade e criar, alterar ou cancelar as próprias reservas. | [Personas — Solicitante](./personas.md#solicitante); [especificação, §2](./especificacao-projeto.md#2-perfis-de-usuario) |
| Responsável | Aprovar solicitações especiais, validar alocação de professores e acompanhar retirada e devolução de materiais e equipamentos. A regra de negócio determina separadamente que somente esse perfil aprova recursos restritos; os documentos não esclarecem se “solicitações especiais” e “recursos restritos” são conceitos relacionados. | [Personas — Responsável](./personas.md#responsavel); [regra 8](./regras-negocio.md#regras-de-negocio) |
| Administrador | Gerenciar salas, professores, materiais, usuários, bloqueios e períodos de manutenção. | [Personas — Administrador](./personas.md#administrador); [especificação, §2](./especificacao-projeto.md#2-perfis-de-usuario) |

As permissões além das ações expressamente listadas, os limites de “gerenciar” e os responsáveis por executar retirada e devolução não estão definidos ([personas, pontos de esclarecimento](./personas.md#pontos-que-precisam-de-esclarecimento)).

### Requisitos arquiteturalmente significativos

- **Identidade e controle de acesso:** autenticar usuários e autorizar ações por perfil; permitir gestão de usuários. O comportamento detalhado de cada permissão ainda é parcial (PRD RF-01; especificação §2–3; [personas, pontos de esclarecimento](./personas.md#pontos-que-precisam-de-esclarecimento)).
- **Alocação sem conflitos e concorrência:** o Solicitante pode criar, alterar e cancelar as próprias reservas; deve-se detectar sobreposição para recursos e conflitos na agenda dos professores, não podendo um professor ter reservas simultâneas. Solicitações simultâneas para o mesmo recurso e horário devem resultar em apenas uma reserva aceita (PRD RF-04–RF-06; [regras 2–4](./regras-negocio.md#regras-de-negocio); [personas — Solicitante](./personas.md#solicitante)). É critério impeditivo permitir reserva duplicada.
- **Ciclo de vida e integridade das reservas:** horário final posterior ao inicial; recursos em manutenção indisponíveis; reservas iniciadas não podem ser excluídas; toda mudança de estado deve ser registrada. O ciclo nominal é SOLICITADA → APROVADA → EM_USO → CONCLUÍDA, além de estados de alternativas rejeitadas, cancelamento e não comparecimento (PRD RF-04, RF-08, RF-10; [regras 1, 5–7, 9–10](./regras-negocio.md#regras-de-negocio)).
- **Aprovação de recursos restritos e solicitações especiais:** somente o Responsável aprova recursos restritos; o perfil Responsável também aprova solicitações especiais. Os documentos não estabelecem se esses conceitos são equivalentes ou qual é a relação entre eles. Critérios de restrição e fluxos de aprovação não foram especificados (PRD RF-07; [regra 8 e pontos de esclarecimento](./regras-negocio.md#pontos-que-precisam-de-esclarecimento)).
- **Cadastros, consulta e busca:** CRUD e consulta de salas, professores e materiais, busca por tipo, capacidade, localização, competência e disponibilidade, além da gestão de usuários e bloqueios (PRD RF-02–RF-03, RF-08). Não se deve presumir que o CRUD inclui usuários.
- **Materiais e equipamentos:** acompanhar retirada e devolução (PRD RF-09); fluxo, atores e estados desse acompanhamento permanecem indefinidos.
- **Histórico, relatórios e notificações:** manter histórico auditável, produzir relatórios de utilização, horas alocadas e conflitos evitados e oferecer notificações simuladas ou integração com API externa (PRD RF-10–RF-12). Conteúdo, escopo e comportamento detalhados não foram definidos.
- **Cron job de treinamento:** existe um cron job executado diariamente ao final do dia para treinar ou retreinar usando somente dados novos. **Origem:** exigência do prompt da atividade anterior; não é requisito confirmado nos documentos do produto. O significado de “dados novos”, a fonte, o critério de treinamento versus retreinamento e o resultado esperado não estão especificados.
- **Documentação e operação:** aplicação executável, instruções de instalação, documentação da API e fluxos públicos e pipeline de CI em Pull Requests são entregáveis/requisitos registrados (PRD RF-14; especificação §§5 e 7–8).

### Atributos de qualidade

| Atributo | Fatos e critérios registrados | Fonte |
|---|---|---|
| Confiabilidade e integridade da alocação | Sem conflitos; impedir reserva duplicada, inclusive sob concorrência; não reservar recursos em manutenção. | [RNF-01](./requisitos-nao-funcionais.md#rnf-01--confiabilidade-da-alocacao); [regras 2–5](./regras-negocio.md#regras-de-negocio) |
| Segurança | Autenticação, autorização por perfil, validação de entradas e erros seguros; meta declarada de zero bugs ou vulnerabilidades críticas. | [RNF-02](./requisitos-nao-funcionais.md#rnf-02--seguranca-e-autorizacao); [especificação, §§3 e 6](./especificacao-projeto.md#3-funcionalidades-obrigatorias) |
| Usabilidade e adaptação | Interface responsiva e mensagens de erro compreensíveis. | [RNF-03](./requisitos-nao-funcionais.md#rnf-03--usabilidade-e-adaptacao); PRD RF-13 |
| Auditabilidade | Histórico auditável de alterações e registro de toda mudança de estado de reserva. | [RNF-04](./requisitos-nao-funcionais.md#rnf-04--auditabilidade); PRD RF-10 |
| Testabilidade e qualidade de software | JUnit 5 para testes unitários, parametrizados, de integração, API black-box e E2E; Testcontainers para banco de dados; WireMock para integrações externas; testes automatizados de concorrência; TDD e BDD em ao menos uma funcionalidade; GitHub Actions para CI em Pull Requests; JaCoCo e SonarCloud. Metas: 80% de cobertura de linhas, 70% de branches e 100% dos requisitos críticos cobertos na RTM. | [RNF-05](./requisitos-nao-funcionais.md#rnf-05--testabilidade-e-qualidade-de-software); especificação §§5–6 |
| Desempenho sob carga | Testes de carga com JMeter e entrega de plano e resultados; não há limiares quantitativos de desempenho especificados. | [RNF-06](./requisitos-nao-funcionais.md#rnf-06--desempenho-sob-carga) |
| Documentação e entrega | API e fluxos públicos documentados; aplicação executável, instruções de instalação, README e entregáveis do projeto especificados. | [RNF-07](./requisitos-nao-funcionais.md#rnf-07--documentacao-e-entrega); especificação §7 |

As ferramentas acima são requisitos documentados para teste e qualidade, não decisões sobre tecnologias de implementação. A especificação determina também que mocks sejam usados somente em testes unitários isolados e que persistência e integrações críticas sejam testadas em ambiente real, com Testcontainers e WireMock ([especificação, §5](./especificacao-projeto.md#5-requisitos-de-qualidade-e-testes)).

### Entregáveis documentados

A especificação exige: repositório com histórico de alterações e revisão por pares; aplicação executável e instruções de instalação; README e documentação dos fluxos e da API; `RTM.md` relacionando requisitos, riscos, testes e evidências; diagramas de contexto, componentes e sequências críticas; plano de testes, suíte automatizada e relatório; pipeline de CI; relatórios JaCoCo e SonarCloud; plano e resultados dos testes de carga; registro de defeitos; relatório de qualidade; apresentação e demonstração da aplicação ([especificação, §7](./especificacao-projeto.md#7-entregaveis); [PRD, §5](./prd.md#5-entregaveis-do-projeto)).

### Drivers priorizados e origem

Esta priorização é provisória e analítica, baseada nos critérios impeditivos explícitos e na centralidade dos requisitos para a alocação. As fontes registram os requisitos, mas não confirmam esta ordem. Em particular, a posição do cron job decorre da exigência do prompt da atividade anterior; ela não representa prioridade confirmada do produto.

| Prioridade provisória | Driver | Origem |
|---|---|---|
| 1 — impeditivo | A aplicação deve executar e não pode permitir falha de autorização ou reserva duplicada; deve haver pipeline. | [Especificação, §8](./especificacao-projeto.md#8-criterios-impeditivos); [PRD, §6](./prd.md#6-criterios-impeditivos) |
| 2 — integridade da alocação | Detectar conflitos de recursos e professores, tratar concorrência sem duplicidade e bloquear recursos em manutenção. | PRD RF-05, RF-06 e RF-08; [regras 2–5](./regras-negocio.md#regras-de-negocio); [RNF-01](./requisitos-nao-funcionais.md#rnf-01--confiabilidade-da-alocacao) |
| 3 — segurança e governança | Autenticar e autorizar por perfil; restringir aprovações de recursos restritos ao Responsável; manter histórico de mudanças. | PRD RF-01, RF-07 e RF-10; [regra 8](./regras-negocio.md#regras-de-negocio); [RNF-02](./requisitos-nao-funcionais.md#rnf-02--seguranca-e-autorizacao) e [RNF-04](./requisitos-nao-funcionais.md#rnf-04--auditabilidade) |
| 4 — execução do treinamento agendado | Executar diariamente ao final do dia treinamento ou retreinamento usando somente dados novos. Esta posição é consequência da exigência do prompt, não de uma prioridade confirmada do produto. | Prompt da atividade anterior; não consta nos documentos de requisitos consultados. |
| 5 — cobertura funcional por perfis | Consultar, pesquisar, reservar, gerir cadastros, acompanhar materiais e equipamentos, notificar, gerar relatórios e documentar API e fluxos públicos conforme o escopo registrado. | PRD RF-02–RF-14; especificação §§2–3 |
| 6 — verificabilidade e entrega | Atender tipos de testes, metas de cobertura, RTM, CI, documentação e entregáveis definidos. | PRD §§4–5; [RNF-05](./requisitos-nao-funcionais.md#rnf-05--testabilidade-e-qualidade-de-software) e [RNF-07](./requisitos-nao-funcionais.md#rnf-07--documentacao-e-entrega); especificação §§5–7 |
| 7 — qualidade da interação e desempenho | Oferecer interface responsiva, mensagens compreensíveis e testes de carga, sem metas numéricas definidas para esses atributos. | PRD RF-13; [RNF-03](./requisitos-nao-funcionais.md#rnf-03--usabilidade-e-adaptacao) e [RNF-06](./requisitos-nao-funcionais.md#rnf-06--desempenho-sob-carga) |

### Glossário

| Termo | Significado documentado |
|---|---|
| Recurso | Item a alocar; os exemplos explicitamente listados como recursos são salas, professores e materiais. Equipamentos são citados no contexto de retirada e devolução, sem confirmação de que sejam uma categoria de recurso alocável. |
| Solicitante | Professor ou coordenador que consulta disponibilidade e gere as próprias reservas. |
| Responsável | Perfil que aprova solicitações especiais, valida alocação de professores e acompanha retirada e devolução de materiais e equipamentos; somente esse perfil aprova recursos restritos. A relação entre solicitações especiais e recursos restritos não está definida. |
| Administrador | Perfil que gerencia cadastros de recursos e usuários, bloqueios e períodos de manutenção. |
| Recurso restrito | Recurso cuja aprovação cabe somente ao Responsável; critérios para determinar restrição não especificados. |
| Solicitação especial | Tipo de solicitação que o Responsável deve aprovar; definição, critérios e relação com recursos restritos não especificados. |
| Reserva simultânea / sobreposição | Condição que deve ser impedida para o mesmo recurso e para a agenda de professores; limites temporais inclusivos/exclusivos não definidos. |
| Dados novos | Restrição do cron job informada no prompt da atividade anterior; critério de novidade e origem dos dados não definidos nos documentos do produto. |
| RTM | Matriz de rastreabilidade de requisitos, relacionando requisitos, riscos, testes e evidências, conforme o entregável descrito na especificação §7. |

### Fatos, lacunas, conflitos e suposições

- **Fatos:** os perfis, requisitos RF-01–RF-14, regras 1–10, requisitos de qualidade e metas acima estão registrados nos documentos vinculados. A restrição do cron job foi fornecida no prompt da atividade anterior e está identificada separadamente, sem ser apresentada como requisito confirmado do produto.
- **Lacunas:** permanecem indefinidos, entre outros, os critérios e a relação entre recursos restritos e solicitações especiais; critérios e etapas de aprovação e validação de alocação de professores; permissões detalhadas por perfil e limites de gerenciamento; campos e operações de CRUD e sua aplicação a usuários; associação de tipo, capacidade, localização e competência aos recursos e possibilidade de combinar filtros; transições e efeitos de cancelamento, rejeição e não comparecimento; semântica de sobreposição e fuso horário; tratamento de solicitações concorrentes não aceitas; relação entre bloqueios de manutenção e reservas existentes; processo e atores de retirada e devolução; política e conteúdo de notificações; escopo, formato e conteúdo de relatórios; critérios para classificar requisitos críticos e gerar/aprovar a RTM; escopo de fluxos públicos e documentação da API; metas de desempenho; critérios de responsividade; disponibilidade, recuperação, backup, retenção e proteção de dados; limiares SonarCloud; critérios de medição de erros seguros, mensagens compreensíveis e confiabilidade; e critérios de “dados novos” e do cron job.
- **Conflitos:** não foi identificado conflito textual direto entre os documentos consultados. Há uma ambiguidade de escopo entre a gestão de usuários atribuída ao Administrador (especificação §2 e PRD) e o CRUD explicitamente previsto apenas para salas, professores e materiais (PRD RF-02); isso não define se usuários também terão CRUD.
- **Suposições:** nenhuma tecnologia de implementação, topologia, volume, SLA ou regra de negócio adicional é assumida. A ordem dos drivers é uma priorização analítica, explicitada acima, não uma decisão já aprovada.

### Riscos iniciais

| Risco | Base documentada / lacuna relacionada |
|---|---|
| Reservas conflitantes ou duplicadas, especialmente sob solicitações concorrentes. | Critério impeditivo e PRD RF-05–RF-06; regras 2–4. |
| Negação indevida de acesso ou aprovação de recurso restrito por perfil não autorizado. | Critério impeditivo, PRD RF-01 e RF-07, regra 8; permissões incompletamente detalhadas em personas. |
| Interpretações diferentes do ciclo de vida, levando a operações ou históricos inconsistentes. | Regra 6 e estados adicionais da regra 7; transições não especificadas. |
| Bloqueio de manutenção afetar reservas existentes de forma não definida. | PRD RF-08 e regra 5; interação com reservas já existentes em aberto. |
| Treinamento usar dados que não atendem à restrição de novidade ou não ocorrer no horário esperado. | Restrição do cron job informada no prompt da atividade anterior; fonte e critério de “dados novos” ausentes nos documentos do produto. |
| Impossibilidade de demonstrar ou aceitar atributos de qualidade de desempenho, responsividade e segurança de forma objetiva. | RNF-02, RNF-03 e RNF-06; critérios ou limiares em aberto nos requisitos não funcionais. |
| Escopo ou comportamento divergente em notificações, relatórios e acompanhamento de materiais. | PRD RF-09, RF-11 e RF-12; atores, conteúdo e fluxos não detalhados no PRD. |

### Perguntas abertas

1. Quais permissões são adicionais ou compartilhadas entre perfis, quais operações compõem “gerenciar” e quem, além do Responsável, pode executar retirada e devolução?
2. O que torna uma solicitação especial ou um recurso restrito? Qual é a relação entre esses conceitos, quais critérios/etapas se aplicam à aprovação e à validação de alocação de professores e quais informações são necessárias?
3. Quais campos e operações compõem o CRUD e ele se aplica também a usuários? A que recursos correspondem tipo, capacidade, localização e competência, e quais filtros podem ser combinados?
4. Quais transições são permitidas para cancelamento, rejeição, alternativas rejeitadas e não comparecimento? O que ocorre com solicitações concorrentes não aceitas?
5. Como definir sobreposição de horários, limites temporais e fuso horário? O que acontece com reservas existentes quando uma manutenção ou bloqueio é criado?
6. Quais procedimentos, estados e atores controlam retirada e devolução de materiais e equipamentos?
7. Quais eventos, destinatários e conteúdo se aplicam às notificações, e será usada simulação ou integração? Quais são o escopo, formato e conteúdo dos relatórios?
8. Qual é a fonte de dados do cron job, o que conta como dado novo, como decidir entre treinar e retreinar e como comprovar a execução diária ao final do dia?
9. Quais são os limiares de latência, throughput, usuários concorrentes, volume de dados e duração dos testes de carga? Quais navegadores, dispositivos, resoluções e critérios definem responsividade?
10. Quais requisitos existem para disponibilidade, recuperação, backup, retenção e proteção de dados? Como medir erros seguros, mensagens compreensíveis e confiabilidade? A meta de zero bugs ou vulnerabilidades críticas também inclui bugs não críticos? Quais limiares do SonarCloud e abrangência de relatórios serão adotados?
11. Qual mecanismo será usado para gerar e aprovar a RTM, quais requisitos serão classificados como críticos e qual é o escopo dos fluxos públicos e da documentação da API?
