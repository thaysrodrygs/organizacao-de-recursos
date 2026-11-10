# Requisitos não funcionais

Este documento organiza os requisitos de qualidade e validação registrados na [especificação do projeto](./especificacao-projeto.md). Os valores e ferramentas abaixo refletem a especificação; itens sem métrica definida permanecem em aberto.

## RNF-01 — Confiabilidade da alocação

- A aplicação deve ser confiável para alocar recursos sem conflitos de horários.
- Deve detectar conflitos, inclusive na agenda dos professores, e impedir reservas duplicadas simultâneas.
- Recursos em manutenção não podem ser reservados.
- **Validação prevista:** testes automatizados de concorrência e cobertura dos requisitos críticos na RTM.

## RNF-02 — Segurança e autorização

- A aplicação deve autenticar usuários e autorizar operações por perfil.
- Deve validar entradas e apresentar erros seguros.
- **Validação prevista:** testes de autorização, validação de entradas e erros seguros.
- **Meta de qualidade declarada:** “zero bugs ou vulnerabilidades críticas”. O escopo exato dessa formulação precisa de esclarecimento.

## RNF-03 — Usabilidade e adaptação

- A interface deve ser responsiva.
- As mensagens de erro devem ser compreensíveis.

## RNF-04 — Auditabilidade

- Alterações devem manter histórico auditável.
- Toda mudança de estado de reserva deve ser registrada no histórico.

## RNF-05 — Testabilidade e qualidade de software

- Usar JUnit 5 para testes unitários, parametrizados, de integração, API black-box e E2E.
- Usar banco de dados em contêiner com Testcontainers e WireMock para integrações externas.
- Automatizar testes de concorrência.
- Aplicar TDD e BDD em pelo menos uma funcionalidade.
- Usar mocks somente em testes unitários isolados. Persistência e integrações críticas devem ser validadas em ambiente real de teste, com Testcontainers e WireMock.
- Integrar GitHub Actions para CI em Pull Requests; usar JaCoCo e SonarCloud.
- **Metas:** cobertura de 80% das linhas e 70% dos branches; 100% dos requisitos críticos cobertos na RTM; “zero bugs ou vulnerabilidades críticas” (formulação da especificação).

## RNF-06 — Desempenho sob carga

- Realizar testes de carga com JMeter e entregar plano e resultados.
- A especificação não estabelece metas quantitativas de desempenho.

## RNF-07 — Documentação e entrega

- Documentar a API e os fluxos públicos.
- Entregar aplicação executável, instruções de instalação, README e documentação dos fluxos e da API.
- Manter repositório com histórico de alterações e revisão por pares, pipeline de CI e artefatos de qualidade previstos.

## Pontos que precisam de esclarecimento

- Não há metas numéricas para latência, throughput, usuários concorrentes, volume de dados ou duração dos testes de carga.
- Não são especificados navegadores, dispositivos, resoluções ou critérios de responsividade.
- Não estão definidos requisitos de disponibilidade, recuperação, backup, retenção ou proteção de dados.
- A meta de “zero bugs” aparece como “zero bugs ou vulnerabilidades críticas” nas metas de qualidade; não está esclarecido se bugs não críticos também entram na meta.
- Não está definido como medir ou comprovar “erros seguros”, “mensagens compreensíveis” ou confiabilidade.
- Não estão definidos os limiares de qualidade configurados em SonarCloud nem a abrangência exata dos relatórios.
