# Projet PME_fictive

Le matériel : 2 commutateurs, 1 serveur, 4 câbles Ethernet, 2 points d'accès UniFi.

1. Schéma de nommage du matériel : 

2. Schéma Physique (draw.io) :

![Shéma Physique](https://github.com/user-attachments/assets/c44b668b-45a7-4f7f-b311-7b435983edd1)

4. plan adressage vlans : 

5. Schéma logique (draw.io) :

![Shéma Logique](https://github.com/user-attachments/assets/ab12eb7c-b429-4fd6-8e51-67dae18de049)

6. Installtion et configuration d'ESXI sur le serveur MTL-SRV-01

7. Configuration du commutateur MTL-SW-02 :
   [Configuration](https://github.com/trolul/PME_fictive/blob/main/startup-config.txt)

8. Création des VMs : windows server, debian_11, almalinux, windows_10, opnsense

9. Configuration du routeur virtuel Opnsense (NAT, DNS, DHCP, redirection de port) :
    [Configuration](https://github.com/trolul/PME_fictive/blob/main/config-OPNsense.localdomain.xml)

   Configuration DHCP dynamique sur Opnsense :
      - VLAN 10 (WIFI_public) : 192.168.10.50 à 100
      - VLAN 20 (Gestion) : 192.168.20.50 à 100
      - VLAN 30 (Corporatif) : 192.168.30.50 à 100
      - VLAN 40 (Serveurs) : 192.168.40.50 à 100
      - VLAN 50 (DMZ) : 192.168.50.50 à 100

   Configuration DNS dynamique sur Opnsense : 9.9.9.9

   Configuration DNS
   Zone directe PME.fictive vers 192.168.40.51
      Zones inversées :
      - 10.168.192: MTL-SRV-03 vers 192.168.10.50
      - 20.168.192: MTL-SRV-01 vers 192.168.20.50
      - 40.168.192: MTL-SRV-02 vers 192.168.40.50
      - 50.168.192: MTL-SRV-O4 vers 192.168.50.50

   Configuration Active Directory :
      - Création d'OU de location : MTL, puis : Département, puis : Local_1, utilisateur, ordinateurs
      - Création d'utilisateur : trolul, membre du groupe : administrateurs domain et techniciens
      - Création de groupe : techniciens et stratégies de groupe : administrateurs domain, un mot de passe complexe, les connexions distantes via bureau à distance, l'ouverture et fermeture de la session

   Redirection de port 206.167.46.16 vers IP web almalinux

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
