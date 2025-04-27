# Projet PME_fictive

Le matériel : 2 commutateurs, 1 serveur, 4 câbles Ethernet, 2 points d'accès UniFi.

1. Schéma de nommage du matériel : 

2. Schéma Physique (draw.io) :

![Shéma Physique](https://github.com/user-attachments/assets/c44b668b-45a7-4f7f-b311-7b435983edd1)

4. Tableau Réseau : 

| **Nom du sous-réseau** | **Plage d’adresses (CIDR)** | **L'adresse ip de la passerelle** | **Rôle et utilisation** | **Mode d'attribution** | **id du VLAN** |
|--------|--------|-----|---|---|---|
| WIFI_public | 192.168.10.0/24 | 192.168.10.1 | plage assigné au public | statique et dynamique | 10 |
| Gestion | 192.168.20.0/24  | 192.168.20.1  | Gestion de ESXI | statique et dynamique | 20 |
| Corporatif | 192.168.30.0/24  | 192.168.30.1  | Gestion des employées | statique et dynamique | 30 |
| Serveurs | 192.168.40.0/24  | 192.168.40.1  | Gestion des serveurs | statique et dynamique | 40 |
| DMZ | 192.168.50.0/24 |  192.168.50.1 | Services accèssible uniquement par internet  | statique et dynamique | 50 |
| FAI | 206.167.46.0/24 |  206.167.46.1 | Fourniseur internet  | statique | 460 |

5. Schéma logique (draw.io) :

![Shéma Logique](https://github.com/user-attachments/assets/ab12eb7c-b429-4fd6-8e51-67dae18de049)

6. Installtion et configuration d'ESXI sur le serveur MTL-SRV-01

7. Configuration du commutateur MTL-SW-02 :
   [Configuration](https://github.com/trolul/PME_fictive/blob/main/startup-config.txt)

8. Création des VMs : windows_server, debian_11, almalinux, windows_10, opnsense

9. Configuration du routeur virtuel Opnsense (NAT, DNS, DHCP, redirection de port) :
    [Configuration](https://github.com/trolul/PME_fictive/blob/main/config-OPNsense.localdomain.xml)

   Configuration DHCP dynamique sur Opnsense : VLAN 10 (WIFI_public) : 192.168.10.50 à 100, etc...

   Configuration DNS dynamique sur Opnsense : 9.9.9.9

   Configuration des interfaces :
   | Interface | VLAN associé | Adresse IP | Passerelle |  
   |-----------|-------------|------------|------------|  
   | FAI            |       460     |      206.167.46.16     | 192.168.46.1 |  
   | WIFI_public    |       10      |      192.168.10.1      |              |  
   | gestion        |       20      |      192.168.20.1      |              |  
   | corporatif     |       30      |      192.168.30.1      |              |  
   | serveurs       |       40      |      192.168.40.1      |              |  
   | DMZ            |       50      |      192.168.50.1     |              |  

   Règles de pare-feu appliquées :
   | Règles      | Source | Destination | Protocole    | Port | Action |
   |-------------|--------|-------------|--------------|------|--------|
   | FAI  | *     | *                 | ipv4 TCP/UDP | 80-443   | Block   |
   | gestion  | *     | *          | ipv4 * | any             | Pass   |
   | WIFI_public | * | *         | ipv4 * | any    | Pass   |
   | corporatif  | *    | *     | ipv4 * | any    | Pass   |
   | serveurs  | *     | *         | ipv4 * | any             | Pass   |
   | DMZ  | *     | *                   | ipv4 * | any         | Pass   |

   _À faire après 11. : Redirection de port 206.167.46.16 vers IP web almalinux_

10. Configuration de la VM windows_server :

   Configuration DNS :
      - Zone directe : PME.fictive vers 192.168.40.51
      - Zones inversées : 10.168.192: MTL-SRV-03 vers 192.168.10.50, etc...

   Configuration Active Directory :
      - Création d'OU de location : MTL, puis : Département, puis : Local_1, utilisateur, ordinateurs
      - Création d'utilisateur : trolul, membre du groupe : administrateur domain et techniciens
      - Création de groupe : techniciens et ses stratégies de groupe : administrateur domain, mdp complexe, bureau à distance, ouverture et fermeture de session

11. Configuration d'une VM web almalinux :
    
    Installation de nginx et de HTTPS avec Let's Encrypt
    
    Achat de nom de domaine sur namecheap.com et configuration du Host_record :
    
    ![Host_record](https://github.com/trolul/PME_fictive/blob/main/dns_record.png)

12. Lancement d'un script d'installation UniFi controller sur debian_11 :
   [Youtube](https://www.youtube.com/watch?v=LP4dIl8Y_Xw)

11. Configuration des deux accès UniFi :

   | SSID | VLAN | ID | Sécurité | Modèle AP | Config Réseau |
   | --- | --- | --- | --- | --- | --- |
   | PME-PUBLIC | WIFI_public | 10 | WPA2 | unifi ap pro | 192.168.30.61 |
   | PME-EMPLOYES | Corporatif | 30 | WPA2 | unifi ap pro | 192.168.10.54 |
