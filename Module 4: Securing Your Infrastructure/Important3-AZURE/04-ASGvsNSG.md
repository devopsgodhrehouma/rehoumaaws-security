# ASG vs NSG

- Explication simplifiée des différences entre un groupe de sécurité des applications (ASG) et un groupe de sécurité réseau (NSG) dans Azure :

### Groupe de Sécurité Réseau (NSG)

#### Qu'est-ce que c'est ?

- **NSG** est utilisé pour filtrer le trafic réseau vers et depuis les ressources Azure, telles que les machines virtuelles (VM), les sous-réseaux, etc.
- Il fonctionne au niveau de la couche réseau et permet de définir des règles pour autoriser ou refuser le trafic entrant et sortant.

#### Fonctionnement :

- **Règles :** Vous pouvez créer des règles basées sur des critères comme le port, le protocole, l'adresse IP source et destination.
- **Exemple :** Vous pouvez configurer un NSG pour permettre le trafic HTTP (port 80) vers une VM et refuser tout autre trafic entrant.

#### Utilisation typique :

- **Scénario :** Vous avez plusieurs VM dans un sous-réseau et vous voulez contrôler le trafic réseau vers toutes ces VM en une seule fois. Vous appliquez un NSG au sous-réseau.

### Groupe de Sécurité des Applications (ASG)

#### Qu'est-ce que c'est ?

- **ASG** est utilisé pour simplifier la gestion des règles de sécurité en regroupant les machines virtuelles (VM) ayant des fonctions similaires.
- Il permet de définir des règles de sécurité basées sur des noms de groupes plutôt que sur des adresses IP.

#### Fonctionnement :

- **Règles :** Vous créez des règles de sécurité dans un NSG en utilisant des ASG comme source ou destination.
- **Exemple :** Vous avez un ASG nommé `web-servers` qui contient toutes vos VM de serveur web. Vous créez une règle dans un NSG qui permet le trafic HTTP vers l'ASG `web-servers`.

#### Utilisation typique :

- **Scénario :** Vous avez des applications complexes avec de nombreuses VM réparties sur plusieurs sous-réseaux. Vous utilisez des ASG pour regrouper les VM par fonction (par exemple, `web-servers`, `database-servers`) et définissez des règles de sécurité spécifiques pour chaque groupe.

### Différences Clés

1. **Niveau d'application :**
   - **NSG :** Appliqué aux sous-réseaux ou aux interfaces réseau des VM.
   - **ASG :** Utilisé pour regrouper les VM et simplifier la gestion des règles dans les NSG.

2. **Flexibilité :**
   - **NSG :** Moins flexible pour les environnements complexes car il repose sur des adresses IP et des plages de ports.
   - **ASG :** Plus flexible car il permet de gérer les règles de sécurité en utilisant des noms de groupes, ce qui est plus facile à maintenir dans des environnements dynamiques.

3. **Utilisation :**
   - **NSG :** Idéal pour des configurations simples où les règles de sécurité doivent être appliquées uniformément à un sous-réseau ou à une VM.
   - **ASG :** Idéal pour des configurations complexes avec des applications multi-tiers, où les VM doivent être regroupées par rôle ou fonction.

### Exemple Visuel Simplifié

#### NSG

- Vous avez deux VM : `VM1` et `VM2`.
- Vous voulez autoriser le trafic HTTP vers les deux VM.
- **NSG:** Vous créez un NSG avec une règle permettant le trafic HTTP et appliquez ce NSG aux deux VM.

#### ASG

- Vous avez deux groupes de VM : `web-servers` (contenant `VM1` et `VM2`) et `database-servers` (contenant `VM3`).
- Vous voulez autoriser le trafic HTTP vers les `web-servers` et le trafic SQL vers les `database-servers`.
- **ASG:** Vous créez deux ASG (`web-servers` et `database-servers`). Vous définissez des règles dans un NSG en utilisant ces ASG comme source et destination.

### Résumé

- **NSG** : Bon pour des configurations simples, moins flexible pour des environnements complexes.
- **ASG** : Offre une gestion simplifiée des règles de sécurité dans des environnements complexes avec des applications multi-tiers.

