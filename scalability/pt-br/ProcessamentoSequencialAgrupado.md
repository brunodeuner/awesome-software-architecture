# Contexto
Fila contendo eventos de transa��o financeira que devem ser processadas sequencialmente conforme Id 
incremental e reincidente.

Pilares ordenados por ordem de import�ncia: efici�ncia, escalabiliade, manutenibilidade, performance.

Volume esperado:

| Percentil | TPS    |
| :---      | ---:   |
| 99,99     | 10.000 |
| 99        | 1000   |
| 90        | 500    | 
| 50        | 100    |
| 10        | 10     |

# Solu��o
![](ProcessamentoSequencialAgrupado.drawio.png)

A solu��o consiste em um unico consumidor consumir os eventos de transa��o financeira, o valor do agrupador
consiste no resto da divis�o do Id da transa��o por N, onde N � a quantidade desejada de escala horizontal.

## Kafka
Criar uma parti��o por agrupador e um consumidor para cada parti��o.
Um �nico publicador no Kafka suprir� p90 e conforme o aumento da escala horizontal desejada ir� suprir o pico.

## RabbitMQ
Criar uma fila por agrupador e um consumidor para cada fila.
Um �nico publicador no RabbitMQ suprir� p99 e conforme o aumento da escala horizontal desejada ir� suprir o pico.

## Banco de dados
Criar uma tabela que contenha uma coluna indexada pelo agrupador e criar um servi�o que consuma um 
unico agrupador.
Um �nico publicador no banco de dados suprir� o pico.

## Diferen�as entre as solu��es
- O banco de dados pode ser uma melhor escolha para retentativas em sistemas que precisam de uma ordem perfeita.
- Caso os eventos n�o precisem ser armazenados o RabbitMQ provavelmente ser� a tecnologia mais eficiente.



