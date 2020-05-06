# Tópicos da apostila

## Capítulo 01

* Conhecendo o instrutor e sabendo mais sobre o curso
  * Sobre o instrutor
  * Sobre o curso
  * O que iremos construir?
* A linguagem de programação Python
* O que é Django Framework?
* Instalando e configurando o ambiente para iniciar o projeto
  * Python 3.8
  * Virtualenv
  * Pip
  * Django framework

## Capítulo 02

* Iniciando seu primeiro projeto Django
  * Um pouco mais sobre o django-admin
* Entendendo a estrutura do projeto
* Criando nosso primeiro aplicativo Django
* Criando nosso "Hello World"
* Escrevendo as models
  * Escrevendo a classe modelo
  * Escrevendo um manager personalizado
  * Alterando o modelo padrão nas configurações
  * Criando as tabelas do nosso banco de dados
* Criando um super usuário
* Conhecendo o Django Admin
* Alterações necessárias nas configurações



## Capítulo 03

* Criando aplicativo para gerenciar porteiros
* Escrevendo as models do nosso aplicativo de porteiros
  * Conhecendo o campo DateField
  * Conhecendo o campo OneToOneField
* Registrando nossa aplicação no Admin do Django
* Aplicando as alterações em nosso banco de dados
* Criando porteiro através do Admin do Django

## Capítulo 04

* Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML
  * Criando a pasta templates em nosso projeto
  * Criando a pasta static em nosso projeto
* Criando views que renderizam templates
  * Conhecendo a função render
* Entendendo as adaptações necessárias no template
  * Conhecendo a tag static
  * Alterando o caminho dos arquivos estáticos
    * Alterando as importações dos arquivos CSS
    * Alterando as importações dos arquivos JS
* Exibindo variáveis no template
  * Definindo nosso dicionário de contexto
  * Exibindo as informações nos templates

## Capítulo 05

* Criando o aplicativo para gerenciar visitantes
* Escrevendo as models do nosso aplicativo de visitantes
  * Conhecendo o campo DateTimeField
  * Conhecendo o campo ForeignKey
* Registrando nossa aplicação no Admin do Django
* Aplicando as alterações em nosso banco de dados
* Adicionando visitante utilizando o Django Admin
* Listando visitantes na página inicial da dashboard
  * Buscando registros de visitantes no banco de dados
  * Listando registros de visitantes no template HTML
    * Conhecendo a tag for e acessando atributos do visitante

## Capítulo 06

* Criando tela para registro de novo visitante
* Criando view para registrar visitante
  * Criando URL para mapear view
* Adaptando nossos templates para trabalhar com a template engine do Django
  * Criando o template base
  * Adaptando template index
  * Adaptando template base
  * Adaptando template registrar\_visitante

## Capítulo 07 - Parte 1

* Trabalhando com formulários no Django
* Criando nosso formulário
* Renderizando nosso formulário automaticamente
* Melhorando a exibição do nosso formulário
* Estilizando nosso formulário com django-widget-tweaks
  * Como instalar
  * Importando no template
  * Utilizando o render\_field

## Capítulo 07 - Parte 2

* Preparando view para receber requisição do tipo POST
  * Conhecendo o objeto request
* Conhecendo um pouco mais dos formulários
  * Tratando problema com atributo nulo
* Exibindo uma mensagem para o usuário ao cadastrar novo visitante
  * Conhecendo o Django messages
  * Alterando o template para exigir as mensagens
* Tratando possíveis erros em nosso formulário
* Deixando nossas mensagens de erro mais claras

## Capítulo 08

* Criando tela para exibir informações de visitante
* Criando a view
  * Conhecendo o atalho get\_model\_or\_404
* Criando URL para acessar informações de visitante
* Criando template para exibir informações de visitante
* Criando métodos personalizados para exibir informações do Visitante
  * Criando método para exibir horário de saída
  * Criando métodos para exibir horário de autorização de entrada e morador responsável por autorizar a entrada
  * Criando método para exibir placa do veículo utilizado na visita
  * Utilizando métodos personalizados no template
* Utilizando o Django para renderizar nossas URLs

## Módulo 10

* [x] Criando função para finalizar visita
  * [x] Escrevendo view para finalizar visita 
  * [x] Criando URL
* [x] Alterando template para enviar uma requisição do tipo POST ao confirmar encerramento da visita
* [x] Prevenindo erros e operações desnecessárias
  * [x] Exibição condicional de botões para autorizar entrada e finalizar visita

## Módulo 11

* [x] Implementando melhorias em nossos templates
  * [x] Exibindo botão com função de "voltar" e "cancelar" em páginas de informações e registro de visitante 
    * [x] Explicar que utilizaremos apenas um link para a página inicial da dashboard devido ao fato de que a única maneira de chegar até as duas páginas é pela página inicial da dashboard
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



