# Référence :

- https://medium.com/@phaubus/azure-nsgs-and-asgs-for-dummies-a9afcc5e2505#:~:text=Application%20Security%20Group%20(ASG)%2C,than%20individual%20VM%20IP%20addresses.
- https://www.corestack.io/azure-security-tools/azure-firewall-vs-nsg/

# Groupes de Sécurité Réseau (NSG) et Groupes de Sécurité d'Application (ASG) pour les Nuls

Lorsqu'il s'agit de la sécurité du cloud, Azure propose une gamme de services utiles couvrant différentes technologies. Concentrons-nous spécifiquement sur la sécurité réseau et certaines des options disponibles que vous avez probablement utilisées ou au moins vues : les groupes de sécurité réseau (NSG) et les groupes de sécurité d'application (ASG). Vous vous êtes peut-être demandé : "Quelle est la différence entre eux ?". Moi, oui :)

Parcourons cela ensemble…

## Qu'est-ce qu'un NSG ?
Les Groupes de Sécurité Réseau (NSG) d'Azure sont utilisés pour filtrer le trafic réseau entre les ressources Azure au sein d'un réseau virtuel. Un groupe de sécurité réseau contient des règles de sécurité qui permettent ou refusent le trafic réseau entrant vers, ou le trafic réseau sortant depuis, plusieurs types de ressources Azure. Pour chaque règle, vous pouvez spécifier la source et la destination, le port et le protocole.

Les NSG Azure contrôlent l'accès et gèrent la communication entre :
- Les charges de travail individuelles hébergées sur un ou plusieurs VNets Azure.
- La connectivité entre les environnements locaux et Azure via une passerelle d'application, une passerelle VPN, un pare-feu Azure, un service Azure Bastion et des appliances réseau virtuelles.
- Les connexions vers et depuis Internet.

Les NSG peuvent être assignés au niveau du sous-réseau ou de l'interface réseau. Cependant, il est important de planifier votre mise en œuvre et d'établir des lignes directrices claires. Si vous commencez à placer des NSG de manière aléatoire au niveau du sous-réseau et de l'interface réseau, vous risquez de vous retrouver dans une situation où certaines règles se chevauchent, ce qui peut entraîner un cauchemar de dépannage.

### Quelques Pratiques Recommandées pour l'Utilisation des NSG :
1. **Parfois, moins c'est mieux** : vous n'avez pas besoin de créer un NSG par sous-réseau ou par NIC. Groupez vos ressources ensemble et concevez votre réseau de manière à pouvoir créer moins de NSG qui peuvent être assignés à des ressources similaires.
2. **Ce qui vient en premier** : Pour le trafic entrant et sortant, un NSG appliqué à l'interface réseau (NIC) prend priorité sur un NSG appliqué au sous-réseau. Évitez d'assigner des NSG aux NICs à moins que ce ne soit la dernière option ; il est plus facile de contrôler les règles au niveau du sous-réseau.
3. **Moins c'est mieux** : l'ordre des règles NSG est très important, les règles NSG sont appliquées dans un ordre prioritaire entre 100 et 4 096, chaque nouvelle règle étant ajoutée de manière séquentielle. L'évaluation des règles commence par les numéros de priorité les plus bas.
4. **Un mauvais plan est mieux que pas de plan du tout** : planifiez votre déploiement à l'avance, analysez les sous-réseaux et les ports requis. Avec des fondations bien arrangées, il sera beaucoup plus facile de gérer vos NSG par la suite.
5. **Appelez un chat un chat** : assurez-vous de mettre en œuvre une convention de nommage uniforme et lisible pour vos règles afin qu'elles puissent être facilement identifiées.

### Journalisation des Flux NSG
Il est essentiel de surveiller, gérer et connaître votre propre réseau afin de pouvoir le protéger et l'optimiser. Vous devez connaître l'état actuel du réseau, qui se connecte et d'où les utilisateurs se connectent. Vous devez également savoir quels ports sont ouverts sur Internet, quel comportement réseau est attendu, quel comportement réseau est irrégulier, et quand des augmentations soudaines de trafic se produisent.

La journalisation des flux de groupes de sécurité réseau est une fonctionnalité d'Azure Network Watcher qui vous permet de consigner des informations sur le trafic IP traversant un groupe de sécurité réseau.

### Important : Encore une fois, planifiez à l'avance, les journaux de flux NSG peuvent générer une énorme quantité de données, ce qui peut entraîner une augmentation du coût de l'abonnement.

## Qu'est-ce qu'un ASG ?
Un Groupe de Sécurité d'Application (ASG) est une fonctionnalité de réseau qui vous permet de regrouper des machines virtuelles (VM) Azure en fonction de l'application à laquelle elles appartiennent. Les ASG permettent aux politiques de groupe de sécurité réseau (NSG) d'être définies en utilisant des regroupements logiques d'applications plutôt que des adresses IP individuelles de VM. Cela simplifie la gestion des règles de sécurité réseau, en particulier lorsque plusieurs VM appartiennent à la même application ou service. Les Groupes de Sécurité d'Application sont particulièrement utiles dans les architectures d'application complexes et distribuées où les VM doivent communiquer en toute sécurité à travers les frontières du réseau. Vous pouvez utiliser les ASG pour faciliter la maintenance des règles de sécurité réseau, améliorer la sécurité et simplifier le déploiement des applications dans Azure.

