# Capítulo 13 - dev

## Aprendendo a filtrar nossos visitantes por data

No capítulo anterior nós aprendemos um pouco sobre os métodos `filter()` e `count()` a até filtramos os visitantes por status. Agora o que precisamos fazer é filtrar os visitantes com base na data em que o mesmo foi cadastrado para que depois seja possível filtramos esses registros por um mês em específico, que é o nosso objetivo.

Para fazer isso, vamos novamente utilizando o método `filter()`, mas dessa vez passando uma condição diferente, pois agora o que queremos é filtrar pela data de chegada. Por hora, vamos somente passar uma data para que o Django filtre somente os visitantes registrados nessa data em específico

## Conhecendo o field lookups da Queryset API

Mas antes, vamos conhercer o field lookups. Eles são filtros que nos possibilitam acessar as propriedades dos atributos do nosso campo. Por exemplo, nosso campo horario\_chegada também recebe uma data, pois é do tipo DateTimeField. Sendo assim, podemos acessar somente a data existente no campo.

* horario\_chegada\_\_date

## Filtrando apenas os registros do mês atual

### Utilizando o datetime para descobrir o mês atual

## Ordenando nossa busca por data e hora

