# Estrutura do curso

Módulo 1

* [x] Conhecendo o instrutor e sabendo mais sobre o curso
* [x] A linguagem de programação Python
* [x] O que é Django Framework?
* [x] Instalando e configurando o ambiente para iniciar o projeto

## Módulo 2

* [x] Iniciando seu primeiro projeto Django
* [x] Entendendo a estrutura do projeto
* [x] Criando nosso primeiro aplicativo Django
* [x] Criando nosso "Hello World"
* [x] Escrevendo as models
* [x] Criando um super usuário
* [x] Conhecendo o Django Admin
* [x] Boas práticas e alterações necessárias nas configurações

## Módulo 3

* [x] Criando nosso aplicativo para gerenciar clientes
* [ ] Escrevendo as models do nosso aplicativo de Clientes
  * [ ] Conhecendo o campo UUIDField
* [ ] Registrando nossa aplicação no Admin do Django
* [ ] Aplicando as alterações nas models no banco de dados

## Módulo 4

* [ ] Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML
* [ ] Configurando a aplicação para trabalhar com a pasta templates
* [ ] Explicando as adaptações realizadas no template para funcionar no Django
* [ ] Atualizar função de home para usar o template
* [ ] Exibir funcionando
* [ ] Passando variáveis para o template

## Módulo 5

* [ ] Criando view para listar clientes
* [ ] Falar sobre o "objects" do model do Django
* [ ] Criando template para listar nossos clientes
* [ ] Adaptando o template para trabalhar com a template engine do Django
  * [ ] Criar template "base.html"
    * [ ] Explicar tag "block"
  * [ ] Criar template "index.html" utilizando a tag "block" e "extends" \(explicar tags\)
  * [ ] Adaptar template "listar\_clientes.html" para trabalhar com a engine do Django
  * [ ] Editar tabela com atributos e atualizar a página para listar o cliente criado no módulo 3
* [ ] Criando view para cadastrar um usuário
  * [ ] Renderizando nosso template
* [ ] Conhecendo o Django Forms
  * [ ] Renderizar form utilizar {{ form.as\_p }}
* [ ] O formulário atenda às nossas necessidades?
  * [ ] O token deve ser exibido?

## Módulo 6

* [ ] Alterando o formulário para atender às nossas necessidades
  * [ ] Conhecendo os campos disponíveis para a classe
  * [ ] Alterando os campos através do atributo "fields"
  * [ ] Atualizar a página para exibir as alterações
* [ ] Melhorando a exibição do nosso formulário
  * [ ] Utilizar os formulários do tema utilizado \(disponibilizar os HTML ou colocar na udemy os trechos\)
    * [ ] Alterar o método do formulário para POST
  * [ ] Renderizando nossos campos manulamente
  * [ ] Estilizando nosso formulário com django-widget-tweaks
    * [ ] Como instalar \(inserindo nas configurações\)
    * [ ] Importando no template \(tags, etc\)
    * [ ] Utilizando o add\_class
* [ ] Preparando nossa view para receber uma requisição do tipo POST
  * [ ] Falar sobre o objeto "request"
  * [ ] Falar sobre as validações dos formulários do Django \(modelForm\)
  * [ ] Falar sobre redirect
  * [ ] Falar sobre csrf\_token
* [ ] Exibindo uma mensagem para o usuário ao cadastrar novo cliente
  * [ ] Conhecendo o Django messages
  * [ ] Adicionando uma mensagem
* [ ] Alterando o template para exibir as mensagens
  * [ ] Adicionar o HTML do alert na página "base.html" e explicar que o Django messages é um middleware global, etc
* [ ] Adicionando botão de atalho para cadastro de cliente
* [ ] Adicionando função para botão "cancelar" de formulário de cadastro de cliente

## Módulo 7:

* [ ] Criando view para editar informações de cliente
  * [ ] Falar sobre passagem de parâmetros por URL
  * [ ] Conhecendo o get\_object\_or\_404
* [ ] Renderizando informações de um único objeto
* [ ] Criando URLs que recebem parâmetros
* [ ] Utilizando formulários para atualizar informações
  * [ ] Primeiro utilizar o formulário para renderizar as informações copiando o template que cria um novo cliente
  * [ ] Preparando a view para receber requisições do tipo POST
    * [ ] Falar sobre a passagem a instância no form e atualização de dados
* [ ] Atualizar informação, enviar requisição e falar sobre sucesso
* [ ] Atualizando tabela de clientes para exibir link para edição de cliente

## Módulo 8:

