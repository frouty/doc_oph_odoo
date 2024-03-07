# 22 février 2018
Normalement j'ai l'installation de aeroo avec son scritpt d'install. 
mais je n'ai pas encore pu le voir dans odoo.
maintenant il faut essayer de lancer le server odoo V8 et voir si on peut importer le module aeroo report.

Comme j'ai migré en debian 9  je ne peux plus utiliser le pc de la maison pour faire le dev. 
J'utilise le pc du cabinet.
j'essaie de faire tourner la base de donnée de prod sur linuxbox avec le nouveau server.  
OK c'est bon

sur linuxbox git pull origin <branch>
cd odoogoeen 
./start.sh pour lancer le server.

sur saphir git pull origin upstream <branch>


# 01 mars 2018
la branche machineinreport a l'air de marcher 
voir à la porter en prod. 

# 25 02 2020 
le pc de la maison est en debian 10. J'écris le code sur saphir à la maison. Mais je teste le code sur linuxbox au cabinet. 
Il y a des erreurs " ascii " sur linuxbox. Mais je ne retrouve pas ces erreurs avec la machine de production openerp70/

# 13/04/2021
J'ai fait des améliorations. rien de majeur surtout des modifications d'odt. 
Et l'odt EVASAN2 ne veut plus se générer sur la machine openerp70. Il fonctionne tres bien sur la machine linuxbox. 
Je crée une nouvelle database. Et là cela remarche à nouveau. 
J'essaie autre chose : Configuration / Module installé / Ophtalmology / mise à jour. : CA MARCHE.

# 19/11/2021 
j'ai fait du ménage dans les branches
j'ai installé une machine de dev @home. Il faut que le vpn marche pour se connecter au rt5100 raspberry.
pour cela systemctl status   
systemctl start openvpn@myvpn.service. myvpn est un lien dans /etc/openvpn vers le bon fichier de conf. # obsolete
maintenant c'est systemctl start openvpn-client@ODOO7_SPARE_DEV.service
le fichier de log : /var/log/openvpn.log

