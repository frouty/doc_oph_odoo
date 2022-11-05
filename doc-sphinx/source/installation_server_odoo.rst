Installation du server ODDO
###########################

Running ODOO in virtual environnement
*************************************

différent version d'odoo differente version de python

:: 

   python3 --version                                                         ─╯
   Python 3.9.2

Installation and setting odoo
=============================
Installation de odoo
--------------------
clone de odoo à partir de github

Ensuite on a juste besoin de addons/, odoo/, odoo-bin file et requirements.txt ::

   cd /docs/odoo/
   git pull upstream <version odoo>
   git checkout <version odoo>
   sudo mkdir -p /opt/odoo/odoo<version odoo>
   sudo rsync-copy /path/to/odoo/addons /opt/odoo/odoo<version odoo>
   sudo rsync-copy /path/to/odoo/odoo /opt/odoo/odoo<version odoo>
   sudo rsync-copy /path/to/requirement.txt /path/to/odoo/oddo-bin /opt/odoo/odoo<version odoo>
   sudo rsync-copy odoo-bin /opt/odoo/odoo<version odoo>

installation des dépendances odoo
---------------------------------
::

   apt install python3-dev libxml2-dev libxslt1-dev libldap2-dev libssl-dev libsasl2-dev libpq-dev 

Installation et creation du virtualenv
--------------------------------------

install pip3 et virtualenv::
  
   apt install python3-pip python3-venv

Il y a python3-venv et python3-virtualenv. Quelle différence? Depuis python 3.3 venv remplace virtualenv.


Creation du virtualenv et installation des requirements.
--------------------------------------------------------
::
   
   cd /opt/odoo/odoo<version odoo>
   python3 -m venv <venv-directory> # meme directory que odoo ou un autre directory
   cela install bin directory, include dir, lib dir, pyenv.cfg, share dir, link lib64 to lib . Cela crée un <venv-directory>
   source  <venv-directory>/bin/activate
   python --version # pour vérifier que tout a bien marché.
   which pyhon # pour vérifier que tout a bien marché.
   which pip
   sudo pip3 install --upgrade pip 
   sudo pip3 install -r requirements.txt

On renseigne la version de python que l'on veut dans notre venv avec la commande python3 -m <venv diretory>. 

Désactivation du venv
^^^^^^^^^^^^^^^^^^^^^
:: 

   deactivate
   which python # on verifie que l'on est bien retourné à la verison systeme de python
   python --version  # on verifie.


Installation et setting postgresql
----------------------------------

::

   sudo apt install postgresql
   sudo su - postgres
   psql
   postgres=#CREATE USER <version odoo> WITH CREATEDB ENCRYPTED PASSWORD 'odoo'; 
   postgres=#/du list  user pour vérifier que cela marche
   postgres=#quit
     
configuration de pg_hba file. ::

   sudo cp pg_hba.conf pg_hba.conf.ini
   sudo nano /etc/postgresql/<postgresql-version>/main/pg_hba.conf
   add line : local   all             odoo16                                  md5
   systemctl status postgresql
   systemctl restart postgresql
   
creation d'un fichier de conf ::

   touch odoo.conf && nano odoo.conf
   [options]
   db_host = 127.0.0.1
   db_port = 5432
   db_user = odoo16
   db_password = odoo
   http_port = 8069
   longpolling_port = 8072
   addons_path = addons

running odoo-bin ::

   ./odoo-bin -c odoo.conf
   blabla
   odoo.service.server: HTTP service (werkzeug) running on cobalt.lan:8069
   
   On se connecte avec localhost:8069
   me donne un master password for database. Je le range dans le manager de password.
   bingo
   