En résumé, utilisez les NSG pour des besoins de sécurité réseau de base et les ASG pour simplifier la gestion des règles de sécurité dans des environnements de cloud complexes.

-----
# Annexe : 

Pour créer des règles de sécurité pour un groupe de sécurité des applications (Application Security Group, ASG) en utilisant Azure CLI, suivez les étapes ci-dessous. Les ASG permettent de gérer les règles de sécurité de réseau pour les applications en utilisant des noms plutôt que des adresses IP.

### Créer un Groupe de Sécurité des Applications (ASG)

1. **Créer un ASG :**
   Utilisez la commande suivante pour créer un ASG. Remplacez `<NomDeL'ASG>` et `<NomDuGroupeDeRessources>` par vos propres valeurs.

   ```bash
   az network asg create \
     --resource-group <NomDuGroupeDeRessources> \
     --name <NomDeL'ASG>
   ```

### Exemple spécifique :

Si votre groupe de ressources s'appelle `myResourceGroup` et que vous voulez créer un ASG nommé `myAppSecurityGroup`, la commande serait :

```bash
az network asg create \
  --resource-group myResourceGroup \
  --name myAppSecurityGroup
```

### Utiliser l'ASG dans une Règle NSG

1. **Créer une règle NSG utilisant un ASG pour le trafic HTTP (port 80) :**
   Utilisez la commande suivante en remplaçant `<NomDuNSG>`, `<NomDuGroupeDeRessources>`, `<NomDeLaRègle>` et `<NomDeL'ASG>` par vos propres valeurs.

   ```bash
   az network nsg rule create \
     --resource-group <NomDuGroupeDeRessources> \
     --nsg-name <NomDuNSG> \
     --name <NomDeLaRègle> \
     --priority 310 \
     --direction Inbound \
     --access Allow \
     --protocol Tcp \
     --destination-port-ranges 80 \
     --source-port-ranges '*' \
     --source-asgs <NomDeL'ASG> \
     --destination-asgs <NomDeL'ASG>
   ```

### Exemple spécifique :

Si votre groupe de ressources s'appelle `myResourceGroup`, votre NSG s'appelle `HelloWorldVm-nsg`, et vous voulez créer une règle nommée `AllowHTTPInbound` en utilisant l'ASG nommé `myAppSecurityGroup`, la commande serait :

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowHTTPInbound \
  --priority 310 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 80 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

### Ajouter d'Autres Exemples

#### SSH (port 22)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowSSHInbound \
  --priority 300 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 22 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

#### HTTPS (port 443)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowHTTPSInbound \
  --priority 320 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 443 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

#### SMTP (port 25)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowSMTPInbound \
  --priority 330 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 25 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

#### FTP (port 21)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowFTPInbound \
  --priority 340 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 21 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

#### MySQL (port 3306)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowMySQLInbound \
  --priority 350 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 3306 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

#### RDP (port 3389)

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowRDPInbound \
  --priority 360 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 3389 \
  --source-port-ranges '*' \
  --source-asgs myAppSecurityGroup \
  --destination-asgs myAppSecurityGroup
```

### Résumé des Commandes

| Service  | Nom de la Règle      | Port  | Priorité |
|----------|-----------------------|-------|----------|
| SSH      | AllowSSHInbound       | 22    | 300      |
| HTTP     | AllowHTTPInbound      | 80    | 310      |
| HTTPS    | AllowHTTPSInbound     | 443   | 320      |
| SMTP     | AllowSMTPInbound      | 25    | 330      |
| FTP      | AllowFTPInbound       | 21    | 340      |
| MySQL    | AllowMySQLInbound     | 3306  | 350      |
| RDP      | AllowRDPInbound       | 3389  | 360      |

En suivant ces exemples, vous pouvez configurer des règles de sécurité réseau pour différents types de trafic en utilisant des groupes de sécurité des applications (ASG) avec Azure CLI. Adaptez les commandes selon vos besoins spécifiques et assurez-vous que les numéros de priorité n'entrent pas en conflit avec d'autres règles existantes dans votre NSG.
