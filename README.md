## Cuadernos
##
# AmazonELB

Aquí,usamosAmazonElasticLoadBalancing(ELB)yAmazonCloudWatchatravésdela
CLIdeAWSparaequilibrarlacargadeunservidorweb.

```
Parte1:ELB
```
```
1.IniciasesiónenelsandboxdelcursoAWS.Vealdirectoriodondeguardaelarchivo
descriptdeinstalacióndeapachedellaboratoriodelaprácticacalificada3.Para
crearunbalanceadordecarga,hazlosiguiente.
awselbcreate-load-balancer--load-balancer-namesophia--listeners
"Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80"
--availability-zonesus-east-1a
```
```
¿CuáleselDNS_Namedelbalanceadordecarga?
