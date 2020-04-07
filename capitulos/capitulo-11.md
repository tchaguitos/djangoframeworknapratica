# Capítulo 11

## Implementando melhorias em nossos templates

Olha só que legal: finalizamos as funcionalidades de maior valor da nossa dashboard. Os porteiros já podem registrar visitantes, autorizar a entrada deles após contato com um morador e ainda sinalizar que a visita foi encerrada. Bem bacana, não?

Agora que temos as principais funcionalidades prontas, vamos nos concentrar em melhorar a experiência de utilização da dashboard e ainda melhorar a estrutura do nosso projeto, de modo que seja mais fácil realizar futuras manutenções.

Começaremos com algumas alterações nos templates.

### Exibindo botão com função de "voltar" e "cancelar" em páginas de informações e registro de visitante

Nossa primeira melhoria será inserir os botões "cancelar" e "voltar" nas páginas de registro de visitante e informações de visitante. Sendo assim, vamos abrir o arquivo `registrar_visitante.html` e procurar pelo seguinte trecho de código:

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
Como a única maneira que podemos chegar até a página de registro de um novo visitante é pelo início da dashboard e a ação que queremos realizar é "Cancelar" a operação de registro de um novo visitante, faz sentido utilizarmos apenas um link fixo
{% endhint %}

### Melhorando a exibição do CPF do visitante

Uma outra melhoria interessante seria na exibição do CPF do visitante. Estamos exibindo os números todos sem nenhuma separação como geralmente o número de CPF é apresentado. Aprendemos que é possível criar métodos nas classes modelo para que a gente altere comportamentos e até já criamos alguns que utilizamos para melhorar a exibição de alguns atributos. Sendo assim, vamos criar o método `get_cpf` que deverá retornar o número do CPF do visitante já formatado com pontos e traço.

Vamos abrir o arquivo models.py e abaixo do método `get_placa_veiculo` vamos criar o método `get_cpf` que, por enquanto, irá retornar o CPF caso o mesmo exista. O código ficará assim:

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

Pensa bem, precisamos cortar a string algumas vezes para pegar apenas algumas partes e depois montar cada parte obedecendo aos pontos e ao traço do formato padrão para exibição de CPF \(`XXX.XXX.XXX-XX`\). Esse processo pode parecer um pouco complicado mas não é.

Antes de tudo, vamos recortar as partes que compõem o CPF e depois criar uma string já com os pontos e o traço. Vamos utilizar os índices da string CPF \(que será igual ao CPF do visitante\) para recortar cada parte, os intervalos \(`[0:3]`, `[3:6]`, `[6:9]` e `[9:]`\), e depois criar a string já formatada.

Para nos ajudar, vamos utilizar um recurso do `Python3.6` chamado strings literais ou `f-strings`. f-string \(ou string literal\) é toda cadeia de caracteres prefixadas por `f` ou `F`, onde pode conter também campos para substituição ou expressões delimitadas por chaves `{}`. Dessa forma, podemos criar a string já formatada e utilizando os intervalos do CPF por meio dos campos para substituição.

Nossa variável ficaria assim:

```python
cpf_mascarado = f"{}.{}.{}-{}"
```

Onde cada `{}` teria um intervalo da string que representa o CPF do visitante. Vamos substituir as chaves pelos intervalos dentro do método, retornar a variável `cpf_mascarado` e ver o que acontece:

```python
def get_cpf(self):
    if self.cpf:
        cpf = self.cpf

        cpf_mascarado = f"{cpf[:3]}.{cpf[3:6]}.{cpf[6:9]}-{cpf[9:]}"

        return cpf_mascarado
```

Feito isso, temos agora que substituir as chamadas ao atributo cpf do modelo pela chamada ao método `get_cpf`. Primeiro no template `index.html` e depois no `informacoes_visitante.html`.

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

### Exibindo informações do usuário logado diretamente no template

Uma outra alteração que vamos fazer é acessar a variável referente ao usuário logado na requisição diratamente no template ao invés de passarmos ela como variável no contexto das views. Quando nos esquecemos de passar essa variável, inclusive, a informação não é exibida.

Para evitar que isso aconteça vamos abrir nosso template `base.html` e procurar pela variável `{{ usuario_logado }}` e substituí-la por `{{ request.user.email }}`. Desta forma estamos acessando a informação diretamente pelo template e não precisamos obter essa informação em nenhum view. Você deverá substituir:

```markup
<span class="mr-2 d-none d-lg-inline text-gray-600 small">
    {{ usuario_logado }}
</span>
```

Por:

```markup
<span class="mr-2 d-none d-lg-inline text-gray-600 small">
    {{ request.user.email }}
</span>
```

Além disso, vamos remover da view `index` a variável `usuario_logado` que criamos no contexto. O código:

```python
context = {	 
    "nome_pagina": "Página inicial",
    "usuario_logado": request.user,	
    "visitantes": visitantes,
}
```

Ficará assim:

```python
context = {	 
    "nome_pagina": "Página inicial",
    "visitantes": visitantes,
}
```

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

