# Tópicos da apostila

## Módulo 1

* [x] Conhecendo o instrutor e sabendo mais sobre o curso
  * [x] Sobre o instrutor
  * [x] Sobre o curso
* [x] A linguagem de programação Python
* [x] O que é Django Framework?
* [x] Instalando e configurando o ambiente para iniciar o projeto
  * [x] Python 3.6
  * [x] Virtualenv
  * [ ] Pip \(dizer que ele já vem instalado ao instalarmos o virtualenv\)
  * [x] Django framework

## Módulo 2

* [x] Iniciando seu primeiro projeto Django
* [x] Entendendo a estrutura do projeto
* [x] Criando nosso primeiro aplicativo Django
* [x] Criando nosso "Hello World"
* [x] Escrevendo as models
  * [x] Escrevendo a classe modelo
  * [x] Escrevendo um manager personalizado
  * [x] Alterando o modelo padrão nas configurações
  * [x] Criando as tabelas do nosso banco de dados
* [x] Criando um super usuário
* [x] Conhecendo o Django Admin
* [x] Boas práticas e alterações necessárias nas configurações

## Módulo 3

* [x] Criando aplicativo para gerenciar porteiros
* [x] Escrevendo as models do nosso aplicativo de porteiros
  * [x] Conhecendo o campo DateField
  * [x] Conhecendo o campo OneToOneField
* [x] Registrando nossa aplicação no Admin do Django
* [x] Aplicando as alterações em nosso banco de dados
* [x] Criando porteiro através do Admin do Django

## Módulo 4

* [x] Configurando a aplicação para trabalhar com arquivos estáticos e pasta templates
* [x] Explicando as adaptações realizadas no template para funcionar no Django
  * [x] Alterar o caminho para arquivos estáticos
* [x] Atualizar função de home para usar o template
* [x] Passando variáveis para o template

## Módulo 5

* [x] Criando nosso aplicativo para gerenciar visitantes
* [x] Escrevendo as models do nosso aplicativo de visitantes
  * [x] Conhecendo o campo ForeignKey
  * [x] Conhecendo o campo DateTimeField
* [x] Aplicando as alterações nas models no banco de dados
* [x] Registrando nossa aplicação no Admin do Django
* [x] Registrando um visitante utilizando o Django Admin
* [x] Listando visitantes na página inicial da dashboard
  * [x] Buscando registros de visitantes no banco de dados
    * [x] Falar sobre Queryset API
  * [x] Listando registros de visitantes no HTML

## Módulo 6

* [ ] Criando tela para registro de visitante
* [ ] Adaptando o template para trabalhar com a template engine do Django
  * [ ] Criando o template "base.html"
  * [ ] Adaptando template "registrar\_visitante.html" para trabalhar com a engine do Django
* [ ] Criando view para registrar visitante
* [ ] Trabalhando com formulários no Django
  * [ ] Preparando view para receber requisição do tipo POST \(falar sobre "request", validações dos formulários do Django \(modelForm\), falar sobre redirect e csrf\_token\)
  * [ ] Criando intimidade com os formulários \(falar sobre atributo fields e alterar para se adequar às nossas necessidades\)
* [ ] Conhecendo um pouco mais dos formulários \(tratar problema de valor nulo para campo "autorizado\_por"\)
  * [ ] Falar sobre relacionamento entre FKs

## Módulo 7

* [ ] Melhorando a exibição do nosso formulário
* [ ] Estilizando nosso formulário com django-widget-tweaks
  * [ ] Como instalar \(inserindo nas configurações\)
  * [ ] Importando no template \(tags, etc\)
  * [ ] Utilizando o render\_field
* [ ] Exibindo uma mensagem para o usuário ao cadastrar novo visitante \([https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b](https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b)\)
  * [ ] Conhecendo o Django messages
  * [ ] Adicionando uma mensagem
  * [ ] Alterando o template para exibir as mensagens
* [ ] Tratando possíveis erros em nosso formulário
  * [ ] Alterando template para exibir mensagem de erro em formulário
* [ ] Deixando nossas mensagens de erro mais claras \(error\_messages\)

## Módulo 8

* [ ] Criando tela para exibir informações de visitante
  * [ ] Criando view
    * [ ] Recebendo token em view
    * [ ] Utilizando o get\_model\_or\_404
  * [ ] Criando template
  * [ ] Criando URL para acessar informações de visitante
    * [ ] Recebendo variáveis através da URL no Django
