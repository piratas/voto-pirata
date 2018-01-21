# voto-pirata
Sistema de votacao eletronica usando democracia liquida

## Requisitos

* Anonimo
* Passivel de verificacao
* Liquida: Pode votar no sim, nao, em alguem e pode nao votar
* Tempo limitado para votacao

### Requisitos desejados no futuro

* Conhecimento minimo - servidor com informacoes encriptadas.

## Partes

### Cliente

### Servidor

## Estudo de casos e os problemas

### Caso 1 - servidor e criptografia.

#### Preparacao
1. Um usuario pede pro cliente `voto-pirata` pra iniciar uma votacao.
2. O app pede pro usuario entrar com informacoes sobre a votacao:  
  a. Nome da votacao e notas de informacao.  
  b. Quantas pessoas envolvidas, seus nomes e seus e-mails  
  c. Quantas pessoas precisam aprovar o processo de contagem dos votos.  
  d. Quando a votacao termina.  

3. O App manda as informacoes pro servidor.
4. O servidor poe as informacoes relevante a votacao num banco.
7. O servidor gera um par de chaves GPG
9. Usa `shamir shared secret` para dividir a chave privada entre todos os participantes 
10. Servidor manda e-mail para os usuarios envolvidos na votacao: informacoes sobre a votacao, a chave publica GPG e as `shares` da chave privada.
11. Servidor deleta todas as chaves.

#### Votacao
1. Usuario recebe um e-mail com uma URL para entrar no App direto na votacao. Recebe tambem a chave publica da votacao e a `share`.
2. Usuarios tambem recebem a lista de nomes dos outros votantes.
3. Usuario vota em sim, nao ou em alguem. Usuario tambem nao precisa votar.
4. App do usuario encripta o voto com chave publica e o envia.
5. Usuarios podem mudar o voto ateh o final do periodo de votacao.

#### Contagem
Quando o tempo se esgota comeca a contagem
1. Servidor manda mensagem aos participantes para enviar suas `shares` como uma forma de concordar na contagem - no central authority.
2. Recebendo o numero minimo de `shares` o servidor recupera a chave GPG privada, decripta e conta os votos.
3. O servidor broadcast o resultado, mantem os votos encriptados e o resultado, mas deleta a chave privada e os votos decriptados.
4. Votos podem ser recontados.

#### Problemas e solucoes
1. Servidor sabe como as pessoas votaram por pelo menos um instante  
  a. Se usar encriptacao homomorfica daria para nao precisar decriptar cada voto.  
  b. Porem, nao conheco encriptacao homomorfica que funcione para democracia liquida (explica aih!!!)  

2. Servidor age como centralizador de informacao - e se o servidor cair?  
  a. Pode usar blockchain tech para armazenar os votos... se o servidor cair, dah pra recuperar os votos da blockchain, a chave privada e revelar os votos na unha (mas... precisaria que encriptacao homomorfica funcionasse).
  
  
### Caso 2 - FLO blockchain e shamir

#### Preparacao
1. Um usuario pede pro cliente `voto-pirata` pra iniciar uma votacao.
2. O app pede pro usuario entrar com informacoes sobre a votacao:  
  a. Nome da votacao e notas de informacao.  
  b. Quantas pessoas envolvidas, seus nomes e seus e-mails  
  c. Quantas pessoas precisam aprovar o processo de contagem dos votos.  
  d. Quando a votacao termina.

3. O App manda as informacoes pro servidor que poe as informacoes relevante a votacao num banco.
4. O servidor gera um endereco de FLO associado a cada votante e manda 0.1 FLO pra cada endereco.  
  a. A tabela de relacionamento entre enderecos FLO e votante tem o endereco FLO 
5. O servidor gera um endereco pra sim e outro pra nao com zero FLOs.
6. Servidor manda e-mail para os usuarios envolvidos na votacao: informacoes sobre a votacao e a lista de votantes.

#### Votacao
1. Usuario recebe um e-mail com uma URL para entrar no App direto na votacao.
2. Usuarios tambem recebem a lista de nomes dos outros votantes.
3. Usuario vota em sim, nao ou em alguem. Usuario tambem nao precisa votar.
4. O app pede pro servidor executar a transacao de FLO do seu endereco para onde deve ir o voto.
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
1. Nao pode mudar de voto.
2. Nao eh anonimo.
3. varios outros.
