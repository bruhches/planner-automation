# Lógica do fluxo

## Objetivo

Determinar se a data atual pertence a um dos dias ativos de uma escala alternada 12x36.

## Expressão sanitizada

```text
mod(
  div(
    sub(
      ticks(
        formatDateTime(
          convertTimeZone(
            utcNow(),
            'UTC',
            '<FUSO_HORARIO>'
          ),
          'yyyy-MM-dd'
        )
      ),
      ticks('<DATA_BASE>')
    ),
    864000000000
  ),
  2
)
```

A condição verifica se o resultado é igual a `0`.

## Como funciona

1. `utcNow()` obtém o instante atual.
2. `convertTimeZone()` converte o horário UTC para o fuso utilizado pela operação.
3. `formatDateTime(..., 'yyyy-MM-dd')` remove a influência do horário e mantém apenas a data.
4. `ticks()` converte as datas em ticks.
5. `sub()` calcula a diferença entre a data atual e a data-base.
6. A divisão por `864000000000` converte a diferença de ticks para dias.
7. `mod(..., 2)` determina a paridade do número de dias transcorridos.
8. Resultado `0` representa um dia da mesma paridade da data-base.

## Por que usar uma data-base?

Uma recorrência configurada simplesmente como “a cada dois dias” pode se desalinhavar quando o fluxo é recriado, alterado ou tem sua data de início modificada. Uma data-base explícita transforma a decisão em uma regra calculada a partir do calendário.

## Vencimento das tarefas

Um horário pode ser associado à data local do dia usando uma expressão como:

```text
concat(
  formatDateTime(
    convertTimeZone(utcNow(),'UTC','<FUSO_HORARIO>'),
    'yyyy-MM-dd'
  ),
  'T08:00:00'
)
```

Troque `08:00:00` pelo horário desejado para cada tarefa.

## Observação sobre 12x36

Esta implementação modela a alternância de **dias de trabalho e folga** usada no caso original. Ela não controla jornada, ponto ou legislação trabalhista; apenas decide em quais datas o conjunto de tarefas deve ser criado.
