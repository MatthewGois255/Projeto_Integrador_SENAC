**colocar as imagens nesse arquivo mesmo**

### DISPOSITIVOS DE REDE
Switch Cisco Catalyst Layer 3560 (img 001).

Router Cisco 2911 (img 002).

O nosso laboratório do SENAC tem três bancadas e mais de 20 pontos pra serem ligados no rack. Todos os cabos, antes de serem ligados em cada switch, são reunidos em dois Patch Panel (**modelo**) com 24 portas cada. As portas do Patch Panel correspondem aos pontos de rede nas bancadas.

O rack é (**modelo do rack**) (img 003) com (**tipo de rack**). O rack é compartilhado por todos os seis grupos em ordem crescente de cima para baixo.

Entre o segundo Patch Panel e o Switch do grupo 01 se encontra o Router 4321 (img 004) (e acho que o switch também) do professor, que faz o papel de Provedor de Internet (ISP).

Acess Point D-link Dir846

### CABEAMENTO
Se não houver nenhuma observação, as conexões citadas são por meio de Patch Cables UTP CAT**modelo do cabo**.

O primeiro acesso para configuração do Switch e do Router é feito via cabo console (rj45 - db9) pelo Putty. Vale resaltar que as nossas estações possuem conexão db9. Caso contrário, um cabo console USB poderia ser usado. Posteriormente o Telnet vai ser ativo no Switch e o cabo console não será mais necessário.

Os Router de todos os grupos estão conectados em *ring topology* por interface DCE/DTE.

### LOGÍSTICA
O projeto prevê cinco pontos para cada grupo. Especificamente, o meu grupo é o 02.

O endereço IP de todas as redes locais têm o prefixo 172.16.x.x e cada um dos cinco pontos será uma vlan. A número da vlan é pradronizadamente um número de dois dígitos, onde o dígito da dezena equivale ao dígito do grupo. Como serão seis vlan ao todo (O SVI do Switch será separado em uma vlan), os endereços IP da minha rede local serão:
~~~
172.16.20.x
172.16.21.x
172.16.22.x
172.16.23.x
172.16.24.x
172.16.25.x
~~~

Embora o Switch seja layer 3, usaremos o Router para interligar as redes de cada grupo e fazer DHCP. O servidor DHCP do Acess Point será desativado.

Para entender as Vlans, foram configuradas Subnets.

O Router está ligado ao Switch (F0/24 - G0/0). Para passar as Vlans para o Router, foram criadas Subnets na interface G0/0.