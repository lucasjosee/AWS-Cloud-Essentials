#### Amazon Virtual Private Cloud(Amazon VPC)
A **Amazon VPC (Virtual Private Cloud)** é a sua **rede privada dentro da AWS**. É um pedaço isolado da nuvem onde seus recursos (instâncias EC2, bancos RDS, containers) vivem, e onde você decide quem entra, quem sai e quem conversa com quem.

A VPC, é criada em cada região do mundo assim que você cria uma conta na AWS. Também é criada uma sub-rede pública em cada AZ de cada região.
![[Pasted image 20261008192748.png]]

Tudo que está dentro da VPC consegue se comunicar livremente, independente se está em outra AZ ou se a sub-rede é publica ou privada.

Usos de um sub-rede dentro de uma VPC:
1. Pode ser usada para compartilhar recursos públicos
2. Pode ser usada para organizar seus recursos
3. Pode ser usada para isolar recursos e mantê-los privados

#### Gateways de Internet
Para permitir que o tráfego público da internet acesse a VPC, é preciso associar um **gateway de internet** à VPC. Um gateway da internet é uma conexão entre uma VPC e a internet. Você pode pensar em um gateway da internet como uma porta que os clientes usam para entrar na cafeteria.

Para acessar recursos privados dentro da VPC, é necessário um gateway privado.

##### Maneiras de se conectar na nuvem AWS:

**AWS Client VPN**
	A solução AWS Client VPN é um serviço de rede que pode ser usado para conectar os profissionais remotos e redes on-premises na nuvem. Trata-se de um serviço VPN elástico e totalmente gerenciado que aumenta ou diminui a escala verticalmente de forma automática de acordo com a demanda do usuário.

**AWS Site-to-Site VPN**
	Algumas empresas talvez queiram estabelecer conexões seguras e criptografadas entre suas redes on-premises, como data centers ou filiais, e seus recursos na solução Amazon VPC. A VPN site a site cria uma conexão segura entre o data center ou as filiais e os recursos da nuvem AWS.

**AWS PrivateLink**
	Às vezes, outras empresas precisam da flexibilidade por meio da conexão de forma privada aos recursos de outros provedores de nuvem, como se estivessem em sua própria VPC. A solução AWS PrivateLink é uma tecnologia altamente disponível e dimensionável que pode ser usada para conectar a VPC de forma privada a serviços e recursos, como se eles estivessem na sua VPC.

**AWS Direct Connect**
	  A solução Direct Connect é um serviço que permite estabelecer uma conexão privada dedicada entre sua rede e a VPC na nuvem AWS. Reduz os custos de rede e aumenta a quantidade de largura de banda.

#### Tráfego de rede em uma VPC
Todos os dados que entram e saem de uma VPC são carregados em **pacotes**. Um pacote, é uma unidade de dados enviada pela internet ou por uma rede. Antes de entrar ele entra pelo gateway, e antes de entrar em uma sub-rede, varias verificações de segurança são feitas.

![[Pasted image 20261010001545.png]]
##### ACLs de Rede
Firewall virtual que controla o tráfego da sub-rede.

Cada conta AWS tem uma ACL de rede-padrão. Ao configurar a VPC, você pode usar a ACL de rede-padrão da conta ou criar ACLs de rede personalizadas. Em geral, a ACL de rede-padrão da conta permite todo o tráfego de entrada e saída, mas é possível modificá-la ao adicionar suas regras. São **stateless**, não se lembram de quem entrou e nem de quem saiu, por isso que fazem a verificação em ambos os sentidos.

##### Grupos de Segurança
Controla o tráfego de entrada a nível do recurso que está dentro na sub-rede.

Depois que um pacote entra em uma sub-rede, ele precisa ter as permissões avaliadas para os recursos dentro da sub-rede, como as instâncias da solução Amazon EC2. Um grupo de segurança é o componente da VPC que verifica as permissões de pacotes para uma instância da solução Amazon EC2. Trata-se de um firewall virtual que controla o tráfego de entrada e saída para recursos específicos da AWS, como instâncias da solução Amazon EC2. Por padrão, um grupo de segurança nega todo o tráfego de entrada e permite todo o tráfego de saída. É **stateful**, então lembra quem entra, por isso só faz a verificação na entrada.

PS: Tive um problema de grupo de segurança ao lançar uma instância, não estava na lista de permissões a conexão HTTP, tive que adicionar e só assim consegui me conectar.




