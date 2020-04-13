# Capítulo 15 - dev

## Paginando listagem de visitantes

Nossa dashboard está praticamente finalizada e pronta para ser entrega ao cliente. Criamos uma dashboard que lista os visitantes recentes e apresenta alguns dados de forma resumida na página inicial, registra novos visitantes, visualiza informações de visitantes e autoriza e finaliza as visitas. Além disso, criamos também toda a parte de login e logout para que os usuários possam se autenticar para visualizar as URLs que agora não mais permitem acessos não autenticados.

No decorrer do curso, aprendemos sobre diversas ferramentas que o Django framework nos dá e vimos como utilizá-las em um projeto do mundo real. A última ferramenta a ser conhecida e melhoria a ser implementada, será a paginação dos resultados dos visitantes recentes. Por agora, com poucos visitantes registrados, não temos problemas, mas pense se o banco de dados tiver 5000 registros de visitantes. Não seria interessante a gente buscar todo esse volume de dados de uma vez e muito menos exibí-los na template.

Pensando nisso, o Django possui a classe `Paginator`, que é quem vai nos ajudar a paginar os resultados do nosso banco de dados. Paginação é um processo conhecido que visa fatiar e exibir os resultados por páginas de quantidade a ser definida.

{% hint style="info" %}
É comum, por exemplo, que as aplicações exibam 10 registros por vez e tenha links com o número das páginas para que você possa navegar nos resultados
{% endhint %}

Vamos conhecer essa classe e utilizá-la para paginar nossos visitantes de forma que a gente exiba 10 registros por vez.

## Conhecendo a classe Paginator

A classe `Paginator` do Django é quem abstrai toda a lógica necessária para paginar os resultados de uma queryset para nós. Tudo que precisamos é fornecer uma página

```python
from django.shortcuts import render
from django.contrib.auth.decorators import login_required
from django.core.paginator import Paginator

# código abaixo omitido
```



```python
# paginando resultados para exibir de 10 em 10 itens
numero_pagina = request.GET.get('page', 1)
visitantes_paginados = Paginator(visitantes, 10)
pagina_obj = visitantes_paginados.get_page(numero_pagina)
```



```python
context = {
    "nome_pagina": "Página inicial",
    "visitantes_em_visita": visitantes_em_visita,
    "visitantes_aguardando": visitantes_aguardando,
    "visitantes_finalizado": visitantes_finalizado,
    "visitantes_mes": visitantes_mes,
    "pagina_obj": pagina_obj
}
```

## Alterando template para exibir resultados paginados

```python
# alterar de:
{% for visitante in visitantates %}

# para:
{% for visitante in pagina_obj %}
```



## Adicionando links no template para navegar nos resultados



```markup
<nav aria-label="Page navigation example">
    <p class="mr-2 text-right">
        <small>
            Página {{ pagina_obj.number }} de um total de {{ pagina_obj.paginator.num_pages }}
        </small>
    </p>

    <ul class="pagination justify-content-end">
        {% if pagina_obj.has_previous %}
            <li class="page-item">
                <a class="page-link" href="?page={{ pagina_obj.previous_page_number }}" tabindex="-1">Página anterior</a>
            </li>
        {% endif %}

        {% for numero_pagina in pagina_obj.paginator.page_range %}
            {% if pagina_obj.number == numero_pagina %}
                <li class="page-item active">
                    <a class="page-link" href="">{{ numero_pagina }}</a>
                </li>
            {% else %}
                <li class="page-item">
                    <a class="page-link" href="?page={{ numero_pagina }}">{{ numero_pagina }}</a>
                </li>
            {% endif %}
        {% endfor %}

        {% if pagina_obj.has_next %}
            <li class="page-item">
                <a class="page-link" href="?page={{ pagina_obj.next_page_number }}">Próxima página</a>
            </li>
        {% endif %}
    </ul>
</nav>
```

