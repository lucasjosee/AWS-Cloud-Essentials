#### Amazon Virtual Private Cloud(Amazon VPC)
A **Amazon VPC (Virtual Private Cloud)** é a sua **rede privada dentro da AWS**. É um pedaço isolado da nuvem onde seus recursos (instâncias EC2, bancos RDS, containers) vivem, e onde você decide quem entra, quem sai e quem conversa com quem.

A VPC, é criada em cada região do mundo assim que você cria uma conta na AWS. Também é criada uma sub-rede pública em cada AZ de cada região.
![[Pasted image 20261008192748.png]]

Tudo que está dentro da VPC consegue se comunicar livremente, independente se está em outra AZ ou se a sub-rede é publica ou privada.

Usos de um sub-rede dentro de uma VPC:
1. Pode ser usada para compartilhar recursos públicos
2. Pode ser usada para organizar seus recursos
3. Pode ser usada para isolar recursos e mantê-los privados