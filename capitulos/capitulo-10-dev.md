# Capítulo 10 - dev

## Criando função para finalizar visita

Agora que criamos a função para autorizar a entrada do visitante, precisamos também criar a função que finaliza a visita. Para a primeira, utilizamos o formulário `AutorizaVisitanteForm` para atualizar o nome do morador responsável e definimos manualmente o status e o horário de autorização.

Desta vez, precisamos atualizar apenas o valor do atributo `horario_saida` e alterar o status para `FINALIZADO`, que é o status para quando o visitante deixa o condomínio. Sendo assim, vamos criar uma outra view que será responsável por receber um `id`, buscar um visitante com este `id` e atualizar essas informações. A view será um pouco parecida com a view `informacoes_visitante`. 

Abaixo da função `informacoes_visitante` crie e função `finalizar_visita`:

```python
# código acima omitido

def finalizar_visita(request, id):

    if request.method == "POST":
        visitante = get_object_or_404(Visitante, id=id)

        visitante.status = "FINALIZADO"
        visitante.horario_saida = datetime.now()

        visitante.save()

        messages.success(
            request,
            "Visita finalizada com sucesso"
        )

        return redirect("index")
```

A função `finalizar_visita` deverá receber um `id` como argumento e utilizar a função `get_object_or_404` para buscar o visitante do `id` em questão. Após isso vamos atualizar os atributos `status` e `horario_saida` diretamente e salvar o visitante. A diferença aqui é que não utilizaremos um formulário e nossa view será acessada somente através do método `POST`. Todo o resto continuará bem parecido com a view já feita anteriormente.

Para garantir que as operações serão realizadas somente quando o método `POST` for utlizado, vamos escrever um `if` e utilizá-lo certificar essa informação \(`if request.method == "POST":`\) e, caso seja, vamos executar as operações necessárias.

Ao contrário das outras funções que escrever, ela  



## Criando URL

## Alterando template para enviar uma requisição do tipo POST ao confirmar encerramento da visita

## Prevenindo erros e operações desnecessárias

* Exibição condicional de botões para autorizar entrada e finalizar visita

