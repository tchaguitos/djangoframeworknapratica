# Módulo 06 - dev

## Criando tela para registro de novo visitante

Assim como quando registramos um novo visitante através do Admin, precisamos de um formulário para inserção das informações. Por isso, vamos trabalhar agora na tela que terá a responsabilidade de exibir um formulário e registrar o visitante em nosso banco de dados.

Uma view é um tipo de função dentro da aplicação que geralmente serve um template específico. Até agora escrevemos somente a view que nos disponibiliza o template inicial da dashboard - `index.html`. Sendo assim, a view apenas busca informações e renderiza o template.

A próxima view que vamos escrever, chamada de `registrar_visitante`, terá a responsabilidade de exibir um formulário, receber e tratar uma requisição do tipo POST, validar o formulário com base das informações enviadas e salvar o novo visitante no banco de dados. Não se assuste, você vai ver como o Django nos ajuda abstraindo a maior parte desses requisitos.

## Criando view para registrar visitante

Como você já deve ter percebido, existe um roteiro a ser seguido quando vamos criar novas views para nosso sistema web com Django, sendo o primeiro passo a criação da função de view no arquivo `views.py`. Como nossa função diz respeito ao registro de um novo vistante, vamos trabalhar dentro do aplicativo visitantes.

Vamos abrir o arquivo `views.py` do aplicativo visitantes e escrever a função de view `registrar_visitante`, que deverá renderizar o template `registrar_visitante.html`. Por hora, nossa view fará apenas isso, ficando assim:

```python
from django.shortcuts import render

def registrar_visitante(request):

    contexto = {}
    
    return render(request, "registrar_visitante.html", contexto)
```

Assim como fizemos anteriormente, vamos baixar o template e colocar na pasta **templates** localizada na raíz do nosso projeto.



## Adaptando o template para trabalhar com a template engine do Django

* [ ] Criando o template "base.html"
* [ ] Adaptando template "registrar\_visitante.html" para trabalhar com a engine do Django

## Trabalhando com formulários no Django

### Preparando view para receber requisição do tipo POST 

* falar sobre "request"
* validações do modelForm
* falar sobre redirect e csrf\_token

### Criando intimidade com os formulários

* falar sobre atributo fields e alterar para se adequar às nossas necessidades

## Conhecendo um pouco mais dos formulários

* [ ] Falar sobre relacionamento entre FKs
* [ ] Tratar problema de valor nulo para campo "autorizado\_por"

