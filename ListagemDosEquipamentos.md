**colocar as imagens nesse arquivo mesmo**

### DISPOSITIVOS DE REDE
Switch Cisco Catalyst Layer 3560 (img 001).

Router Cisco 2911 (img 002).

O nosso laboratório do SENAC tem três bancadas e mais de 20 pontos pra serem ligados no rack. Todos os cabos, antes de serem ligados em cada switch, são reunidos em dois Patch Panel (**modelo**) com 24 portas cada. As portas do Patch Panel correspondem aos pontos de rede nas bancadas.

O projeto prevê cinco pontos para cada grupo. Especificadamente, o meu grupo é o 02 e ficamos com os pontos (**numerar os pontos**).

O rack é (**modelo do rack**) (img 003) com (**tipo de rack**). O rack é compartilhado por todos os seis grupos em ordem crescente de cima para baixo.

Entre o segundo Patch Panel e o Switch do grupo 01 se encontra o Router 4321 (img 004) (e acho que o switch também) do professor, que faz o papel de Provedor de Internet (ISP).

### CABEAMENTO
Se não houver nenhuma observação, as conexões citadas são por meio de Patch Cables UTP CAT**modelo do cabo**.

O primeiro acesso para configuração do Switch e do Router é feito via cabo console (rj45 - db9) pelo Putty. Vale resaltar que as nossas estações possuem conexão db9. Caso contrário, um cabo console USB poderia ser usado. O Router está ligado ao Switch (**porta do switch ligado ao router** - G0/0). Posteriormente o Telnet vai ser ativo no Switch e o cabo console não será mais necessário.

Os Router de todos os grupos estão conectados em *ring topology* por cabos seriais (img 005) (**especificar o tipo de cabo**).

O Access Point é um D-link Dir846 (**confirmar**).
