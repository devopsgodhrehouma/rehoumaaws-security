# Question 
Comment appelle-t-on le concept de mettre un réseau intermédiaire avant le réseau principal pour que ce réseau intermédiaire puisse être attaqué à la place du réseau principal ?

### Réponse Vulgarisée
Ce concept s'appelle une **DMZ (zone démilitarisée)**. 

### Explication Vulgarisée
Une DMZ est comme une zone tampon entre Internet (le monde extérieur) et le réseau interne de votre organisation (votre maison). Imaginez que vous avez des services que vous voulez rendre accessibles au public, comme un site web ou un serveur de messagerie. Vous ne voulez pas que les personnes de l'extérieur aient un accès direct à votre réseau interne, car cela pourrait être dangereux.

### Voici comment cela fonctionne :

1. **Réseau interne** : C'est votre réseau principal, où vous gardez toutes vos informations sensibles et importantes. C'est comme votre maison.

2. **Internet** : C'est le monde extérieur, où tout le monde peut essayer d'accéder à vos services publics.

3. **DMZ** : C'est une zone spéciale entre votre maison et le monde extérieur. C'est comme une véranda ou une avant-cour où vous pouvez recevoir des visiteurs sans les laisser entrer directement dans votre maison. 

### Pourquoi utiliser une DMZ ?

- **Isolation** : Les services publics (comme les sites web) sont placés dans la DMZ, ce qui empêche les attaquants d'accéder directement à votre réseau interne.
- **Sécurité** : Le trafic entre Internet et la DMZ est contrôlé par des pare-feu, et le trafic entre la DMZ et le réseau interne est également très limité.
- **Réduction des risques** : Si un attaquant parvient à pénétrer dans la DMZ, il ne pourra pas facilement accéder au réseau interne.

### Exemple de mise en place d'une DMZ

1. **Pare-feu extérieur** : Il filtre le trafic venant d'Internet vers la DMZ, ne permettant que les connexions nécessaires.
2. **Pare-feu intérieur** : Il contrôle le trafic entre la DMZ et le réseau interne, n'autorisant que les connexions spécifiques.

### Schéma Simplifié
```
Internet --- Pare-feu extérieur --- DMZ --- Pare-feu intérieur --- Réseau interne
              (site web)                      (base de données)
```

### Illustration avec iptables (sur Linux)

Pour mettre en place ces règles avec `iptables` (un outil de pare-feu pour Linux) :

1. **Pare-feu extérieur** :
```bash
# Autoriser l'accès au site web dans la DMZ
iptables -A INPUT -p tcp --dport 80 -d 192.168.1.10 -j ACCEPT
```

2. **Pare-feu intérieur** :
```bash
# Autoriser le site web de la DMZ à communiquer avec la base de données interne
iptables -A FORWARD -p tcp -s 192.168.1.10 --dport 3306 -d 192.168.2.10 -j ACCEPT
```

Ainsi, une DMZ permet de protéger votre réseau interne en ajoutant une couche de sécurité supplémentaire.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
#### Annexe :  un réseau intermédiaire entre le réseau privée (sensible) et réseau publique (pas sécurisée) c'est DZ ?
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

Oui, ce concept est appelé une "zone démilitarisée" ou DMZ (en anglais, "Demilitarized Zone"). Une DMZ est une sous-réseau physique ou logique qui contient et expose les services externes d'une organisation à un réseau non fiable, généralement l'internet. L'objectif principal d'une DMZ est d'ajouter une couche de sécurité supplémentaire pour le réseau interne de l'organisation.

Voici comment cela fonctionne :

1. **Réseau interne (LAN)** : C'est le réseau principal et le plus sécurisé de l'organisation, contenant les ressources critiques et les données sensibles.

2. **Réseau externe (Internet)** : Le réseau non fiable auquel tout le monde peut accéder.

3. **DMZ** : Une zone intermédiaire qui contient les serveurs accessibles publiquement, tels que les serveurs web, les serveurs de messagerie, et les serveurs DNS. Ces serveurs dans la DMZ sont isolés du réseau interne.

### Pourquoi utiliser une DMZ ?

1. **Isolation des services publics** : Les services accessibles au public sont placés dans la DMZ, séparés du réseau interne, ce qui limite l'accès direct des attaquants aux ressources internes.

2. **Contrôle des accès** : Le trafic entre le réseau externe et la DMZ est strictement contrôlé par des pare-feu. Le trafic entre la DMZ et le réseau interne est également contrôlé, avec des règles plus strictes.

3. **Réduction des risques** : En cas de compromission d'un serveur dans la DMZ, les attaquants auront encore des barrières à franchir avant d'accéder au réseau interne.

### Exemple de mise en place d'une DMZ

1. **Pare-feu périmétrique** : Sépare le réseau externe de la DMZ. Il permet uniquement le trafic nécessaire vers les serveurs de la DMZ.

2. **Pare-feu interne** : Sépare la DMZ du réseau interne. Il autorise uniquement le trafic légitime et nécessaire entre la DMZ et le réseau interne.

3. **Serveurs dans la DMZ** : Hébergent des applications accessibles publiquement comme des sites web, des serveurs de messagerie, des serveurs FTP, etc.

### Schéma d'une architecture typique avec DMZ

```
Internet --- Pare-feu périmétrique --- DMZ --- Pare-feu interne --- Réseau interne
                    |                                |
              Serveur web                      Serveur de base de données
              Serveur mail                      Serveur de fichiers
```

### Exemple de Configuration avec iptables (Linux)

Pour illustrer la mise en place d'une DMZ avec `iptables`, voici un exemple de règles :

1. **Règles pour le pare-feu périmétrique** :

```bash
# Autoriser le trafic HTTP et HTTPS vers le serveur web dans la DMZ
iptables -A INPUT -p tcp --dport 80 -d 192.168.1.10 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -d 192.168.1.10 -j ACCEPT

# Autoriser le trafic SMTP vers le serveur de messagerie dans la DMZ
iptables -A INPUT -p tcp --dport 25 -d 192.168.1.20 -j ACCEPT
```

2. **Règles pour le pare-feu interne** :

```bash
# Autoriser le trafic du serveur web de la DMZ vers le serveur de base de données interne
iptables -A FORWARD -p tcp -s 192.168.1.10 --dport 3306 -d 192.168.2.10 -j ACCEPT

# Bloquer tout autre trafic de la DMZ vers le réseau interne
iptables -A FORWARD -s 192.168.1.0/24 -d 192.168.2.0/24 -j DROP
```

Ces règles assurent que seul le trafic autorisé entre la DMZ et les réseaux externes et internes est permis, augmentant ainsi la sécurité du réseau.

