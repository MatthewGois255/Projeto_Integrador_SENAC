Essa documentação não entra em detalhes de como restaurar os dispositivos às configurações de fábrica.

No modo Privileged EXEC, configurar a hora e data em inglês:
~~~
enable
clock set hh:mm:ss dia mês ano
~~~
Em seguida entrar no modo Global Configuration:
~~~
  configure terminal
~~~
Dar nome ao Equipamento. No nosso caso, o nome de todos os Switches deveriam começar com "sw-g" mais o número do grupo.
~~~
  hostname ...
~~~
(**pesquisar sobre os tipos de senhas**)
