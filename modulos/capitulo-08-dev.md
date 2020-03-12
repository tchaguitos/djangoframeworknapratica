# Capítulo 08 - dev

## Criando tela para exibir informações de visitante

Seguindo as especificações que recebemos do cliente, sabemos que existe a necessidade de visualização das informações do visitante na dashboard. Sendo assim, é necessário que a gente trabalhe para que seja possível buscar as informações de cada visitante registrado e exibir essas informações em um template à parte, afim de mostrar mais detalhes de cada visitante.

Se você observar a tabela que lista os visitantes recentes na página inicial da dashboard, vai notar que existe um link para ver informações detalhadas de cada visitante. O que vamos é desenvolver a view que deverá buscar o visitante separadamente e exibir as informações do mesmo.

## Criando a view

Como já sabemos, começaremos a trabalhar na funcionalidade pela função de view, no arquivo `views.py`. Abaixo da função `registrar_visitante()`, vamos criar a função `informacoes_visitante()`.

Essa função de view terá uma pequena diferença com relação à função `registrar_visitante()`, desta vez, além do argumento `request`, vamos também receber um argumento de nome `id`, que representará o `id` do visitante a ser buscado em nosso banco de dados. É assim que vamos identificar qual visitante buscar.

Para adiantar um pouco, já vamos criar a view e deixar algumas coisas prontas, como a variável `context` e o retorno renderizando o template `informacoes_visitante.html`, que ainda vamos criar. Por hora, nossa função `informacoes_visitante()` ficará assim:

```python
# código acima omitido

def informacoes_visitante(request, id):
    
    context = {
        "nome_pagina": "Informações de visitante",
    }
    
    return render(request, "informacoes_visitante.html", context)
```

### Conhecendo o atalho get\_model\_or\_404

Agora que nossa view está escrita e recebe um `id`, precisamos buscar o visitante em nosso banco de dados utilizando esse `id`. Existem várias formas de fazer isso, mas vamos utilizar o atalho `get_model_or_404()` do Django. Para utilizá-lo, temos que passar uma classe modelo a ser utilizada na busca e o parâmetro pelo qual queremos buscar \(em nosso caso, utilizaremos o `id` e, por isso, vamos passá-lo como segundo argumento da função `get_model_or_404()`\).

Antes de tudo, claro, vamos importar a função `get_model_or_404()` em nossa view juntamente com a classe modelo `Visitante`, pois precisaremos dela. A função `get_model_or_404()` está localizada no mesmo pacote que as funções `render` e `redirect`, desta forma, vamos alterar as primeiras linhas do nosso código para o seguinte:

```python
from django.contrib import messages
from django.shortcuts import (
    render, redirect, get_object_or_404
)

from visitantes.models import Visitante
from visitantes.forms import VisitanteForm

# código abaixo omitido
```

Conforme vimos, para utilizar a função `get_object_or_404()`, precisamos passar a classe modelo e o atributo a ser utilizado para busca. Vamos passar a classe `Visitante` e dizer que vamos buscar o visitante pelo `id` e que o `id` é igual à variável que estamos recebendo como argumento da função `informacoes_visitante()`. Nosso código ficará assim:

```python
def informacoes_visitante(request, id):
    
    visitante = get_object_or_404(Visitante, id=id)
    
    context = {
        "nome_pagina": "Informações de visitante",
        "visitante": visitante
    }
    
    return render(request, "informacoes_visitante.html", context)
```

Não vamos nos esquecer de passar a variável `visitante` no contexto, para que possamos acessá-la nos templates!

## Criando template

Com nossa view pronta, agora precisamos criar o template `informacoes_visitante.html` e colocá-lo na pasta templates do nosso projeto. Mais uma vez, você pode fazer download do template clicando no link abaixo:

continuar...





## Criando URL para acessar informações de visitante

* Recebendo variáveis através da URL no Django

## Utilizando o Django para renderizar nossas URLs

## Aprendendo um pouco mais sobre URLs no Django

* Recebendo informações por meio das URLs

## Modelos do Django e seus métodos

* get\_status\_display\(\)
* h[ttps://docs.djangoproject.com/en/2.2/ref/models/instances/\#django.db.models.Model.get\_FOO\_display](https://docs.djangoproject.com/en/2.2/ref/models/instances/#django.db.models.Model.get_FOO_display)

## Criando métodos personalizados para exibir informações do Visitante

* Exibindo atributos somente quando preenchidos
  * Criando método get\_horario\_autorizacao
  * Criando método get\_autorizado\_por
  * Criando método get\_horario\_saida

## Utilizando métodos personalizados no template

Com nossos métodos peronslizados criados, temos que 

