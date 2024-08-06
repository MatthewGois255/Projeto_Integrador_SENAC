**Essa documentação não entra em detalhes de como restaurar os dispositivos às configurações de fábrica**

A sequência dos comandos não importa tanto, com excessão de alguns. Presei por seguir uma sequência lógica que favorecesse a explicação do passo a passo e ao mesmo tempo fosse funcional

A configuração básica, das linhas e do SSH no Router são praticamente idênticas às do Switch. Portanto, não entrarei em detalhes nessa etapa (conferir documento 01-switch)

~~~
no
enable
$clock set ... ...
conf t
$$hostname ...
enable password ...
username ... privilege 15 secret ...
login block-for ... attemps ... within ...
service timestamps log datetime
service password-encryption
no ip domain-lookup
$$$line console 0
login local
logging synchronous
exec time-out ...
~~~