# Capítulo 11

## Implementando melhorias em nossos templates

Olha só que legal: finalizamos as funcionalidades de maior valor da nossa dashboard. Os porteiros já podem registrar visitantes, autorizar a entrada deles após contato com um morador e ainda sinalizar que a visita foi encerrada. Bem bacana, não?

Agora que temos as principais funcionalidades prontas, vamos nos concentrar em melhorar a experiência de utilização da dashboard e ainda melhorar a estrutura do nosso projeto, de modo que seja mais fácil realizar futuras manutenções.

Começaremos com algumas alterações nos templates que visam melhorar a experiência de utilização da dashboard.

### Exibindo botão com função de "voltar" e "cancelar" em páginas de informações e registro de visitante

Nossa primeira melhoria será inserir os botões com as ações "cancelar" e "voltar" nas páginas de registro de visitante e informações de visitante. Sendo assim, vamos primeiro abrir o arquivo `registrar_visitante.html` e procurar pelo seguinte trecho de código:

```markup
<div class="text-right">
    <button class="btn btn-primary" type="submit">
        <span class="text">Registrar visitante</span>
    </button>
</div>
```

Esse trecho de código representa o botão que envia a requisição via método `POST` com as informações do formulário. Acima do elemento `<button class="btn btn-primary" type="submit">` vamos inserir um link para a página inicial da nossa dashboard com o texto "Cancelar", aproveitando algumas classes do bootstrap para que o link tenha a aparência de um botão. O código ficará assim:

```markup
<div class="text-right">
    <a href="{% url 'index' %}" class="btn btn-secondary text-white" type="button">
        <span class="text">Cancelar</span>
    </a>

    <button class="btn btn-primary" type="submit">
        <span class="text">Registrar visitante</span>
    </button>
</div>
```

{% hint style="info" %}
Como a única maneira que podemos chegar até a página de registro de um novo visitante é pelo início da dashboard e a ação que queremos realizar é "Cancelar" a operação de registro de um novo visitante, faz sentido utilizarmos apenas um link fixo para a home da dashboard
{% endhint %}

O template `informacoes_visitante.html` já possui o botão com a ação "voltar" visível, mas não temos um link para onde o botão deve enviar o usuário. Vamos procurar pelo seguinte trecho de código e inserir um link para a URL `index` em seu atributo `href`:

```markup
<div class="mr-1 text-right">
    <a href="#" class="btn btn-secondary text-white" type="button">
        <span class="text">Voltar</span>
    </a>
</div>
```

O código ficará assim:

```markup
<div class="mr-1 text-right">
    <a href="{% url 'index' %}" class="btn btn-secondary text-white" type="button">
        <span class="text">Voltar</span>
    </a>
</div>
```

### Melhorando a exibição do CPF do visitante

Uma outra melhoria interessante seria na exibição do CPF do visitante. Estamos exibindo os números todos sem nenhuma separação, como geralmente o número de CPF é apresentado. Aprendemos que é possível criar métodos nas classes modelo para que a gente altere comportamentos e até já criamos alguns que utilizamos para melhorar a exibição de alguns atributos. Agora vamos criar o método `get_cpf` que deverá retornar o CPF do visitante já formatado com pontos e traço.

Vamos abrir o arquivo `models.py` e abaixo do método `get_placa_veiculo` vamos criar o método `get_cpf` que, por enquanto, irá retornar o CPF caso o mesmo exista. O código ficará assim:

```python
# código acima omitido

def get_placa_veiculo(self):
    if self.placa_veiculo:
        return self.placa_veiculo

    return "Veículo não registrado"

def get_cpf(self):
    if self.cpf:
        return self.cpf

    return "CPF não registrado"

# código abaixo omitido
```

#### Conhecendo o f-strings do Python

Precisamos cortar a string algumas vezes para pegar apenas algumas partes e depois montar uma outra string com cada parte obedecendo aos pontos e ao traço do formato padrão para exibição de CPF \(`XXX.XXX.XXX-XX`\). Esse processo pode parecer um pouco complicado mas não é.

Antes de tudo, vamos recortar as partes que compõem o CPF. Vamos utilizar os índices da variável `cpf` \(que será igual ao CPF do visitante\) para recortar cada parte, os intervalos \(`[0:3]`, `[3:6]`, `[6:9]` e `[9:]`\). Além disso, vamos também criar variáveis para guardar as partes do CPF:

```python
def get_cpf(self):
    if self.cpf:
        cpf = self.cpf
        
        cpf_parte_um = cpf[0:3]
        cpf_parte_dois = cpf[3:6]
        cpf_parte_tres = cpf[6:9]
        cpf_parte_quatro = cpf[9:]
        
        # código abaixo omitido
```

