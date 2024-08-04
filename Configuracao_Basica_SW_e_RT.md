Essa documentação não entra em detalhes de como restaurar os dispositivos às configurações de fábrica.

Nessa etapa das configurações básicas a sequência dos comandos não importa tanto. Eu presei por seguir uma sequência lógica que favorecesse a explicação do passo a passo.

<br>
<br>

No modo Privileged EXEC, configurar a hora e data em inglês
~~~
enable
clock set hh:mm:ss dia mês ano
~~~
Em seguida entrar no modo Global Configuration
~~~
  configure terminal
~~~
Dar nome ao Equipamento. No nosso caso, o nome de todos os Switches deveriam ser "sw-g" mais o número do grupo
~~~
  hostname ...
~~~
Requisitar senha para entrar no modo EXEC privilegiado.
~~~
  enable password 123@senac
~~~
Nosso projeto previa a criação de um usuário para cada integrante do grupo. A fim de economizar tempo, todos usuários que criamos possuem privilégio 15 que automaticamente entra no modo de configuração global. Caso não tenha esse parâmetro, o usuário criado adquiri privilégio 1.
~~~
  username ... privilege 15 secret ...
~~~
Importante notar que esses usuários não são usados para autentificação a menos que você especifique na configuração das linhas console e vty com o comando "login local".

<br>
<br>

No nosso projeto nós habilitamos o serviço de marcação de data e hora com milisegundos nos logs, mas é uma configuração opcional
~~~
  service timestamps log datetime msec
~~~
Bloquear temporariamente a conexão em caso de muitas tentativas malsucedidas dentro de um intervalo. Os parâmetros são respectivamente tempo bloqueado, tentativas erradas e intervalo máximo para errar tentativas
~~~
  login block-for 120 attemps 4 within 60
~~~
É comum digitar um comando no prompt errado e receber a mensagem de tradução de domínio. Esse comando desativa essa função.
~~~
  no ip domain-lookup
~~~


As senhas *password* não têm criptografia e ficam ficam disponíveis em *plain text* na *running-config*. Esse comando habilita a encriptação delas em md5 hash
~~~
  service password-encryption
~~~
<br>
<br>

Acessando as configurações da porta console
~~~
line console 0
~~~
No nosso projeto, tanto o usuário como a senha são pedidas para ter acesso à linha console
~~~
  login local
~~~
Mas se não quisessemos usar os usuários configurados no modo global, poderíamos criar uma senha apenas para acessar o console
~~~
  password 123@senac
  login
~~~
Impedir que mensagens do sistema interrompam na digitação
~~~
  logging synchronous
~~~
estabelecer um tempo máximo de inatividade na porta console. Os parâmetros são minutos e segundos. Configurar o tempo para 0 0 desabilita a função
~~~
  exec-timeout 5 30
~~~

<br>
<br>

Configurar as linhas virtuais para o acesso remoto. Os comandos são praticamente os mesmo, com excessão do tipo de protocolo
~~~
line vty 0 4
  login local
  logging synchronous
  exec-timeout 5 30
  transport input ssh
~~~

Finalmente volta ao modo EXEC privilegiado, salva e checa a *running-config*
~~~
  end
copy running-config startup-config
show running-config
~~~
