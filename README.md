# Implementação 4 — Pint-OS Alarm Clock

Atividade da disciplina de Infraestrutura de Software com o objetivo de substituir a implementação de `timer_sleep()` baseada em busy wait por um mecanismo de bloqueio e despertar de threads.

## Objetivo

A implementação faz com que uma thread que solicita uma espera seja bloqueada durante o período necessário, sem permanecer consumindo CPU com chamadas repetidas a `thread_yield()`.

Quando o tick de despertar é atingido, a interrupção do temporizador solicita o desbloqueio da thread, que volta para a fila de threads prontas.

## Principais alterações

### `src/devices/timer.c`

- `timer_sleep()` calcula o tick em que a thread deve acordar.
- Esperas com valor zero ou negativo retornam imediatamente.
- A thread é bloqueada por meio de `thread_sleep()`.
- A interrupção do timer chama o mecanismo responsável por acordar threads cujo tempo de espera terminou.

### `src/threads/thread.c`

- Utilização de uma lista específica para threads adormecidas.
- Implementação de `thread_sleep()`.
- Implementação de `thread_wakeup()`.
- Desbloqueio das threads quando `wakeup_tick` é atingido.
- Ordenação da `ready_list` por prioridade utilizando `list_insert_ordered()`.
- Uso da prioridade tanto no desbloqueio quanto no `thread_yield()`.

### `src/threads/thread.h`

- Campo `wakeup_tick` associado à thread.
- Protótipos das funções utilizadas pelo mecanismo de sono e despertar.

## Compilação

A partir do diretório `src/threads`:

```bash
make clean
make
