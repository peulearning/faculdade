#Conceitos

Domínio de Colisão 
Protocolo CSMA/CD -> Ethernet 
Protocolo CSMA/CA -> Wi-fi


Switch 


## Comandos Utilizados até aqui !

-  *Topologia da rede lab.conf :*
```
PC1[0]=A e por ai vai ... ( Desenhe para Facilitar )
```
* *Iniciar o Kathara* :
```
kathara lstart
``` 

* *Finalizar o Kathara:* 
```
kathara wipe
``` 

- *Configurar um link*:  

```
ifconfig eth0 endereço ip 
```

* *Configurar roteador:* colocar as configurações dos links : 
``` 
route add -net 192.168.20.0/24 gw 10.0.0.2
route add default gw 50.0.0.0 
```

* *Configurar Switch:* 
```
brctl addbr br0
brctl addif br0 eth1 eth2 
ifconfig br0 up
```



