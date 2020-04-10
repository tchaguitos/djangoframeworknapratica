# Capítulo 13 - dev

## Aprendendo a filtrar nossos visitantes por data

No capítulo anterior nós aprendemos um pouco sobre os métodos `filter()` e `count()` a até filtramos os visitantes por status. Agora o que precisamos fazer é filtrar os visitantes com base na data em que o mesmo foi cadastrado para que depois seja possível filtramos esses registros por um mês em específico, que é o nosso objetivo.

Para fazer isso, vamos novamente utilizando o método `filter()`, mas dessa vez passando uma condição diferente, pois agora o que queremos é filtrar pela data de chegada. Por hora, vamos somente passar uma data para que o Django filtre somente os visitantes registrados nessa data em específico

## Conhecendo o field lookups da Queryset API

Mas antes, vamos conhercer o `field lookups`. Eles são filtros que nos possibilitam acessar propriedades dos atributos do nosso campo. Por exemplo, nosso campo `horario_chegada` também recebe uma data, pois é do tipo `DateTimeField`. Sendo assim, podemos acessar somente a data existente no valor do campo.

Podemos utilizá-los com alguns métodos das querysets e entre eles o `filter()`. Nós vamos utilizar `__` após o nome do atributo que passamos como condição para o método `filter()`. Se a gente quiser filtrar os visitantes pela data contida no atributo `horario_chegada`, podemos fazer isso:

```python
visitantes_mes = visitantes.filter(
    horario_chegada__date="2020-04-04"
).count()
```

Nesse caso, estamos filtrando apenas os registros de visitantes que foram registrados no dia 04 de abril de 2020. Existem vários outros lookups e cada tipo de campo possui os seus, além de ser possível utilizá-los de formas diferentes.

## Filtrando apenas os registros do mês atual

Agora que sabemos que é possível filtrar por propriedades contidas nos atributos e até filtramos os visitantes de um dia específico, vamos conhecer um outro `field lookup`, o `month`. Assim como nós podemos buscar somente pela data, que está contida no `DateTimeField`, podemos buscar o mês, que é o que esse lookup faz. Dessa vez vamos buscar todos os visitantes registrados no mês de abril \(mês 04\).

```python
visitantes_mes = visitantes.filter(
    horario_chegada__month="04"
).count()
```

### Utilizando o datetime para descobrir o mês atual

```python
# filtrando visitantes por data (mês atual)
hora_atual = datetime.now()
mes_atual = hora_atual.month

visitantes_mes = visitantes.filter(
    horario_chegada__month=mes_atual
).count()
```

## Ordenando nossa busca por data e hora

O último passo antes da gente finalizar mais um capítulo é ordenar a lista de visitante por horário de chegada. Isto é, queremos que os registros de visitantes sigam uma certa ordem e o parâmetro será a ordem de chegada de forma que os registros mais recentes fiquem sempre no topo.

Para isso vamos utilizar o método `order_by()` que funciona de forma parecida com o `filter()`, com a diferença que precisamos passar somente o nome do atributo que queremos utilizar de parâmetro. Vamos utilizá-lo em substituição ao método `all()`: 

```python
visitantes = Visitante.objects.order_by(
    "-horario_chegada"
)
```

Note que estamos utilizando o operador \(`-`\) antes do nome do atributo `horario_chegada`. O operador utilizado dessa forma faz com que os registros sejam listados em ordem descendente que, para data, tem o comportamento de ordenar do registro mais recente para o mais antigo.