### Avantages des ASG
Les Groupes de Sécurité d'Application (ASG) offrent plusieurs avantages pour la gestion de la sécurité réseau dans Azure, y compris :
- **Politiques de Sécurité Cohérentes** : Avec les ASG, vous pouvez garantir que toutes les VM au sein d'un groupe suivent les mêmes politiques de sécurité.
- **Contrôle Granulaire** : Créez des politiques de sécurité réseau détaillées basées sur les charges de travail, les applications ou les environnements. Les ASG facilitent la segmentation du réseau pour contenir les violations.
- **Évolutivité** : Les ASG s'ajustent automatiquement pour s'adapter aux changements d'échelle des VM, éliminant le besoin de configuration supplémentaire.

### En Pratique
Testons certaines de ses pratiques. J'ai le déploiement suivant dans mon abonnement Azure.

- **G1 - Vert** : win-vm01, win-vm03, lin-vm01
- **G2 - Rose** : win-vm02, win-vm04
- **G3 - Rouge** : lin-vm02

Les VM sont situées dans des sous-réseaux désignés avec des NSG assignés et divisés à l'aide de ASG.

Chaque NSG contient des règles spécifiant la source et/ou la destination basées sur les ASG.

### Exemples de NSG :
- **WEB-NSG** : Ce NSG a 3 règles :
  - 900 : permettre la connexion entre les VM du groupe G1
  - 910 : permettre la connexion entre les VM du groupe G2
  - 930 : permettre les ICMP entrants depuis le groupe G3
- **APP-NSG** : Ce NSG a 3 règles :
  - 900 : permettre la connexion entre les VM du groupe G1
  - 910 : permettre la connexion entre les VM du groupe G2
  - 930 : permettre les ICMP entrants depuis le groupe G3
- **DATA-NSG** : Ce NSG a 2 règles :
  - 900 : permettre la connexion entre les VM du groupe G1
  - 930 : permettre les ICMP entrants depuis le groupe G3

### Test
Basé sur le diagramme ci-dessus et toutes les règles fournies, nous pouvons tirer quelques conclusions :
- Toutes les VM du groupe G1 peuvent communiquer avec les autres VM du groupe G1 mais ne peuvent pas communiquer avec les VM des groupes G2 ou G3.
- Toutes les VM du groupe G2 peuvent communiquer avec les autres VM du groupe G2 mais ne peuvent pas communiquer avec les VM des groupes G1 ou G3.
- Toutes les VM du groupe G3 peuvent communiquer avec les autres VM des groupes G1, G2 ou G3.

Vérifions si cela est vrai.

#### Test depuis win-vm-01 (G1)
Comme vous pouvez le voir, je peux utiliser ‘ping’ pour communiquer avec succès avec d'autres VM du groupe G1. Mais les VM des groupes G2 et G3 ne sont pas accessibles.

#### Test depuis win-vm-02 (G2)
Nous avons une situation similaire ici; je peux atteindre les VM du groupe G2 mais rien d'autre.

#### Test depuis lin-vm02 (G3)
La VM du groupe G3 peut communiquer avec tous les autres groupes.

### Conclusion
Vous ne verrez probablement pas l'exemple exact fourni ci-dessus dans le monde réel. Avec ce court laboratoire, j'ai essayé de mettre en évidence les capacités des ASG et comment vous pouvez les utiliser dans votre environnement.

L'implémentation des Groupes de Sécurité d'Application (ASG) Azure représente une avancée significative dans la simplification de la gestion de la sécurité réseau et l'amélioration des mécanismes de protection pour les applications hébergées sur Azure. En permettant le regroupement des machines virtuelles (VM) et d'autres ressources en fonction de leur structure fonctionnelle, sécuritaire ou applicative, les ASG facilitent une approche plus organisée et efficace des politiques de sécurité réseau. Cela permet l'application de contrôles granulaires adaptés aux besoins spécifiques de chaque groupe. De plus, les ASG contribuent à réduire la complexité des règles de sécurité réseau, minimisant ainsi le risque de mauvaises configurations.

---
# Annexe 01: Plus de détails + résumé

### Les Groupes de Sécurité Réseau (NSG) et les Groupes de Sécurité d'Application (ASG) pour les Débutants

Azure propose une gamme de services utiles pour la sécurité des réseaux. Parmi ces options, on trouve les groupes de sécurité réseau (NSG) et les groupes de sécurité d'application (ASG). Vous vous êtes peut-être demandé : "Quelle est la différence entre eux ?". Explorons cela ensemble.

#### Qu'est-ce qu'un NSG ?

