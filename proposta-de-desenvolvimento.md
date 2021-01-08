# Proposta de desenvolvimento

O condomínio Montanhas Azuis é um conceituado empreendimento que oferece conforto, qualidade e segurança para seus moradores. O condomínio nos procurou com o intuito de melhorar a experiência de trabalho de seus porteiros e tornar informatizado o processo de registro e administração de visitantes, de modo que os porteiros tenham acesso a uma página web e essas informações possam ser acessadas de qualquer lugar. Além do mais, desta forma, é possível garantir a segurança e a confiabilidade dos registros.

## Entendendo o processo

Quando um visitante chega à portaria do condomínio, o mesmo deve informar alguns dados pessoais para que o porteiro anote estas informações no caderno de visitantes. Feito isso, o porteiro deve comunicar um morador que esteja na casa no momento e que possa autorizar a entrada deste visitante. Com a autorização concedida, o porteiro deve anotar o nome do morador e o horário em que a autorização ocorreu. Somente após esta etapa que o visitante pode adentrar ao condomínio e iniciar sua visita. Ao final da visita, o porteiro deve registrar o horário em que o visitante saiu das dependências do condomínio e ainda escrever seu nome para registrar que ele é o responsável pelo registro.

Todo esse processo, atualmente, ocorre de forma manual e através de um caderno que é destinado ao registro de visitantes. Devido a isso, o processo acaba levando mais tempo que deveria e as informações podem ter sua confiabilidade questionada e segurança ameaçada, pois problemas podem acontecer com o caderno ou as folhas deste caderno e até mesmo rasuras podem acontecer e prejudicar as informações ali contidas. Além dos pontos levantados, com um sistema web podemos automatizar algumas funções para poupar tempo e reduzir as informações que devem ser preenchidas pelo porteiro.

Ter um sistema web para registro e administração de visitantes é um projeto importante para o condomínio pois será um diferencial frente aos seus concorrentes e ainda vai melhorar bastante a experiência de trabalho dos porteiros que, com o sistema em funcionamento, terão menos informações para preencher e, consequentemente, menos tempo será perdido com as atividades relacionadas ao registro de visitantes.

## Levantamento de requisitos

Com todo esse cenário em mente, precisamos desenvolver uma página web que seja acessível por meio das credenciais **e-mail** e **senha** e que possibilite o **registro dos visitantes**, conforme é feito atualmente no caderno. Para cadastrar um visitante é necessário informar seu **nome completo**, **CPF**, **data de nascimento**, o **número da casa a ser visitada,** a **placa do veículo do visitante**, se houver, e ainda o **horário em que o visitante se apresentou à portaria**.

Após o registro, o porteiro deve entrar em contato com um morador que esteja na casa no momento e informar à respeito da visita para que o morador autorize a entrada do visitante. Com a autorização concedida, o porteiro deve colocar no caderno o **nome do morador que autorizou a entrada do visitante** e o **horário em que essa autorização ocorreu**. Ao final da visita, o **horário em que o visitante deixou as dependências do condomínio** deve ser registrado junto do **nome do porteiro responsável pelo registro**. O fluxo e as etapas de todo o processo são as seguintes:

![Imagem com etapas do processo de registro de visitantes, onde temos quatro etapas: chegada na portaria, aguardando autoriza&#xE7;&#xE3;o, realizando visita e visita finalizada](.gitbook/assets/processo-registro.png)

A divisão do processo por etapas faz com que fiquem mais claras as atividades e a ordem em que elas devem ocorrer. Enxergando desta forma, podemos pensar em formas de dividir o preenchimento das informações e a automatização de determinadas informações.

Para poupar tempo do porteiro, ao receber as primeiras informações para o registro, seria interessante que o horário de chegada fosse preenchido automaticamente pois, desta forma, o porteiro fica preocupado somente em receber as informações pessoais do visitante, a casa a ser visitada e informar o placa do veículo, se houver.

Outro ponto interessante seria o registro automático do horário de autorização, assim que o porteiro informar o nome do morador responsável por autorizar a entrada do visitante. Essa informação pode ser preenchida num segundo momento, pois o visitante ficará na recepção aguardando o contato do porteiro com um morador que autorize a sua entrada. Preenchendo o nome do morador, vamos atualizar essa informação junto do horário de autorização e a visita se inicia. Ao final, claro, quando o visitante retorna à portaria para finalizar sua visita, devemos atualizar o horário de saída e inserir o nome do porteiro responsável pelo registro, tudo de forma automática.

Tudo isso deverá ocorrer em uma interface web onde seja possível visualizar os últimos visitantes registrados, ver informações detalhadas de cada visitante e ainda autorizar e finalizar suas visitas. Além destas informações, é desejável que o sistema web exiba um resumo das informações como número de visitantes aguardando autorização, número de visitantes no condomínio, número de visitas finalizadas e ainda o número de registros no mês.

### Quais informações salvar

Nosso sistema deve estar preparado para armazenar uma série de informações, tanto dos porteiros, quanto dos visitantes. Essas informações são úteis para fins de controle e segurança de todos os envolvidos e também para o setor responsável pelos recursos humanos do condomínio.

#### Com relação aos porteiros, é necessário armazenar as seguintes informações para registro:

* E-mail \(Utilizado na criação do usuário para acesso ao sistema\)
* Nome completo
* CPF
* Telefone
* Data de nascimento

#### Com relação aos visitantes:

* Nome completo
* CPF
* Data de nascimento
* Casa a ser visitada
* Placa do veículo utilizado, se houver

Além das informações acima, ainda precisamos registrar as seguintes informações

* Horário de chegada na portaria
* Nome do morador responsável por autorizar a entrada do visitante
* Horário em que a autorização ocorreu
* Horário em que o visitante deixou as dependências do condomínio
* Porteiro responsável pelo registro



