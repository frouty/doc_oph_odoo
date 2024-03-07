Comment restaurer une database
##############################
L'interface odoo de restauration des database ne marche plus
sur la machine de dev. Pas de message pour essayer de
régler le problème.
Du coup en ligne de commande

Récuperer le dump backup dans l'interface odoo déplacer le
fichier dump sauvegardé dans la machine de dev

:: 

   su
   su postgres
   createdb nom_de_database -O odoo
   pg_restore -d nom_de_database /path/to/dump
   c'est un peu long. Et rien ne s'affiche
   vérifier avec psql \l

Comment supprimer une database
##############################

:: 

   su
   su postgres
   psql
   postgres=#\l liste les databases
   postgres=#DROP DATABASE nom_de_database;