# Capítulo 11 - dev

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



### Exibindo informações do usuário logado diretamente no template

## Implementando melhorias na estrutura do nosso projeto

* Criar pasta "apps"

## Criando métodos para organizar melhor nosso código

* get\_absolute\_url, etc \(revisar\)

