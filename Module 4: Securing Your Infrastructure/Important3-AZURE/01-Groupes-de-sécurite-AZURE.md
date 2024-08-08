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



-----

# ANNEXE 01 


- Pour illustrer la différence entre un *Network Security Group* (NSG) et un *Application Security Group* (ASG) dans Azure, voici un schéma  :

```plaintext
          +-------------------+
          |    Azure VNet     |
          +-------------------+
                  |
          +-------------------+
          |     Subnet        |
          +-------------------+
                  |
          +-------------------+
          |  Network Security |
          |      Group (NSG)  |
          +-------------------+
                  |
          +-------------------+
          |   Application     |
          | Security Group    |
          |      (ASG)        |
          +-------------------+
             /         \
            /           \
   +----------------+  +----------------+
   |   VM in ASG    |  |   VM in ASG    |
   +----------------+  +----------------+
```

**Explications :**

- **NSG** : Les *Network Security Groups* sont utilisés pour filtrer le trafic réseau entrant et sortant au niveau des sous-réseaux ou des interfaces réseau. Ils permettent de définir des règles de sécurité basées sur des adresses IP source et destination, des ports, et des protocoles[1][2][3].

- **ASG** : Les *Application Security Groups* permettent de regrouper des machines virtuelles en fonction des applications auxquelles elles appartiennent. Cela simplifie la gestion des règles de sécurité du NSG en utilisant des groupes logiques au lieu d'adresses IP individuelles[1][2][3].

Dans le schéma ci-dessus, le NSG contrôle le trafic au niveau du sous-réseau et applique des règles de sécurité qui peuvent inclure des ASG comme sources ou destinations. Les ASG permettent de regrouper les machines virtuelles pour appliquer des politiques de sécurité cohérentes et simplifiées.


-----

# ANNEXE 02


- Schéma ASCII qui illustre la différence entre les **Network Security Groups (NSG)** et les **Application Security Groups (ASG)** dans Azure :

```
+------------------------------------------------------+
|                    Virtual Network (VNet)            |
|                                                      |
|  +--------------------+    +--------------------+    |
|  |     Subnet 1       |    |     Subnet 2       |    |
|  |                    |    |                    |    |
|  | +---------------+  |    | +---------------+  |    |
|  | | NIC 1         |  |    | | NIC 2         |  |    |
|  | |   (VM1)       |  |    | |   (VM2)       |  |    |
|  | +---------------+  |    | +---------------+  |    |
|  |        |            |    |        |            |   |
|  |        | NSG 1      |    |        | NSG 2      |   |
|  |        |            |    |        |            |   |
|  +--------|------------+    +--------|------------+   |
|           |                         |                |
|           +-------------------------+                |
|                     ASG 1                             |
|           (Groupe logique d'applications)             |
+------------------------------------------------------+
```

**Explication :**

- **NSG (Network Security Group)** : 
  - **Position** : Appliqué directement aux interfaces réseau (NIC) ou aux sous-réseaux.
  - **Fonction** : Contrôle le trafic entrant et sortant basé sur les adresses IP, les ports, et les protocoles. NSG 1 et NSG 2 ont des règles spécifiques à chaque interface réseau.

- **ASG (Application Security Group)** : 
  - **Position** : Groupes logiques auxquels les interfaces réseau (comme NIC 1 et NIC 2) peuvent être associées.
  - **Fonction** : Permet la gestion des règles de sécurité de manière plus granulée et logique, en fonction de l'application ou du rôle des VM, sans utiliser directement les adresses IP.

**Différence clé :**

- **NSG** filtre le trafic au niveau des interfaces réseau ou des sous-réseaux avec des règles de sécurité générales.
- **ASG** regroupe les interfaces réseau logiquement pour appliquer des règles de sécurité plus spécifiques et adaptées à des ensembles d'applications, facilitant ainsi la gestion à grande échelle.

# Annexe 3 : table comparative 

