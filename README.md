# PASSCOM: Venda Compartilhada de Passagens

## Introdução
Este projeto aborda o desenvolvimento de um sistema de venda de passagens aéreas interconectado entre diferentes companhias. Cada companhia possui um servidor próprio para consultas e reservas de trechos de voos, com a possibilidade de compartilhamento de rotas com companhias conveniadas. O desafio central foi implementar uma solução que permitisse ao cliente, a partir de qualquer servidor, reservar passagens em outras companhias, preservando a ordem e preferência dos trechos selecionados, além de garantir uma estrutura descentralizada, onde a queda de um servidor não comprometa o funcionamento dos demais.

A aplicação foi desenvolvida utilizando o FastAPI para criação de APIs REST. Para gerenciar a concorrência e a ordenação de requisições entre diferentes servidores, foi empregado um relógio lógico, que facilita a sincronização e evita conflitos ao tratar as reservas de múltiplos clientes.

Os principais requisitos atendidos incluem a descentralização dos servidores e o bloqueio de trechos reservados pelo primeiro cliente que iniciar a compra. Contudo, a aplicação ainda apresenta limitações no que se refere à confiabilidade e consistência do sistema, aspectos que discutiremos na seção de resultados.

## Metodologia Utilizada
A comunicação entre as APIs segue a arquitetura REST, permitindo que consultas de disponibilidade e requisições de reserva sejam processadas de maneira descentralizada e isolada. Cada trecho de voo é representado por uma entrada em um arquivo JSON, e que é atualizado conforme as reservas são efetuadas.

O processo de reserva de passagens utiliza o relógio lógico para organizar as requisições e evitar conflitos. Cada servidor mantém uma ordem de preferência com base nas requisições recebidas, e, quando uma requisição de reserva é iniciada, a passagem é temporariamente bloqueada. Dessa forma, o sistema oferece concorrência controlada para os trechos de voos disponíveis, ainda que atualmente essa funcionalidade só seja garantida entre servidores ativos.

 - Implementação dos Testes: Os testes da aplicação foram realizados de forma simples, utilizando o Swagger UI para validação das principais funcionalidades e endpoints. Não foram implementados testes consistentes para garantir o comportamento esperado sob condições de alta concorrência ou falhas de servidores.

## Discussão e Resultados
Durante a execução, o sistema atendeu parcialmente aos requisitos de descentralização e concorrência. O sistema implementa o compartilhamento de trechos entre companhias, garantindo que, em caso de queda de um servidor, as demais companhias não são diretamente impactadas, mantendo a operabilidade para os clientes conectados nos servidores ativos. Entretanto, isso gera uma limitação em relação ao acesso aos dados do servidor que caiu, o que compromete a experiência do usuário em casos de falhas.

 - Consistência e Confiabilidade: O sistema não atende totalmente os requisitos de confiabilidade. Atualmente, uma compra em andamento não é recuperada se o servidor onde o cliente iniciou a requisição cair e reiniciar. Não há um mecanismo que armazene o estado da transação de forma persistente para recuperação posterior. Isso representa uma limitação relevante em termos de experiência do cliente e continuidade das transações.

 - Concorrência e Preferência: A preferência dos trechos é garantida apenas entre os servidores online, não sendo assegurada entre servidores desconectados. Este aspecto representa uma fragilidade em cenários de queda de servidores, onde clientes que ainda estão realizando a compra podem encontrar dificuldades para completar a seleção dos trechos desejados.

## Conclusão
O projeto de venda de passagens entre diferentes servidores de companhias aéreas foi bem-sucedido em criar uma base descentralizada e uma experiência de reserva com concorrência parcialmente controlada. Entretanto, o sistema apresenta limitações de consistência e confiabilidade: falhas de um servidor em meio a uma transação interrompem a experiência do cliente, e o acesso aos dados de um servidor é perdido quando o mesmo cai.

Apesar dessas limitações, a aplicação demonstra uma solução viável para o problema de descentralização. O uso de um relógio lógico simplificado facilita a organização das requisições e oferece um modelo inicial para uma eventual expansão da funcionalidade de persistência das transações. 

## Referências
https://fastapi.tiangolo.com

https://docs.docker.com/reference/

https://github.com/camilafbarcellos/LamportClockSim

https://fastapidozero.dunossauro.com

https://www.youtube.com/watch?v=5ObmfcaugjA
