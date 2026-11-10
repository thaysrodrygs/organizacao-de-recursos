# Regras de negócio

As regras abaixo preservam o significado e a ordem da [especificação do projeto](./especificacao-projeto.md).

1. O horário final deve ser posterior ao horário inicial.
2. O mesmo recurso não pode ter reservas com horários sobrepostos.
3. Um professor não pode ter reservas simultâneas.
4. Solicitações simultâneas para o mesmo recurso e horário devem resultar em apenas uma reserva aceita.
5. Recursos em manutenção não podem ser reservados.
6. Estados da reserva: **SOLICITADA → APROVADA → EM_USO → CONCLUÍDA**.
7. Também devem ser tratados os estados de alternativas rejeitadas, canceladas e não comparecimento.
8. Somente o Responsável aprova recursos restritos.
9. Reservas iniciadas não podem ser excluídas.
10. Toda mudança de estado deve ser registrada no histórico.

## Pontos que precisam de esclarecimento

- Não está definido se os limites de horário são inclusivos ou exclusivos ao determinar sobreposição, nem como tratar fusos horários ou reservas que atravessam mudanças de horário.
- A regra de concorrência não especifica o que ocorre com as solicitações não aceitas, nem se devem ser rejeitadas, mantidas em espera ou encaminhadas para outro fluxo.
- Não estão definidos os critérios que tornam um recurso “restrito” ou uma solicitação “especial”.
- A sequência de estados apresentada não descreve transições para cancelamento, rejeição ou não comparecimento, embora esses estados também devam ser tratados.
- Não está definido o significado operacional de “alternativas rejeitadas” nem a quais objetos ou estados se refere.
- Não está definido o que caracteriza uma reserva “iniciada”, nem se ela pode ser cancelada ou alterada; a regra apenas impede sua exclusão.
- Não está especificado se alterações de horário ou de recurso de uma reserva aprovada precisam de nova aprovação.
- Não está definida a relação entre manutenção, bloqueios e reservas já existentes quando o bloqueio é criado.
