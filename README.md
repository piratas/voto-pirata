# Decisoes distribuidas aka dd
Sistema para tomadas de decisao em grupos horizontais usando principios de democracia liquida e outros.

## Requisitos

* Anonimo
* Passivel de verificacao
* Liquida: Pode escolher no sim, nao, em alguem e pode nao escolher nada
* Tempo limitado para tomada de decisao.

### Requisitos desejados no futuro

* Conhecimento minimo - servidor com informacoes encriptadas.

## Partes

### Cliente

### Servidor

## Estudo de casos e os problemas

### Caso 1 - servidor e criptografia.

#### Preparacao
1. Um usuario pede pro cliente `dd` pra iniciar uma tomada de decisao sobre um assunto.
2. O app pede pro usuario entrar com informacoes sobre o que deve ser decidido:  
  a. O assunto e notas de informacao.  
  b. Quantas pessoas envolvidas, seus nomes e seus e-mails  
  c. Quantas pessoas precisam aprovar o processo de contagem das decisoes individuais.  
  d. Quando a decisao deve ser tomada.  

3. O App manda as informacoes pro servidor.
4. O servidor poe as informacoes relevante a decisao a ser tomada num banco.
7. O servidor gera um par de chaves GPG
9. Usa `shamir shared secret` para dividir a chave privada entre todos os participantes 
10. Servidor manda e-mail para os usuarios envolvidos na tomada de decisao: informacoes sobre o assunto a ser decidido, a chave publica GPG e as `shares` da chave privada.
11. Servidor deleta todas as chaves.

#### Tomando a decisao
1. Usuario recebe um e-mail com uma URL para entrar no App direto no assunto a ser decidido. Recebe tambem a chave publica do  e a `share`.
2. Usuarios tambem recebem a lista de nomes das outras pessoas involvidas nas decisoes.
3. Usuario decide em sim, nao ou em alguem. Usuario tambem nao precisa decidir.
4. App do usuario encripta a decisao com chave publica e o envia.
5. Usuarios podem mudar a decisao ateh o final do periodo maximo para tomada da decisao.

#### Contagem
Quando o tempo se esgota comeca a contagem
1. Servidor manda mensagem aos participantes para enviar suas `shares` como uma forma de concordar na contagem - no central authority.
2. Recebendo o numero minimo de `shares` o servidor recupera a chave GPG privada, decripta e calcula a decisao mais escolhida.
3. O servidor broadcast o resultado, mantem as decisoes individuais encriptadas e o resultado, mas deleta a chave privada e as decisoes decriptadas.
4. As decisoes podem ser recontadas.

#### Problemas e solucoes
1. Servidor sabe como as pessoas decidiram por pelo menos um instante  
  a. Se usar encriptacao homomorfica daria para nao precisar decriptar cada decisao.  
  b. Porem, nao conheco encriptacao homomorfica que funcione para decisao distribuida(explica aih!!!)  

2. Servidor age como centralizador de informacao - e se o servidor cair?  
  a. Pode usar blockchain tech para armazenar as decisoes... se o servidor cair, dah pra recuperar as decisoes da blockchain, a chave privada e revelar as decisoes na unha (mas... precisaria que encriptacao homomorfica funcionasse).
  
  
### Caso 2 - FLO blockchain e shamir

#### Preparacao
1. Um usuario pede pro cliente `dd` pra iniciar uma tomada de decisao sobre um assunto.
2. O app pede pro usuario entrar com informacoes sobre o assunto a ser decidido:  
  a. O assunto e notas de informacao.  
  b. Quantas pessoas envolvidas, seus nomes e seus e-mails  
  c. Quantas pessoas precisam aprovar o processo de contagem das decisoes individuais.  
  d. Quando a decisao deve ser tomada.

3. O App manda as informacoes pro servidor que poe as informacoes relevante a decisao a ser tomada num banco.
4. O servidor gera um endereco de FLO associado a cada pessoa decidindo e manda 0.1 FLO pra cada endereco.  
  a. A tabela de relacionamento entre enderecos FLO e pessoas decidindo tem o endereco FLO 
5. O servidor gera um endereco pra sim e outro pra nao com zero FLOs.
6. Servidor manda e-mail para os usuarios envolvidos na tomada de decisao: informacoes sobre o assunto a ser decidido e a lista de pessoas envolvidas na tomada de decisao.

#### Tomando a decisao
1. Usuario recebe um e-mail com uma URL para entrar no App direto no assunto a ser decidido.
2. Usuarios tambem recebem a lista de nomes das outras pessoas envolvidas na tomada de decisao.
3. Usuario decide em sim, nao ou em alguem. Usuario tambem nao precisa decidir.
4. O app pede pro servidor executar a transacao de FLO do seu endereco para onde deve ir a decisao.
5. O servidor verifica se o endereco de destino jah executou a transacao ou nao.  
  a. Se sim, verifica se o endereco de destino da transacao feita pelo endereco de destino original executou a transacao, repete.  
  b. Se nao, executa a transacao diretamente para o endereco de destino final.
  
>Alternativa: Servidor executa transacao para o endereco de destino e verifica se o endereco de destino jah executou alguma transacao.  
  a. Se sim, executa outra transacao do endereco original para o novo endereco de destino.  
  b. Se nao, para.

6. Servidor manda identificador da transacao como recibo de voto

#### Contagem

1. Conta-se o numero de FLOs nos enderecos de sim e nao
2. Todas as moedas distribuidas sao retornadas ao endereco do servidor.

#### Problemas:
1. Nao pode mudar de decisao.
2. Nao eh anonimo.
3. varios outros.
