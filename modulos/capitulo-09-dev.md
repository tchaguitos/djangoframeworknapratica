# Capítulo 09 - dev

## Criando função para autorizar entrada de visitante

Nos capítulos anteriores, nos dedicamos à criação das funcionalidades para registro e visualização de informações de visitantes. Aprendemos um pouco sobre como requisições funcionam, aprendemos a utilizar os formulários do Django, adaptamos os templates e aprendemos um pouco mais como eles funcionam, conhecemos o django-widget-tweaks, o Django messages e o get\_object\_or\_404 e ainda criamos métodos personalizados para nosso modelo de visitantes. Olha só quanta coisa conseguimos absorver somente nesses últimos capítulos!

Com essas funcionalidades criadas, precisamos seguir o roteiro que criamos com base nas necessidades do cliente, de modo que, agora, trabalharemos na funcionalidade que autoriza a entrada do visitante no condomínio. Quando um visitante chega na portaria e se identifica, o mesmo deve aguardar que o colaborador responsável entre em contato com um morador da casa a ser visitada e autorize que este visitante adentre ao condomínio.

Ao receber a informação de que o visitante pode adentrar ao condomínio, o porteiro deverá registrar na dashboard qual o nome do morador que autorizou a entrada deste visitante. Para fins de controle, também é necessário que o sistema registre o horário em que essa autorização ocorreu. Sendo assim, no momento da autorização do visitante, devemos registrar o horário e o nome do morador responsável pela autorização.

## Criando um status diferente para cada estágio da visita

Antes de seguir, precisamos analisar o cenário e extrair algumas informações com base nos fluxos e eventos que ocorrem, afim de melhorar a experiência de utilização da dashboard. Existem três cenários para o visitante: quando ele chega na portaria e está aguardando autorização, quando ele está dentro do condomínio durante a visita e quando ele vai embora e finaliza a visita. 

## Criando formulário para atualizar atributos específico do visitante

## Alterando template para exibir modal com formulário

## Alterando formulário HTML para se adequar às nossas necessidades

* Alterando método do formulário HTML

## Atualizando os campos horario\_autorizacao e status diretamente

### Conhecendo o datetime do Python



