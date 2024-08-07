# **Groupes de Sécurité AZURE (NSG)**

Dans Azure, les groupes de sécurité jouent un rôle crucial dans la gestion et la sécurisation du trafic réseau. Il existe principalement deux types de groupes de sécurité : **les groupes de sécurité réseau (NSG)** et **les groupes de sécurité d'application (ASG)**. Voici une explication détaillée de chacun :

# 1- **Groupes de Sécurité Réseau (NSG)**

Les **Network Security Groups (NSG)** sont des objets de sécurité qui agissent comme des pare-feux distribués dans les environnements Azure. Ils permettent de filtrer le trafic réseau entrant et sortant vers et depuis les ressources Azure dans un réseau virtuel (VNet).

### **Fonctionnalités des NSG**
- **Règles de Sécurité** : Un NSG contient des règles de sécurité qui spécifient si le trafic réseau doit être autorisé ou refusé en fonction de critères tels que l'adresse IP source et destination, les ports et les protocoles. Chaque règle a une priorité, et les règles sont évaluées dans l'ordre de priorité (plus le nombre est bas, plus la priorité est haute) jusqu'à ce qu'une règle soit appliquée[1][4].
- **Règles par Défaut** : Azure crée des règles de sécurité par défaut dans chaque NSG pour permettre la communication au sein du réseau virtuel et avec le load balancer Azure, tout en bloquant le trafic entrant non sollicité depuis Internet[1].
- **Association** : Les NSG peuvent être associés à des sous-réseaux ou à des interfaces réseau (NIC) de machines virtuelles. Lorsqu'un NSG est associé à un sous-réseau, les règles s'appliquent à toutes les interfaces réseau dans ce sous-réseau. Il est recommandé de ne pas associer un NSG à la fois à un sous-réseau et à des interfaces réseau individuelles pour éviter des conflits de règles[4][7].

### **Exemple de Règles de Sécurité**
- **AllowVNetInBound** : Permet tout le trafic entrant au sein du même réseau virtuel.
- **DenyAllInbound** : Bloque tout le trafic entrant de l'extérieur du réseau virtuel.

# 2 - **Groupes de Sécurité d'Application (ASG)**

Les **Application Security Groups (ASG)** permettent de configurer la sécurité réseau comme une extension naturelle de la structure de l'application. Ils facilitent la gestion de la sécurité en regroupant des machines virtuelles et en définissant des politiques de sécurité réseau basées sur ces groupes.

### **Fonctionnalités des ASG**
- **Regroupement Logique** : Les ASG permettent de regrouper des interfaces réseau de machines virtuelles en fonction de rôles ou de niveaux d'application, simplifiant ainsi la gestion des règles de sécurité sans avoir à gérer des adresses IP explicites[2][3].
- **Réutilisation des Politiques** : Les politiques de sécurité peuvent être réutilisées à grande échelle sans maintenance manuelle des adresses IP, car la plateforme gère la complexité des adresses IP et des ensembles de règles multiples[2].
- **Contraintes** : Toutes les interfaces réseau affectées à un ASG doivent se trouver dans le même réseau virtuel. De plus, les ASG ne peuvent pas être utilisés pour définir des règles de sécurité entre des interfaces réseau situées dans des réseaux virtuels différents[2].

### **Exemple de Scénario**
- **ASG Web** : Regroupe les interfaces réseau des serveurs web.
- **ASG Logic** : Regroupe les interfaces réseau des serveurs de logique métier.
- **ASG DB** : Regroupe les interfaces réseau des serveurs de base de données.

En résumé, les groupes de sécurité dans Azure, qu'ils soient des NSG ou des ASG, offrent des moyens puissants et flexibles pour gérer et sécuriser le trafic réseau au sein des environnements cloud. Les NSG sont principalement utilisés pour filtrer le trafic au niveau des sous-réseaux et des interfaces réseau, tandis que les ASG permettent une gestion plus granulaire et logique des politiques de sécurité basées sur les rôles applicatifs.

[1]: https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview
[2]: https://learn.microsoft.com/fr-fr/azure/virtual-network/application-security-groups
[4]: https://learn.microsoft.com/en-us/azure/virtual-network/network-security-group-how-it-works

# Citations:

[1] https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview

[2] https://learn.microsoft.com/fr-fr/azure/virtual-network/application-security-groups

[3] https://learn.microsoft.com/en-us/azure/virtual-network/application-security-groups

[4] https://learn.microsoft.com/en-us/azure/virtual-network/network-security-group-how-it-works

[5] https://blog.cellenza.com/cloud-2/azure-basics-les-network-security-groups/

[6] https://www.geeksforgeeks.org/azure-network-security-groups/

[7] https://aviatrix.com/learn-center/cloud-security/create-network-security-groups-in-azure/

[8] https://www.cloudbolt.io/azure-costs/azure-nsg/

