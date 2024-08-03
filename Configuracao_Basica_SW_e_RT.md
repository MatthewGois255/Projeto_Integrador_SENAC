Essa documentação não entra em detalhes de como restaurar os dispositivos às configurações de fábrica.

Nessa etapa das configurações básicas a sequência dos comandos não importa tanto. Eu presei por seguir uma sequência lógica que favorecesse a explicação do passo a passo.

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



As senhas *password* não têm criptografia e ficam ficam disponíveis em *plain text* na *running-config*. Esse comando habilita a encriptação delas em md5 hash
~~~
  service password-encryption
~~~





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
estabelecer um tempo máximo de inatividade na porta console. Configurar o tempo para 0 0 desabilita a função
~~~
  exec-timeout mm ss
~~~


.
.
.