* [ ] Revisão do que foi criado até o momento
* [ ] Falar sobre o que foi estudado, o conteúdo passado e tudo mais. Revisar um pouco dos requisitos e, ao final, tentar criar um novo cliente com dados já existentes \(forçar o erro pra entrar no próximo tópico\)
* [ ] Tratando erros nos formulários
* [ ] Utilizar modo "primitivo" para avaliar os erros \(print\(form.errors\)\)
* [ ] Falar sobre o objeto "form.errors" \(imprimir na tela, falar sobre a estrutura ser HTML e renderizar no template do formulário\)
* [ ] Falar sobre as validações que foram escritas nas models e como o Django usa o conceito DRY e, por isso, replica as informações nos forms
* [ ] Melhorando a exibição dos erros
* [ ] Personalizando as mensagens de erro do formulário
* [ ] Falar sobre o objeto "error\_messages" para configurar as mensagens de erro
* [ ] Entendendo melhor como os formulários validam nossas informações
* [ ] Falar sobre as funções "clean" e "validate\_campo" \(sobrescrever as funções\)
* [ ] Criando validações personalizadas \(criar função para validar se o nome contém nome e sobrenome\)

## Módulo 9:

* [ ] Iniciando nosso aplicativo para gerenciar remessas
* [ ] Registrando nosso aplicativo nas configurações do projeto
* [ ] Definindo as models
* [ ] Conhecendo o campo ForeignKey
* [ ] Conhecendo o campo DateTimeField
* [ ] Escrevendo formulário para criação de remessa
* [ ] Definindo nossa view para listar remessas
* [ ] Criando o template
* [ ] Criando a URL
* [ ] Dividindo o "urlpatterns" pra manter uma organização
* [ ] Adicionando link para a lista de remessas ao nosso "base.html"

## Módulo 10:

* [ ] Definindo view para adicionar uma remessa
* [ ] Criando o template
* [ ] Definindo view para mostrar informações de encomenda a partir do token
* [ ] Recebendo um token na view
* [ ] Fazendo a busca utilizando token
* [ ] Explicar que não utilizaremos o formulário aqui pois a tela exibirá as informações somente, havendo outra tela que irá cadastrar as movimentações referentes à remessa
* [ ] Definindo o template \(definir template básico somente para exibir as informações\)
* [ ] Utilizando uuid como identificador \(falar sobre como boa prática devido a não utilização do identificar sequencial, etc\)
* [ ] Criando template para exibir informações de remessa
* [ ] Explicando o template \(primeira parte deverá exibir a última movimentação, e que a barra de progresso funcionará com \(18%, 57%, 78% e 100% de acordo com o status\), e a timeline\)
* [ ] Alterando template que lista encomendas para linkar com página de detalhes da encomenda

## Módulo 11:

* [ ] Iniciando nosso aplicativo para gerenciar movimentações em remessas
* [ ] Criando as models
* [ ] Definindo os forms
* [ ] Definindo as views
* [ ] Trabalhando nos templates
* [ ] Registrando nossas URLs
* [ ] Alterando o template "base.html" para ter link para movimentações de remessa

## Módulo 12:

* [ ] Revisar até agora
* [ ] Exibindo movimentações em tela de "informações de remessa"
* [ ] Conhecendo e utilizando o atributo "related\_name" nos campos do tipo ForeignKey
* [ ] Reforçar a necessidade de utilizar os comandos "makemigrations" e "migrate" ao realizar alterações nas models
* [ ] Listando movimentações referentes à remessa utilizando o related \(remessa.movimentacoes.all\(\)\)
* [ ] Alterando o template "informacoes\_remessa.html" para exibir as movimentações
* [ ] Utilizando o for para loop nas movimentações
* [ ] Explicar que o retorno é um iterável do tipo queryset
* [ ] Utilizando variáveis para exibir a informação desejada
* [ ] Utilizando if para renderizar template com base no tipo de movimentação
* [ ] Utilizar o IF para renderizar mensagens diferentes para cada tipo de movimentação
* [ ] Conhecendo o "for empty" para renderizar informações para quando não houver informações para exibir
* [ ] Utilizando método para exibir nome do label
* [ ] Utilizando o "order\_by" para ordenar queryset

## Módulo 13:

* [ ] Criando métodos personalizados para nossas models
* [ ] Criando função para gerar e preencher campo "código" automaticamente
* [ ] Criar função para gerar e preencher um código de, no máximo 13 dígitos, para identificar as encomendas
* [ ] Definindo formato do código \(13 dígitos, {3 letras \(retiradas do token\)}{8 números \(data atual\)}{número com 3 dígitos}\)
* [ ] Conhecendo o random do Python
* [ ] Utilizando random para gerar um número aleatório de 3 dígitos
* [ ] Conhecendo o datetime do Python
* [ ] Utilizando o datetime para pegar a data atual
* [ ] Sobrescrevendo o método "save"
* [ ] Dando entrada em nova remessa para testar se o código está sendo gerado automaticamente
* [ ] Criando método personalizado para exibir mensagem por tipo de movimentação

