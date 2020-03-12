# Capítulo 08 - dev

## Criando tela para exibir informações de visitante

Seguindo as especificações que recebemos do cliente, sabemos que existe a necessidade de visualização das informações do visitante na dashboard. Sendo assim, é necessário que a gente trabalhe para que seja possível buscar as informações de cada visitante registrado e exibir essas informações em um template à parte, afim de mostrar mais detalhes de cada visitante.

Se você observar a tabela que lista os visitantes recentes na página inicial da dashboard, vai notar que existe um link para ver informações detalhadas de cada visitante. O que vamos é desenvolver a view que deverá buscar o visitante separadamente e exibir as informações do mesmo.

## Criando a view

Como já sabemos, começaremos a trabalhar na funcionalidade pela função de view, no arquivo `views.py`. Abaixo da função `registrar_visitante()`, vamos criar a função `informacoes_visitante()`.

Essa função de view terá uma pequena diferença com relação à função `registrar_visitante()`, desta vez, além do argumento `request`, vamos também receber um argumento de nome `id`, que representará o `id` do visitante a ser buscado em nosso banco de dados. É assim que vamos identificar qual visitante buscar.



```python
from django.contrib import messages
from django.shortcuts import render, redirect
from visitantes.forms import VisitanteForm

# código acima omitido
    return render(request, "registrar_visitante.html", contexto)


def informacoes_visitante(request, id):
    pass
```

* Recebendo token em view
* Utilizando o get\_model\_or\_404

## Criando template

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

