Comment filtrer les database dans l'interface database manager?
###############################################################

Dans le fichier /etc/odoo/odoo7-server.conf db_name = False
pas de filtrage db_name = nomdedatabase
/opt/odoo/odoo7-server/openerp-server --addons-path
/opt/odoo/odoo7-server/addons,/opt/odoo/odoo7/custom/addons/aeroo,/opt/odoo/odoo7/custom/addons/oph
-w dKLiHn -r odoo



Comment lister les databases?
#############################

Sur le server

::

   su postgres 
   $ psql 
   psql (9.4.10) Type "help" for help.

   postgres=# \l

pour quitter
:: 

   postgres=# \q exit


Comment lister les tables d'une database
########################################

se connecter ::

   psql -d nom_de_database
   ou
   psql 
   postgres=#\c nom_de_database
   postgres=#\dt
   ou
   postgres=#\dt+ # pour lister les tables.
 