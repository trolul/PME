# PME_fictive
Voici mon projet de création d'entreprise fictive (réalisé IRL).

Le matériel : 2 commutateurs, 1 serveur, 4 câbles Ethernet, 2 points d'accès UniFi.

1. Schéma de nommage du matériel : 

2. Schéma Physique (draw.io) :

![Shéma Physique](https://github.com/user-attachments/assets/c44b668b-45a7-4f7f-b311-7b435983edd1)

4. plan adressage vlans : 

5. Schéma logique (draw.io) :

![Shéma Logique](https://github.com/user-attachments/assets/ab12eb7c-b429-4fd6-8e51-67dae18de049)

6. Installtion et configuration d'ESXI sur le serveur MTL-SRV-01

7. Configuration du commutateur MTL-SW-02 :
   [Lien conf](https://github.com/trolul/PME_fictive/blob/main/startup-config.txt)

8. Création des VMs : windows server, debian_11, almalinux, windows_10, opnsense

9. Configuration du routeur virtuel Opnsense (NAT, DNS, DHCP, pare-feu, redirection de port) :
    [Lien conf](https://github.com/trolul/PME_fictive/blob/main/config-OPNsense.localdomain.xml)

10. Configuration du serveur virtuel web almalinux (HTTPS, domain, nginx) :




