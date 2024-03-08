Developpement de oph_odoo16
###########################

odoo server est fonctionnel dans /opt/odoo/odoo16/

Mise en place du systeme de fichier
***********************************

::

   mkdir -p /opt/odoo/odoo16/custom/addons && cd /opt/odoo/odoo16/custom/addons
   git clone git@github.com:frouty/oph_odoo16.git 

Update du fichier odoo.conf
***************************

::

   addons_path = /opt/odoo/odoo16/odoo-server16/addons, /opt/odoo/odoo16/custom/addons
   # ne pas mettre /opt/odoo/odoo16/custom/addons/oph_odoo
   

Chaque fois que l'on modifie un fichier python il faut redémarrer le server. et on va rajouter un paramètre::

   -u oph_odoo16 # nous voulons upgrade le module oph_odoo16
   -d odoo16-01 # doit toujours etre utilisé avec u. 
   cd /opt/odoo/odoo16/odoo-server16
   ./odoo-bin --addons-path=custom/addons,odoo_server16/addons -u oph_odoo16
   ou 
   ./odoo-bin  -c odoo.conf -u oph_odoo16 -d odoo16-01 --dev xml
   
   
Passer en mode developper dans le webgui
****************************************

Aller à : Settings ‣ Activate the developer mode.

Il y a aussi une extension odoo debug. Mais je n'ai pas trop bien réussi à la faire marcher.

https://www.odoo.com/documentation/16.0/applications/general/developer_mode.html#developer-mode

Comment démarrer le server
**************************
::

   cd /opt/odoo/odoo16
   python --version # on verifie si notre venv est actif.
   source  /opt/odoo/odoo16/bin/activate 
   python --version
   ./odoo-server16/odoo-bin  -c odoo-server16/odoo.conf -u oph_odoo16 -d odoo16-01 --dev xml
   
   
Comment avoir la liste des champs ordonnés pour une table
#########################################################
::

   psql 
   SELECT column_name
   FROM information_schema.columns 
   WHERE table_schema = 'public' 
   AND table_name = 'blah' 
   ORDER BY column_name ASC;

Comment avoir la liste des champs pour une table
################################################

::

   psql
   \d+ nomdelatable
   