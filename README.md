# Desafio Node Chat

## Objetivo
Construir um sistema de servidor e clientes baseado em Web Sockets usando apenas os módulos nativos do Node, que permita troca de mensagens instantâneas com fan-out (ou broadcast), onde clientes conectados em uma mesma "sala" recebem todas as mensagens enviadas por qualquer um deles.

## Regras
- Não pode ver tutorial passo-a-passo
- Não pode pedir ajuda pra ninguém, é você e o VSCode e as docs
- Não pode usar nenhuma biblioteca, **exceto** bibliotecas para testes automatizados, caso queira
- Liberado para consulta:
  - Documentação oficial do Node
  - Stack Overflow

## Requisitos
- É preciso haver dois arquivos javascript, ao menos:
  - server.js
  - client.js
- Você pode incluir mais arquivos caso desejar
- Comportamento esperado de cada arquivo:
  - server.js, ao rodar com `node server.js`:
    - O servidor abre em uma porta qualquer (à sua escolha ao fazer o código, não precisa ser dinâmico), passando a aceitar conexões nessa porta
    - As conexões são sempre seguindo o protocolo WebSocket, e devem incluir uma "sala"
    - Caso a sala não exista, deve ser criada
    - Caso a sala exista, o client é adicionado à lista de clients daquela sala
    - Qualquer mensagem recebida em uma "sala" é entregue a todos os clients conectados na mesma sala
    - Clients conectados a uma sala não recebem mensagens de outras salas
    - O servidor gera um ID único para cada client, e é capaz de enviar mensagens por si só tanto para clients individualmente quanto para toda a sala, sem necessitar necessariamente que um client envie uma mensagem
    - Quando um client se conectar a uma sala, é preciso mostrar todo o histórico de mensagens já enviadas naquela sala antes da conexão, e é preciso avisar todos os clients da sala que um novo client se conectou
    - Considere que a sala dura somente enquanto o servidor está de pé (ou seja, pode ficar somente na memória), mas a sala deve permanecer aberta mesmo que não haja nenhum client conectado mais a ela, pois pode receber conexões de novos clients
    - Todas as mensagens enviadas e sempre que um client se conecta ou desconecta são mostradas no terminal do servidor
  - client.js, ao rodar com `node client.js nome-da-sala`, considerando que o servidor está aberto
    - Conecta-se a `nome-da-sala`, e mantém o terminal aberto
    - Qualquer mensagem digitada no terminal (ou seja, que vem através do `stdin`) é enviada ao servidor
    - Qualquer mensagem recebida do servidor é mostrada no terminal
- Espera-se que um client não mostre 2x a mensagem que acabou de enviar, você pode fazer o código que remove essa duplicação tanto no client quanto no server (ou em ambos), à sua escolha

## Dicas
- Você provavelmente vai usar o módulo `http` do Node
- Você pode querer usar o módulo `crypto` para gerar IDs únicos
- Normalmente é mais fácil fazer o servidor primeiro
- Deixe seu código organizado, com funções de escopo bem definido, sempre que terminar alguma "parte" (como abrir o servidor)

## Requisitos bônus
(fique à vontade para fazer quaisquer desses que quiser)
(PS: qualquer um desses só importa se todos os requisitos mínimos estiverem cumpridos)
- Testes
- O servidor persiste o histórico de mensagens mesmo quando é desligado
  - Pode ser no disco local (usando o módulo `fs` por exemplo)
    - Requisito bônus do bônus: as mensagens são gravadas em um arquivo de log "append-only" em modo "stream"
      - Ou seja, quando uma sala é aberta, o servidor abre uma conexão de append para um arquivo com o nome da sala, e
      - Adiciona as mensagens ao final do arquivo conforme chegam, e
      - Quando algum client novo se conecta, o servidor lê todas as mensagens (linhas) desse arquivo e envia para o client, sem manter as mensagens em memória
  - Caso queira persistir em um banco de dados, pode instalar uma biblioteca de driver desse banco que for usar
- Os clients podem escolher um nickname
- O client que abrir uma sala nova pode definir uma senha, e quaisquer novos clients devem informar a senha para entrar