Com as quatro partes do CPF recortadas e guardadas em variáveis, temos agora que colocá-las em ordem obedecendo o formato padrão do CPF. Para nos ajudar com isso, vamos utilizar um recurso do Python chamado strings literais ou `f-strings`.

Uma f-string \(ou string literal\) é toda cadeia de caracteres prefixada por `f` ou `F`, onde pode conter também campos para substituição de variáveis ou expressões, delimitadas por chaves `{}`. Utilizando uma string literal, podemos criar a string já formatada e inserir os intervalos recortados do CPF na ordem por meio dos campos de substituição da string literal. Abaixo temos a variável que vamos retornar no método, onde cada par de chaves `{}` será substituído por um intervalo da string que representa o CPF do visitante:

```python
cpf_formatado = f"{}.{}.{}-{}"
```

Agora vamos inserir as variáveis que representam as partes do CPF do vistante na ordem e dentro das chaves `{}` da variavel `cpf_formatado`, nossa string literal. E já que nosso objetivo é retornar o CPF já formatado, então vamos retornar essa variável no método `get_cpf`. O método ficará assim:

```python
def get_cpf(self):
    if self.cpf:
        cpf = self.cpf
        
        cpf_parte_um = cpf[0:3]
        cpf_parte_dois = cpf[3:6]
        cpf_parte_tres = cpf[6:9]
        cpf_parte_quatro = cpf[9:]

        cpf_formatado = f"{cpf_parte_um}.{cpf_parte_dois}.{cpf_parte_tres}-{cpf_parte_quatro}"

        return cpf_formatado
```

Feito isso, temos agora que substituir as chamadas ao atributo `cpf` do modelo pela chamada ao método `get_cpf`. Primeiro no template `index.html` e depois no `informacoes_visitante.html`.

```markup
<!-- código acima omitido -->

<tbody>
    {% for visitante in pagina_obj %}
        <tr>
            <td>{{ visitante.nome_completo }}</td>
            <td>{{ visitante.get_cpf }}</td>
            
            <!-- código abaixo omitido -->
```

E agora no `informacoes_visitante.html`:

```markup
<!-- código acima omitido -->
<div class="form-group col-md-6">
    <label>CPF</label>
    <input type="text" class="form-control" value="{{ visitante.get_cpf }}" disabled>
</div>
<!-- código abaixo omitido -->
```

### Exibindo mensagem de boas-vindas para usuário

Uma outra funcionalidade rápida que vamos implementar é a identificação do usuário logado e a exibição de uma mensagem de boas vindas para o mesmo, assim que ele entrar na home na dashboard. Vamos abrir o arquivo `views.py` do aplicativo `usuarios` e importar o módulo `messages`. O trecho das importações ficará assim:

```python
from django.shortcuts import render
from django.contrib import messages

from visitantes.models import Visitante
```

Agora tudo que precisamos fazer é adicionar uma mensagem, assim como fizemos com as mensagens de sucesso. A diferença aqui é que utilizaremos o método `info` ao invés de `success`. Nossa view ficará assim:

```python
def index(request):

    todos_visitantes = Visitante.objects.all()

    messages.info(
        request,
        f"Seja bem vindo, { request.user.porteiro }!"
    )

    context = {
        "nome_pagina": "Início da dashboard",
        "todos_visitantes": todos_visitantes,
    }

    return render(request, "index.html", context)

```

Como aprendemos sobre strings literais anteriormente, vamos utilizá-las aqui para escrever o texto que vamos passar na mensagem. Queremos exibir uma mensagem de boas vindas e identificar o nome do porteiro que está logado. Essa informações podemos tirar da variável `request` que possui a propriedade `user`. Como nossos usuários possuem uma relação 1 para 1 com nossos porteiros, o Django cria a propriedade `porteiro` no modelo de usuários, desta forma podemos acessar `request.user.porteiro`.

## Implementando melhorias na estrutura do nosso projeto

Uma medida interessante que vamos implementar é criar uma pasta de nome `apps` na raíz do nosso projeto para agrupar todos os nossos aplicativos. Antes disso, tudo que precisamos fazer é alterar o arquivo `settings.py`. Vamos importar o módulo sys e depois adicionar a pasta ao projeto.

```python
import os
import sys

# código abaixo omitido
```

Feito isso tudo que precisamos fazer é adicionar a seguinte linha de código abaixo da variável `DEBUG`:

```python
sys.path.append(os.path.join(BASE_DIR, "apps"))
```

Agora, vamos criar a pasta apps e mover as pastas dos nossos aplicativos para ela.