- Cette table détaille les différences entre les **Network Security Groups (NSG)** et les **Application Security Groups (ASG)** dans Azure, ainsi que des conseils sur quand les utiliser et quand ne pas les utiliser.

| **Critère**                              | **NSG (Network Security Group)**                                        | **ASG (Application Security Group)**                                        |
|------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------|
| **Fonction principale**                  | Filtrer le trafic réseau entrant et sortant basé sur des règles de sécurité. | Organiser les interfaces réseau de VM en groupes logiques pour une gestion simplifiée des règles de sécurité. |
| **Niveau d'application**                 | Sous-réseaux et interfaces réseau (NIC).                                | Groupes logiques de VM au sein d'un même VNet.                               |
| **Utilisation recommandée**              | Utiliser pour définir des règles de sécurité générales sur un sous-réseau ou une interface réseau spécifique. | Utiliser pour regrouper des VMs similaires et appliquer des règles spécifiques selon leur rôle ou application. |
| **Cas d'utilisation typiques**           | - Contrôle du trafic entrant/sortant dans un sous-réseau. <br> - Protection des ressources d'un VNet. | - Gestion de la sécurité dans des environnements d'applications complexes.<br> - Simplification de la gestion des règles de sécurité pour des applications avec plusieurs VMs. |
| **Avantages**                            | - Simplicité dans la gestion des règles de sécurité au niveau réseau.<br> - Flexibilité pour contrôler le trafic à différents niveaux (sous-réseau/NIC). | - Réduction de la complexité en évitant de gérer des adresses IP spécifiques.<br> - Permet des politiques de sécurité basées sur des rôles/applications. |
| **Limites**                              | - Peut devenir complexe si appliqué à la fois au niveau sous-réseau et NIC (risque de conflits de règles).<br> - Gère les règles de sécurité de manière plus rigide (par adresse IP). | - Limité au regroupement logique dans un même VNet.<br> - Ne remplace pas les NSG mais fonctionne en complément. |
| **Quand l'utiliser**                     | - Lorsque vous devez contrôler le trafic à un niveau plus global.<br> - Pour des environnements simples où chaque ressource peut être protégée par un ensemble de règles définies. | - Lorsqu'il est nécessaire de gérer de manière flexible et logique les règles de sécurité pour des groupes de VMs.<br> - Pour des environnements complexes avec de nombreuses applications. |
| **Quand ne pas l'utiliser**              | - Si vous avez besoin de gérer des règles de sécurité très spécifiques pour des groupes d'applications (utilisez ASG dans ce cas).<br> - Si l'environnement est complexe avec des rôles/applications multiples. | - Lorsque vous avez besoin de contrôler directement le trafic à un niveau global ou pour des ressources spécifiques (utilisez NSG dans ce cas).<br> - Si les VMs sont réparties sur plusieurs VNets. |
| **Exemple de scénario**                  | - Contrôle du trafic pour l'ensemble d'un sous-réseau dans un VNet.<br> - Protection d'une interface réseau connectée à une application web. | - Groupement de VMs qui font partie d'une même application pour appliquer des règles spécifiques.<br> - Gestion simplifiée des règles de sécurité pour une application multi-tier. |

Les NSG sont principalement utilisés pour appliquer des politiques de sécurité au niveau du réseau, tandis que les ASG permettent de gérer la sécurité entre différents tiers d'applications en regroupant les machines virtuelles selon leur rôle ou leur fonction[1][2][3].





# Citations:

[1] https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview

[2] https://learn.microsoft.com/fr-fr/azure/virtual-network/application-security-groups

[3] https://learn.microsoft.com/en-us/azure/virtual-network/application-security-groups

[4] https://learn.microsoft.com/en-us/azure/virtual-network/network-security-group-how-it-works

[5] https://blog.cellenza.com/cloud-2/azure-basics-les-network-security-groups/

[6] https://www.geeksforgeeks.org/azure-network-security-groups/

[7] https://aviatrix.com/learn-center/cloud-security/create-network-security-groups-in-azure/

[8] https://www.cloudbolt.io/azure-costs/azure-nsg/

