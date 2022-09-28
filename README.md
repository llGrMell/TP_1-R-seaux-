
# Exploration locale en solo

1. Affichage d'informations sur la pile TCP/IP locale

**.ipconfig / all** (Permet d'afficher la configuration TCP/IP complète de toutes les cartes réseau):

Wifi:
**nom (Descrption) :** Intel(R) Wi-Fi 6 AX201 160MHz
**Adresse Mac (Adresse Physique) :** A4-6B-B6-E9-E1-E8
**adresse IP de l'interface WiFi (Adresse IPv4):** 10.33.18.91

Ethernet:
**nom (Descrption) :** Realtek PCIe GbE Family Controller
**Adresse Mac (Adresse Physique) :** D8-BB-C1-20-6B-1F
**adresse IP de l'interface Ethernet (Adresse IPv4):** non
<br>
**B/** Affichez votre gateway

**.ipconfig / all** (Permet d'afficher la configuration TCP/IP complète de toutes les cartes réseau):

**Passerelle par défaut :** 10.33.19.254
<br>
**C/** En graphique (GUI : Graphical User Interface) Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

Chemin : Panneau de configuration\Réseau et Internet\Connexions réseau

![](https://i.imgur.com/yXNlPqs.png)
```
IP: 10.33.18.91
Adresse Physique: A4-6B-B6-E9-E1-E8
Passerelle : 10.33.19.254
```
<br>
**D/** Question

Il permet de se connecter au réseaux d'Ynov.

2. **Modifications des informations**

...


# II. Exploration locale en duo


mon pc va être en 192.168.0.2, le pc connecté a internet sera en 192.168.0.1 et la gateway sera en 192.168.0.3 avec un masque 255.255.255.252

Envoi d’une requête 'Ping'  192.168.0.1 avec 32 octets de données : 
```
Réponse de 192.168.0.1 : octets=32 temps=344 ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps=1 ms TTL=128
```
je peut ping l'aure pc, testons avec google :

Envoi d’une requête 'Ping'  8.8.8.8 avec 32 octets de données : 
```
Réponse de 8.8.8.8 : octets=32 temps=21 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=23 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=22 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=21 ms TTL=113


on peut uttiliser un traceroute (tracert 8.8.8.8 (pour dns.google )).


Détermination de l’itinéraire vers dns.google [8.8.8.8]
avec un maximum de 30 sauts :
```
  1    <1 ms    <1 ms    <1 ms  MSI [192.168.0.1]
  2     *        *        *     Délai d’attente de la demande dépassé.
  3     7 ms     7 ms     6 ms  10.33.19.254
  4     5 ms     5 ms     4 ms  137.149.196.77.rev.sfr.net [77.196.149.137]
  5    10 ms     8 ms     9 ms  108.97.30.212.rev.sfr.net [212.30.97.108]
  6    23 ms    22 ms    23 ms  222.172.136.77.rev.sfr.net [77.136.172.222]
  7    24 ms    22 ms    22 ms  221.172.136.77.rev.sfr.net [77.136.172.221]
  8    25 ms    26 ms    25 ms  186.144.6.194.rev.sfr.net [194.6.144.186]
  9    22 ms    27 ms    22 ms  186.144.6.194.rev.sfr.net [194.6.144.186]
 10    24 ms    23 ms    22 ms  72.14.194.30
 11    22 ms    22 ms    22 ms  172.253.69.49
 12    27 ms    22 ms    38 ms  108.170.238.107
 13    23 ms    22 ms    22 ms  dns.google [8.8.8.8]
```

avec netcat on peut lancer un chat 
    - on ecrit "192.168.0.2 8888" dans la console de netcat sur le pc client.
    - on ecrit "-l -p 8888" dans la console de netcat sur le pc server.

```
  TCP    192.168.0.2:9999       192.168.0.1:1055       ESTABLISHED
```

pour pouvoir acceder a ce service via ethernet et wifi, pour pouvoir préciser l'IP d'écoute il faut lancer netcat avec cette commande : 

```
.\nc.exe -l -p 9999 -s 192.168.0.2
```

PS C:\WINDOWS\system32> netstat -a -n -b  | select-string 9999

le ping a déjà une règle nomé **"Analyse de l’ordinateur virtuel (Demande d’écho - Trafic entrant ICMPv4)"** dans le fire-wall qui permet le ping

il suffit après de créer une règle pour associet à netcat le port 9999 au service.
je l'ai appelé **"tcp netcat"**
```

<br>
# III. Manipulations d'autres outils/protocoles côté client

**1. DHCP**

afficher l'adresse IP du serveur DHCP du réseau WiFi YNOV,  trouver la date d'expiration de votre bail DHCP :

    .Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
    .Bail obtenu. . . . . . . . . . . . . . : mercredi 28 septembre 2022 15:20:19
    .Bail expirant. . . . . . . . . . . . . : jeudi 29 septembre 2022 14:02:00
<br>

**2. DNS**
**A/** trouver l'adresse IP du serveur DNS que connaît votre ordinateur

    ````Address:  8.8.8.8````

**B/** Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

.pour google.com :  
```
Nom :    google.com
Addresses:  2a00:1450:4007:806::200e
          216.58.213.78
```
.pour ynov.com /
```
Nom :    ynov.com
Addresses:  2606:4700:20::681a:be9
          2606:4700:20::ac43:4ae2
          2606:4700:20::681a:ae9
          172.67.74.226
          104.26.10.233
          104.26.11.233
```
          

**C/** Faites un reverse lookup (= "dis moi si tu connais un nom de domaine pour telle IP") :

Pour l'adresse 78.74.21.21 : 
```
Serveur :   dns.google
Address:  8.8.8.8
Nom :    host-78-74-21-21.homerun.telia.com
Address:  78.74.21.21
```

pour l'adresse 92.146.54.88 : 
```
Serveur :   dns.google
Address:  8.8.8.8

*** dns.google ne parvient pas à trouver 92.146.54.88 : Non-existent domain
```

Ces deux adresses appartiennent a google 
![](https://i.imgur.com/0vcAaTe.png)

Nous pouvons voir sur le screen que la lan entre nos deux pc il y a des requets TCP en bleu émant nectcat et ichp en violet provenant d'un ping.
