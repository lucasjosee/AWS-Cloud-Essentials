### Amazon EC2
Serviço de computação em nuvem da AWS, é através dele que podemos alugar poder computacional para diversos usos.

Essa locação, é feita através de instâncias que são maquinas virtuais hospedadas em  nuvem. Varias instâncias podem rodar em um mesmo data-center na AWS, e o poder computacional delas podem ser configuradas de acordo com a necessidade do cliente. O acesso a essas VMs podem ser feitos através por meio do Console Web da AWS, AWS CLI e SDKs.


### Tipos de Instâncias
![[Pasted image 20261006091954.png]]

### Precificação
![[Pasted image 20261006092111.png]]

### Escalando o EC2
**Escalabilidade x Elasticidade:**
	Escalabilidade refere-se a capacidade da operação designar mais recursos conforme a demanda aumenta. Isso pode acontecer aumentando o poder computacional de cada instância (vertical), ou designando mais instâncias para a operação (horizontal). Agora, uma operação ser elástica significa que ela vai se adaptar a demanda que possui ao longo do tempo.

A AWS cuida da escalabilidade usando o EC2 Auto Scaling, esse serviço usa outros serviços da própria AWS como o Amazon CloudWatch para monitorar as métricas necessárias. Ele oferece duas abordagens. O _escalonamento dinâmico_ se ajusta em tempo real às flutuações na demanda. O _escalonamento preditivo_ agenda preventivamente o número certo de instâncias com base na demanda prevista.


### Elastic Load Balancing
O serviço Elastic Load Balancing (ELB) opera distribuindo de forma automática o fluxo de tráfego de uma aplicação entre diversos recursos, como as instâncias EC2, com o objetivo de otimizar o desempenho e a confiabilidade. O balanceador de carga atua como um ponto centralizado de entrada para todo o tráfego web destinado a um grupo de Auto Scaling. À medida que a quantidade de instâncias EC2 varia em resposta à demanda, todas as requisições são inicialmente recebidas pelo balanceador de carga. A partir daí, o tráfego é distribuído uniformemente pelas instâncias disponíveis.

O ELB e o Auto Scaling são serviços independentes, mas para garantir uma escalabilidade adequada é recomendando o uso dos dois em conjunto.

##### Os tipos de load balancer

| Tipo                                | Camada         | Quando usar                                             |
| ----------------------------------- | -------------- | ------------------------------------------------------- |
| **Application Load Balancer (ALB)** | 7 (HTTP/HTTPS) | Sites, APIs, microsserviços. O mais usado.              |
| **Network Load Balancer (NLB)**     | 4 (TCP/UDP)    | Altíssimo desempenho, jogos online, IP fixo.            |
| **Gateway Load Balancer (GWLB)**    | 3 (IP)         | Passar tráfego por firewalls e appliances de segurança. |
| **Classic Load Balancer**           | 4 e 7          | Legado; não se usa em projetos novos.                   |


