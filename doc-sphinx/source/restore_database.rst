Comment restaurer une database
==============================
L'interface odoo de restauration des database ne marche plus sur la machine de dev.
Pas de message pour essayer de regler le probleme
Du coup en ligne de commande

récuperer le dump backup dans l'interface odoo
déplacer le fichier dump sauvegardé dans la machine de dev
su
su postgres
pg_restore -d nom_de_database -C  /path/to/dump