Les Groupes de Sécurité Réseau (NSG) d'Azure sont utilisés pour filtrer le trafic réseau entre les ressources Azure au sein d'un réseau virtuel. Un groupe de sécurité réseau contient des règles de sécurité qui permettent ou interdisent le trafic réseau entrant ou sortant de plusieurs types de ressources Azure. Pour chaque règle, vous pouvez spécifier la source et la destination, le port et le protocole.

Les NSG d'Azure contrôlent l'accès et gèrent la communication entre :

- Les charges de travail individuelles hébergées sur un ou plusieurs réseaux virtuels Azure.
- La connectivité entre les environnements locaux et Azure via une passerelle d'application, une passerelle VPN, un pare-feu Azure, un service Azure Bastion et des appliances réseau virtuelles.
- Les connexions vers et depuis Internet.

Les NSG peuvent être attribués au niveau du sous-réseau ou de l'interface réseau, mais il est important de planifier votre mise en œuvre et d'établir des directives claires. Si vous commencez à placer des NSG de manière aléatoire, certaines règles peuvent se chevaucher, ce qui rendra le dépannage difficile.

**Quelques pratiques recommandées pour l'utilisation des NSG :**

- **Moins, c'est mieux** : Vous n'avez pas besoin de créer un NSG par sous-réseau ou par interface réseau. Groupez vos ressources, concevez correctement votre réseau pour créer moins de NSG qui peuvent être attribués à des ressources similaires.
- **Priorité** : Pour le trafic entrant et sortant, un NSG appliqué à l'interface réseau prend la priorité sur un NSG appliqué au sous-réseau. Évitez d'attribuer des NSG aux interfaces réseau sauf en dernier recours.
- **Ordre des règles** : L'ordre des règles NSG est très important, les règles NSG sont appliquées dans un ordre prioritaire entre 100 et 4096. Les nouvelles règles sont ajoutées de manière séquentielle. L'évaluation des règles commence par le numéro de priorité le plus bas.
- **Planification** : Planifiez votre déploiement à l'avance, analysez les sous-réseaux et les ports nécessaires. Une fondation bien structurée facilitera la gestion de vos NSG.
- **Convention de nommage** : Implémentez une convention de nommage uniforme et lisible pour vos règles afin de les identifier facilement.

#### Journaux de flux NSG

Il est essentiel de surveiller, gérer et connaître votre propre réseau pour le protéger et l'optimiser. Vous devez savoir l'état actuel du réseau, qui se connecte et d'où les utilisateurs se connectent. Les journaux de flux des groupes de sécurité réseau sont une fonctionnalité d'Azure Network Watcher qui vous permet de consigner les informations sur le trafic IP passant par un groupe de sécurité réseau.

#### Qu'est-ce qu'un ASG ?

Un Groupe de Sécurité d'Application (ASG) est une fonctionnalité de mise en réseau qui vous permet de regrouper des machines virtuelles Azure (VM) en fonction de l'application à laquelle elles appartiennent. Les ASG permettent de définir des politiques de groupe de sécurité réseau (NSG) à l'aide de regroupements d'applications logiques plutôt que d'adresses IP individuelles. Cela simplifie la gestion des règles de sécurité réseau, surtout lorsque plusieurs machines virtuelles appartiennent à la même application ou service.

**Avantages des ASG :**

- **Politiques de sécurité cohérentes** : Avec les ASG, vous pouvez vous assurer que toutes les VM d'un groupe suivent les mêmes politiques de sécurité.
- **Contrôle granulaire** : Créez des politiques de sécurité réseau détaillées basées sur les charges de travail, les applications ou les environnements. Les ASG facilitent la segmentation du réseau pour contenir les violations.
- **Évolutivité** : Les ASG s'ajustent automatiquement pour s'adapter aux changements de l'échelle des VM, éliminant le besoin de configuration supplémentaire.

#### En Pratique

Considérons un exemple de déploiement dans une souscription Azure avec les groupes suivants :

- **G1 — Vert** : win-vm01, win-vm03, lin-vm01
- **G2 — Rose** : win-vm02, win-vm04
- **G3 — Rouge** : lin-vm02

Les VM sont situées dans des sous-réseaux désignés avec des NSG assignés et divisés à l'aide des ASG. Chaque NSG contient des règles spécifiant la source et/ou la destination basées sur les ASG.

#### Conclusion

La mise en œuvre des Groupes de Sécurité d'Application (ASG) d'Azure représente une avancée significative dans la gestion simplifiée de la sécurité réseau et l'amélioration des mécanismes de protection pour les applications hébergées sur Azure. En permettant le regroupement des machines virtuelles (VM) et d'autres ressources en fonction de leur structure fonctionnelle, sécuritaire ou applicative, les ASG facilitent une approche plus organisée et efficace des politiques de sécurité réseau. Cela permet l'application de contrôles granulaires adaptés aux besoins spécifiques de chaque groupe, réduisant ainsi la complexité des règles de sécurité réseau et minimisant le risque de mauvaises configurations.

Azure propose des outils puissants pour gérer la sécurité réseau de manière efficace et en toute sécurité. En comprenant et en utilisant correctement les NSG et les ASG, vous pouvez mieux protéger vos ressources et simplifier la gestion de votre infrastructure réseau.
