## Contexto
Em sistemas que utilizam cache para otimizar a leitura de dados, a abordagem mais comum � armazenar dados 
temporariamente e, quando o cache expira, ele � removido, for�ando o sistema a buscar os dados diretamente 
da fonte original. Embora essa estrat�gia seja eficiente para evitar dados desatualizados, 
ela pode gerar picos de carga significativos, especialmente quando m�ltiplas requisi��es simult�neas 
precisam recarregar o cache a partir da fonte original.
Esses picos de carga podem impactar negativamente o desempenho do sistema, causar atrasos em requisi��es e 
sobrecarregar os recursos da infraestrutura. Al�m disso, em sistemas de larga escala, o descarte 
simult�neo de caches expirados pode resultar em comportamentos imprevis�veis e prejudicar a disponibilidade.
Desta forma adotar uma t�cnica que permita a atualiza��o proativa do cache, antes que ele expire, 
evitando a exclus�o imediata e a recarga sob demanda.

## Solu��o
A proposta � a introdu��o do conceito de **idade m�nima para o cache**. 
Em vez de simplesmente descartar e remover um item do cache ao atingir o tempo de expira��o, 
o sistema verificaria se o item est� pr�ximo de expirar e, ao inv�s de remov�-lo, iniciaria um processo de atualiza��o.
Esse processo de atualiza��o garantiria que, quando o cache atingisse sua idade m�nima, 
ele fosse automaticamente renovado, mantendo os dados atualizados sem precisar ser removido. 
O sistema poderia atualizar o cache de maneira controlada, com menos impacto em termos de picos de carga, 
abaixo uma simula��o da carga de um sistema com e sem a solu��o.

![](CacheIdadeMinima.png)

### Vantagens
- **Redu��o de picos de carga:** O sistema evitaria picos de requisi��es � fonte original, 
pois o cache seria atualizado de maneira proativa e controlada.
- **Redu��o de custos:** A suaviza��o dos picos de carga reduziria o uso de recursos de infraestrutura, 
resultando em uma menor necessidade de provisionamento de capacidade extra, o que pode reduzir custos operacionais.
- **Consist�ncia de desempenho:** O sistema manteria um desempenho mais est�vel, 
j� que a atualiza��o do cache seria distribu�da ao longo do tempo.
- **Disponibilidade aprimorada:** Ao minimizar o tempo em que o cache fica vazio ou inv�lido, 
a disponibilidade dos dados seria melhorada.

### Quando aplicar?
Essa t�cnica faz sentido em sistemas que t�m grandes volumes de dados cacheados ou que experimentam picos de 
carga significativos quando o cache expira. Sistemas que requerem alta disponibilidade ou que 
enfrentam problemas de lat�ncia ao carregar dados da fonte original tamb�m podem se beneficiar dessa abordagem.
Sistemas que utilizam dados que mudam com frequ�ncia moderada podem obter bons resultados, pois podem possuir uma
idade minima pequena porem uma idade m�xima grande para suportar momentos de diminui��o de performance e disponibilidade
da fonte original.
Em sistemas onde o custo � importante esta solu��o pode ser uma boa abordagem tamb�m.
