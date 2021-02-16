# Capítulo 14

## Aprendendo a filtrar nossos visitantes por data

No capítulo anterior nós aprendemos um pouco sobre os métodos `filter()` e `count()` e até já filtramos os visitantes por status. Agora o que precisamos fazer é filtrar os visitantes com base na data em que o mesmo foi cadastrado para que depois seja possível filtramos esses registros por um mês em específico, que é o nosso objetivo.

Para fazer isso, vamos novamente utilizar o método `filter()`, mas dessa vez passando uma condição diferente, pois agora o que queremos é filtrar pela data de chegada. Por hora, vamos somente passar uma data para que o Django filtre somente os visitantes registrados nessa data em específico

## Conhecendo o field lookups da Queryset API

Antes de filtrar o horário de chegada pela data, precisamos conhecer os `field lookups`. Eles são filtros que nos possibilitam especificar algumas condições para nossos campos ou acessar propriedades. Por exemplo, nosso campo `horario_chegada` também recebe uma data, pois é do tipo `DateTimeField`. Sendo assim, podemos acessar somente a data existente no valor do campo.

Podemos utilizá-los com alguns métodos das querysets e entre eles o método `filter()`. Nós vamos utilizar `__` após o nome do atributo que passamos como condição para o método `filter()`. Se a gente quiser filtrar os visitantes pela data contida no atributo `horario_chegada`, podemos fazer isso:

```python
visitantes_mes = todos_visitantes.filter(
    horario_chegada__date="2020-04-04"
)
```

Nesse caso, estamos buscando apenas os visitantes que foram registrados no dia 04 de maio de 2020. Existem vários outros _lookups_ e cada tipo de campo possui os seus.

## Filtrando apenas os registros do mês atual

Agora que sabemos que é possível filtrar por propriedades contidas em alguns atributos e até filtramos os visitantes de um dia específico, vamos conhecer um outro `field lookup`, o `month`. Assim como nós podemos buscar somente pela data, que está contida no `DateTimeField`, podemos também buscar pelo mês, que é o que esse _lookup_ faz. Dessa vez vamos buscar todos os visitantes registrados no mês de maio \(mês 05\).

```python
visitantes_mes = todos_visitantes.filter(
    horario_chegada__month="05"
)
```

### Utilizando o timezone para descobrir o mês atual

Descobrimos como filtrar nossos visitantes pelo mês em que foram registrados, mas ainda precisamos fazer com que o método filtre os visitantes do mês atual de forma automática, isto é, que o mês atual seja reconhecido e passado para o método `filter()` como uma variável.

Para fazer isso vamos utilizar um velho conhecido, o `timezone.now()`. Esse método retorna o valor referente a data e hora em que foi invocado e, a partir dessa data e hora, podemos buscar o mês. Para importar o módulo `timezone` basta utilizar o seguinte código:

```python
from django.utils import timezone
```

Feito isso, vamos criar a variável `hora_atual` sendo igual ao método `timezone.now()` e depois acessar a propriedade `month` da variável `hora_atual` para passá-la à variável que vamos criar de nome `mes_atual`.

```python
# filtrando visitantes por data (mês atual)
hora_atual = timezone.now()
mes_atual = hora_atual.month

visitantes_mes = todos_visitantes.filter(
    horario_chegada__month=mes_atual
)
```

Agora temos o valor referente ao mês atual numa variável e podemos passá-la como valor para a condição do método `filter()`. Com isso nós já temos uma lista com os visitantes que foram registrados no mês atual. Vamos agora passar essa variável no contexto da função e depois exibi-la no template.

```python
context = {
    "nome_pagina": "Página inicial",
    "visitantes": visitantes,
    "visitantes_aguardando": visitantes_aguardando.count(),
    "visitantes_em_visita": visitantes_em_visita.count(),
    "visitantes_finalizado": visitantes_finalizado.count(),
    "visitantes_mes": visitantes_mes.count(),
}
```

Agora vamos voltar para o arquivo `index.html` e alterar o último trecho, onde vamos exibir a quantidade de visitantes registrados no mês atual:

```markup
<div class="col-xl-3 col-md-6 mb-4">
    <div class="card border-left-info shadow h-100 py-2">
        <div class="card-body">
            <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                    <div class="text-xs font-weight-bold text-info text-uppercase mb-1">Visitantes registrados no mês atual</div>
                    <div class="h5 mb-0 font-weight-bold text-gray-800">{{ visitantes_mes }}</div>
                </div>

                <div class="col-auto">
                    <i class="fas fa-users fa-2x text-gray-300"></i>
                </div>
            </div>
        </div>
    </div>
</div>
```

Ao voltar para a página inicial da dashboard, o número de visitantes registrados no mês já estará sendo exibido.

## Ordenando nossa lista de visitantes por horário de chegada

O último passo antes da gente finalizar mais um capítulo é ordenar a lista de visitantes recentes por horário de chegada. Isto é, queremos que os registros de visitantes sigam uma certa ordem e essa ordem seja baseada no horário de chegada, de forma que os registros mais recentes fiquem sempre no topo.

Para isso vamos utilizar o método `order_by()` que funciona de forma parecida com o `filter()`, com a diferença que precisamos passar somente o nome do atributo que queremos utilizar como parâmetro. Vamos utilizá-lo em substituição ao método `all()`: 

```python
todos_visitantes = Visitante.objects.order_by(
    "-horario_chegada"
)
```

Note que estamos utilizando o operador \(`-`\) antes do nome do atributo `horario_chegada`. O operador utilizado dessa forma faz com que os registros sejam listados em ordem descendente que, para data, tem o comportamento de ordenar do registro mais recente para o mais antigo.

