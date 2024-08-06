**Essa documentação não entra em detalhes de como restaurar os dispositivos às configurações de fábrica.**

Nessa etapa das configurações básicas a sequência dos comandos não importa tanto. Presei por seguir uma sequência lógica que favorecesse a explicação do passo a passo.

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
Importante notar que esses usuários não serão usados para autentificação a menos que se especifique na configuração das linhas console e vty com o comando "login local"

<br>

No nosso projeto habilitamos o serviço de marcação de data e hora com milisegundos nos logs, mas é uma configuração opcional
~~~
  service timestamps log datetime msec
~~~
Bloquear temporariamente a conexão em caso de muitas tentativas malsucedidas dentro de um intervalo. Os parâmetros são respectivamente *tempo bloqueado*, *tentativas erradas* e *intervalo máximo para errar tentativas*
~~~
  login block-for 120 attemps 4 within 60
~~~
É comum digitar um comando no prompt errado e receber a mensagem de tradução de domínio. Esse comando desativa essa função.
~~~
  no ip domain-lookup
~~~
As senhas *password* não têm criptografia e ficam disponíveis em *plain text* na *running-config*. Esse comando habilita a encriptação delas em md5 hash
~~~
  service password-encryption
~~~
<br>
<br>

### PORTA CONSOLE

Acessando a linha console
~~~
line console 0
~~~
No nosso projeto, tanto o usuário como a senha são pedidas para ter acesso à linha console
~~~
  login local
~~~
Caso não quisessemos usar os usuários configurados no modo global, poderíamos criar uma senha apenas para acessar o console com os seguintes comandos:
~~~
  password 123@senac
  login
~~~
<br>

Impedir que mensagens do sistema interrompam na digitação
~~~
  logging synchronous
~~~
Estabelecer um tempo máximo de inatividade na porta console. Os parâmetros são minutos e segundos. Configurar o tempo para 0 0 desabilita a função
~~~
  exec-timeout 5 30
  end
~~~

<br>
<br>

### SSH E ACESSO REMOTO

O nome de domínio é necessário para a criptografia do ssh
~~~
  ip domain-name ...
~~~
Gera pares de chaves rsa e habilitar a versão 2 do ssh
~~~
  crypto key generate rsa general-keys modulus 1024
  ip ssh version 2
~~~
Agora são outras configurações como limitar o tempo de inatividade (em segundos)
~~~
  ip ssh time-out 60
~~~
Número máximo de tentativas de conexão do SSH
~~~
  ip ssh authentication-retries 2
  end
~~~

Configurar as linhas virtuais para o acesso. Os comandos são praticamente os mesmos da linha console, com excessão do tipo de protocolo (que não queremos que seja Telnet)
~~~
line vty 0 4
  login local
  logging synchronous
  exec-timeout 5 30
  transport input ssh
  end
~~~

### VLANs E IPs

O protocolo SSH trabalha em cima do protocolo de Internet (IP). Pensando em segurança, só uma das VLANs (a que chamaremos de SVI) possui endereço IP para ser acessada e não será a VLAN 1 default. As demais vão ser configuradas para receber endereços IP do DHCP Server que vai ser implementado no Router

~~~
ip default-gateway ...
vlan "número da vlan de gerenciamento"
  name "nome da vlan de gerenciamento"
  ip address "ip" "subnet mask"
  no shutdown
~~~
Vai ser com o IP dessa VLAN que acessaremos o Switch remotamente

Perceba que o Gateway Padrão da VLAN de gerenciamento foi configurado no próprio Switch. Isso porque se trata de um Switch Layer 3 que toma decisões de roteamento e não precisa de roteador externo. Mesmo assim, não usaremos o SVI para o roteamento das outras Vlans.
<br>

Criando econfigurando as outras VLANs. Como tinhamos cinco pontos de rede no nosso projeto, criamos cinco dessas VLANs
~~~
vlan "número da vlan"
  name "nome da vlan"
  exit
interface "interface(s) da vlan"
  switchport mode access
  switchport access vlan "número da vlan"
  exit
~~~
Desativar as interfaces que não serão usadas. No nosso caso, as seguintes
~~~
interface f0/7 -23
  shutdown
  exit
~~~
Configuração do trunk com o Router. A inteface escolhida foi a 0/24
~~~
interface f0/24
  switchport trunk encapsulation dot1q
  switchport mode trunk
  exit
~~~
<br>
<br>

E aqui se encerra a configuração do Switch. Ficam faltando o Router e o Access Point. Para finalizar, passamos as configurações da RAM para a NVRAM e salvamos uma cópia para o cartão de memória Flash
~~~
copy running-config startup-config
copy startup-config flash:
~~~
