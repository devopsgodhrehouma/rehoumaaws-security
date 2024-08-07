# Autoriser le trafic HTTP (port 80) - Méthode 2

Pour autoriser le trafic HTTP (port 80) dans un groupe de sécurité réseau (NSG) en utilisant Azure CLI, suivez les étapes ci-dessous :

# 1 - Étapes pour Autoriser le Trafic HTTP avec Azure CLI :

1. **Ouvrir votre terminal Azure CLI :**
   - Si vous n'avez pas encore Azure CLI installé, vous pouvez le télécharger et l'installer à partir de [ce lien](https://docs.microsoft.com/fr-fr/cli/azure/install-azure-cli).

2. **Se connecter à votre compte Azure :**
   ```bash
   az login
   ```

3. **Créer une nouvelle règle de sécurité pour autoriser le trafic HTTP :**
   Utilisez la commande suivante en remplaçant `<NomDuNSG>`, `<NomDuGroupeDeRessources>` et `<NomDeLaRègle>` par vos propres valeurs.

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
     --source-address-prefixes '*' \
     --destination-address-prefixes '*'
   ```

# 2 - Exemple spécifique :

Si votre groupe de ressources s'appelle `myResourceGroup`, votre NSG s'appelle `HelloWorldVm-nsg`, et vous voulez créer une règle nommée `AllowHTTPInbound`, la commande serait :

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Détails des Paramètres :

- `--resource-group`: Le nom de votre groupe de ressources Azure.
- `--nsg-name`: Le nom de votre groupe de sécurité réseau (NSG).
- `--name`: Le nom que vous souhaitez donner à la règle de sécurité.
- `--priority`: Le numéro de priorité pour la règle (plus le nombre est bas, plus la priorité est haute).
- `--direction`: La direction du trafic (`Inbound` pour le trafic entrant).
- `--access`: Le type d'accès (`Allow` pour autoriser le trafic).
- `--protocol`: Le protocole (`Tcp` dans ce cas, ou `Any` si vous voulez autoriser tous les protocoles).
- `--destination-port-ranges`: Les plages de ports de destination (ici, `80` pour HTTP).
- `--source-port-ranges`: Les plages de ports source (`*` pour tous les ports).
- `--source-address-prefixes`: Les préfixes d'adresses source (`*` pour toutes les adresses).
- `--destination-address-prefixes`: Les préfixes d'adresses de destination (`*` pour toutes les adresses).

En utilisant cette commande, vous allez créer une règle de sécurité dans votre NSG qui permet le trafic HTTP (port 80) à partir de n'importe quelle source.

# Autres commandes : 

- Je vous présente quelques exemples de commandes Azure CLI pour autoriser différents types de trafic, notamment SSH, HTTP, HTTPS et SMTP. 
- Vous pouvez adapter les commandes à vos besoins spécifiques en remplaçant les noms des groupes de ressources, des NSG et des règles.

### Autoriser le Trafic SSH (port 22)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic HTTP (port 80)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic HTTPS (port 443)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic SMTP (port 25)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic FTP (port 21)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic MySQL (port 3306)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Autoriser le Trafic RDP (port 3389)

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
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
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

Ces exemples vous permettent de configurer des règles de sécurité réseau pour différents types de trafic en utilisant Azure CLI. Adaptez les commandes selon vos besoins spécifiques et assurez-vous que les numéros de priorité n'entrent pas en conflit avec d'autres règles existantes dans votre NSG.