* [ ] Utilizando o Django para renderizar nossas URLs
* [ ] Aprendendo um pouco mais sobre URLs no Django
* [ ] Modelos do Django e seus métodos \([https://docs.djangoproject.com/en/2.2/ref/models/instances/\#django.db.models.Model.get\_FOO\_display](https://docs.djangoproject.com/en/2.2/ref/models/instances/#django.db.models.Model.get_FOO_display)\)
  * [ ] get\_status\_display\(\)
* [ ] Criando métodos personalizados para exibir informações do nosso modelo de Visitante
  * [ ] Exibindo atributos somente quando preenchidos
    * [ ] Criando método get\_horario\_autorizacao
    * [ ] Criando método get\_autorizado\_por
    * [ ] Criando método get\_horario\_saida
* [ ] Utilizando métodos para exibir informações no template

## Módulo 9

* [ ] Criando função para autorizar entrada de visitante
* [ ] Utilizando formulários para atualizar atributos específico
* [ ] Alterando template para exibir modal com formulário
* [ ] Alterando formulário HTML para se adequar às nossas necessidades
  * [ ] Alterar método
* [ ] Atualizando os campos horario\_autorizacao e status
  * [ ] Conhecendo o datetime do Python

## Módulo 10

* [ ] Criando função para finalizar visita
  * [ ] Escrevendo view para finalizar visita 
  * [ ] Criando URL
* [ ] Alterando template para enviar uma requisição do tipo POST ao confirmar encerramento da visita
* [ ] Prevenindo erros e operações desnecessárias
  * [ ] Exibição condicional de botões para autorizar entrada e finalizar visita

## Módulo 11

* [ ] Implementando melhorias em nossos templates
  * [ ] Exibindo botão com função de "voltar" e "cancelar" em páginas de informações e registro de visitante 
    * [ ] Explicar que utilizaremos apenas um link para a página inicial da dashboard devido ao fato de que a única maneira de chegar até as duas páginas é pela página inicial da dashboard
  * [ ] Melhorando a exibição do CPF do visitante
    * [ ] Criando método get\_cpf
    * [ ] Conhecendo o f-strings do Python
  * [ ] Exibindo informações do usuário logado diretamente no template
* [ ] Implementando melhorias na estrutura do nosso projeto
  * [ ] Criar pasta "apps"
* [ ] Criando métodos para organizar melhor nosso código
  * [ ] get\_absolute\_url, etc \(revisar\)

## Módulo 12

* [ ] Criando aplicativos para administrar informações da dashboard
  * [ ] Falar um pouco mais sobre a função dos apps django para realizar tarefas específicas, etc
* [ ] Migrando view "index" para aplicativo dashboard
* [ ] Buscando quantidade de visitantes para exibir na dashboard
  * [ ] Conhecendo o método count\(\)
  * [ ] Exibindo número de visitantes no template
* [ ] Conhecendo o método filter\(\)
  * [ ] Filtrando nossos visitantes por nome e CPF
* [ ] Filtrando nossos visitantes por status
  * [ ] Filtrando e contando visitantes em visita
  * [ ] Filtrando e contando visitantes aguardando autorização
  * [ ] Filtrando e contando visitas finalizadas

## Módulo 13

* [ ] Filtrando nossos visitantes por data
  * [ ] Conhecendo o field lookups da Queryset API \(horario\_chegada\_\_date\)
* [ ] Filtrando apenas os registros do mês atual
* [ ] Utilizando o datetime para descobrir o mês atual
* [ ] Ordenando nossa busca por data e hora

## Módulo 14

* [ ] Bloqueando o acesso para usuários não autenticados nas nossas views
* [ ] Utilizando o sistema de autenticação do Django para nos fornecer a view de login
  * [ ] Criando a URL
    * [ ] Utilizando o método as\_view\(\)
* [ ] Criando o template de login
  * [ ] Renderizando formulário de login
* [ ] Alterando a URL padrão para login e redirecionamento após login
* [ ] Adicionando mensagem de erro em formulário de login
* [ ] Criando URL para logout
* [ ] Criando template de logout
* [ ] Inserindo link para logout em dashboard

## Módulo 15

* [ ] Paginando listagem de visitantes 
  * [ ] Conhecendo a classe Paginator
* [ ] Alterando template para exibir resultados paginados \(10 em 10 itens\)
* [ ] Adicionando links no template para navegar nos resultados paginados

## Módulo 16

* [ ] Criando função para filtrar e buscar visitantes com django-filter
  * [ ] Como instalar \(inserindo nas configurações\)
* [ ] Criando nossos filtros
  * [ ] Criando o arquivo filters.py
* [ ] Utilizando o filtro na view
  * [ ] falar sobre ordem de operação e porque a queryset vem antes do filtro e depois dele vem a paginação\)
  * [ ] falar sobre request.GET \(somente lembrar do objeto request\)
  * [ ] Passando o resultado do filtro para a classe Paginator
* [ ] Testando o filtro passando parâmetros pela URL e acessando no navegador
* [ ] Criando interface para usuário realizar busca ou filtro na dashboard
  * [ ] Utilizando o django-widget-tweaks para renderizar nosso formulário
* [ ] Adicionando os campos "numero\_casa" e "status"

## Módulo 17

* [ ] Criando botão para "limpar filtro"
  * [ ] Identificando quando o usuário está solicitando os resultados filtrados
* [ ] Mantendo a query ao voltar da página de informações de visitante
  * [ ] Conhecendo um pouco mais o objeto request \(request.META.HTTP\_REFERER\)
* [ ] Melhorando nossos filtros
  * [ ] Tornando possível pesquisa por parte de nome



