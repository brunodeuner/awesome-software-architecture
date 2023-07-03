# Contexto
Este documento � um incremento de 
[processamento sequencial agrupado](ProcessamentoSequencialAgrupado.md) com a adi��o de uma 
complexidade de tratativa de erros, com o objetivo de manter a ordem dos eventos 
conforme um agrupamento mesmo quando processados com erro.

# Solu��o
![](ProcessamentoSequencialAgrupadoComTratativaDeErros.drawio.png)

A solu��o consiste em verificar se o Id processado possui erro, caso possua o mesmo deve ser armazenado
para ser processado posteriormente, este armazenamento tamb�m deve ocorrer ao processar 
com erro um evento. 

## Reprocessamento usando a mesma solu��o de processamento normal
Ao obter os eventos para reprocessamento ou processar um evento com erro, usar a mesma l�gica do distribuidor.

## L�gica do "agrupador possui erro?"
Consultar um banco de dados para cada evento � custoso, como o consumidor agrupado n�o deve 
possuir paralelismo pode ser criado uma l�gica em mem�ria, contendo os Id's com erro.
� importante possuir um limite e ao atingi-lo este recurso n�o deve ser mais utilizado 
ou o consumo parar.
Outra op��o � um valor booleano informando se a aplica��o possui eventos com erro ou n�o,
para evitar chamadas ao banco de dados.

## Armazenamento dos eventos com erro utilizando RabbitMQ ou Kafka
Criar uma fila para retentativas.

## Armazenamento dos eventos com erro utilizando banco de dados
Criar uma tabela para retentativas ou uma flag na pr�pria tabela de eventos.