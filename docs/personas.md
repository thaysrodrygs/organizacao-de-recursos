# Personas

Este documento descreve os perfis de usuário previstos na [especificação do projeto](./especificacao-projeto.md). As descrições se limitam às atividades e permissões nela indicadas; não pressupõem dados pessoais, características demográficas ou comportamentos não especificados.

## Solicitante

**Perfil previsto:** professor ou coordenador.

- **Objetivos:** encontrar recursos disponíveis e gerir as próprias reservas.
- **Necessidades:** consultar disponibilidade e criar, alterar ou cancelar reservas próprias.
- **Responsabilidades:** fazer solicitações e manter suas reservas de acordo com as regras do sistema.
- **Permissões especificadas:** consultar disponibilidade; criar, alterar e cancelar as próprias reservas.
- **Dificuldades a considerar:** evitar conflitos de horário e reservas duplicadas; entender, por mensagens compreensíveis, quando uma solicitação não pode ser atendida. A especificação não detalha quais informações ou alternativas devem ser apresentadas.

## Responsável

- **Objetivos:** avaliar solicitações especiais, validar alocações de professores e acompanhar a movimentação de materiais e equipamentos.
- **Necessidades:** aprovar solicitações para recursos restritos, validar alocação de professores e acompanhar retirada e devolução de materiais e equipamentos.
- **Responsabilidades:** executar essas aprovações, validações e acompanhamentos.
- **Permissões especificadas:** aprovar recursos restritos; a regra de negócio determina que somente o Responsável pode fazê-lo. As permissões para as demais atividades são descritas como responsabilidades, sem detalhamento de acesso no sistema.
- **Dificuldades a considerar:** evitar aprovações indevidas de recursos restritos e acompanhar retiradas e devoluções. A especificação não define os critérios de aprovação nem o processo de acompanhamento.

## Administrador

- **Objetivos:** manter os cadastros de recursos e usuários e gerir indisponibilidades.
- **Necessidades:** gerenciar salas, professores, materiais e usuários; gerenciar bloqueios e períodos de manutenção.
- **Responsabilidades:** manter esses cadastros e bloqueios conforme as necessidades de uso dos recursos.
- **Permissões especificadas:** gerenciar as entidades e os bloqueios citados. A especificação não discrimina operações ou limites dentro desse gerenciamento.
- **Dificuldades a considerar:** manter os dados e períodos de bloqueio atualizados para que recursos em manutenção não sejam reservados. A especificação não define o processo de manutenção nem os campos dos cadastros.

## Pontos que precisam de esclarecimento

- A especificação não detalha permissões dos perfis além das atividades explicitamente listadas, nem se há permissões adicionais ou compartilhadas.
- Não são definidos os limites do gerenciamento do Administrador (por exemplo, quais operações compõem “gerenciar”).
- Não são definidos critérios, etapas ou informações necessárias para aprovação de solicitações especiais ou validação de alocação de professores.
- Não são definidos os procedimentos de retirada e devolução nem quem, além do Responsável, pode executá-los.
