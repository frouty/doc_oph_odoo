Installation et mise en route du server ODOO 16
###############################################

Fait sur le VPS  ::

   .o LSB modules are available.
   Distributor ID:   Ubuntu
   Description:   Ubuntu 22.04.4 LTS
   Release: 22.04
   Codename:   jammy
   

installation de ohmyzsh
***********************


faire google search : https://ohmyz.sh/

apt install zsh mate wget

installation des fonts
======================

::

   apt install fontconfig

Allez sur https://github.com/ryanoasis/nerd-fonts/releases/tag/v2.1.0

choisir un fichier

wget ce fichier zip

unzip le zip telechargé ::

   mkdir /usr/share/font/lenomduzipfile
   unzip lezipfile -d /usr/share/font/lenomduzipfile
   fc-cache -fv
   fc-list

installation de powerlevel10K
=============================

::

   git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc.

source ~/.zshrc

configure powerlevel10k

Il faudra le faire pour les autres users.

Installation des plugins ohmyzsh
================================
zsh-autosuggestion
------------------
https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md

::

   cd $HOME
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):

   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   
   git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search

:: 

   plugins=(git zsh-syntax-highlighting zsh-autosuggestions)


Installation and setting odoo
=============================

Odoo 16 source code sera installé dans 
   - /opt/odoo/odoo16/odoo16-server
Customs addons seront installés dans:
   - /opt/odoo/odoo16/custom-addons

Odoo 16 will run sous le user odoo16

Seuls les users linux du group odoo pourront lire et executer le code odoo 16

Odoo 16 tournera dans un environnement virtual isolé 

La database sera postgresql

Les utilisateurs pourront acceder à Odoo 16 avec http://IP_du_server:8069

Le user qui connait le master password pourra creer / effacer les databases. 

Odoo 16 tournera en daemon et demarrera automatiquement quand le server demarre. 

Prerequis
---------
:: 
   
   apt update; apt dist-ugrade
   apt install python3-pip build-essential python3.10-dev \
   python3.10-venv libsass-dev libjpeg-dev \
   libjpeg8-dev libldap-dev libldap2-dev libpq-dev \
   libsasl2-dev libxslt1-dev zlib1g-dev

Comment savoir si des paquets sont déja installés ?
---------------------------------------------------

::

   apt list <nom exact du paquet> ou
   apt-cache policy <nom exact du paquet>
   
Creation du group odoo et du linux user odoo16
----------------------------------------------

::

    addgroup odoo

::

    adduser odoo16 --system \
   --home=/home/odoo16 --disabled-login \
   --disabled-password --ingroup odoo
   
Setup postgresql
----------------

::

   apt install postgresql postgresql-contrib
   
Create a role
^^^^^^^^^^^^^

On crée un role qui a le même nom que le linux account. 

::

   sudo -u postgres createuser --interactive
   
   Enter name of role to add : odoo16
   Shall the new role be a superuser? : n
   Shall the new role be allowed to create databases? (y/n) y
   Shall the new role be allowed to create more new roles? (y/n) n

Installation et setting postgresql
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   sudo apt install postgresql
   sudo su - postgres
   psql
   postgres=#CREATE USER <version odoo> WITH CREATEDB ENCRYPTED PASSWORD 'odoo'; 
   postgres=#\du liste les user pour vérifier que cela marche
   postgres=#quit

Quelques commandes utiles SQL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

   psql $DB_NAME  # Connect to a database.
    \l  #List all the available databases.
    \dt  #List all the tables of the $DB_NAME database.
    \d $TABLE_NAME  #Show the structure of the table $TABLE_NAME.
    \q  #Quit the psql environment (ctrl + d).

Installation de wkhtmltopdf for generating pdf
----------------------------------------------
::
   
   sudo apt install xfonts-base xfonts-75dpi
   sudo wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
   sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb

Create a log directory, change owner, and set permissions (TODO)
----------------------------------------------------------------

::

   sudo mkdir /var/log/odoo16
   sudo chown odoo16:root /var/log/odoo16
   
set permission todo

Running ODOO in virtual environnement
#####################################

différentes versions d'odoo differentes versions de python

:: 

   python3 --version                                                         ─╯

Setup python virtual environnement
==================================
Création d'un dir pour le virtuel environnement qui va faire tourner odoo 16

::

   sudo mkdir -p /python-venv/3.10/odoo16
   sudo chown -hR odoo16:root /python-venv/3.10/odoo16
   
On passe sous le compte odoo16

::

   sudo su - odoo16 -s /bin/bash

Activation du virtual env 

::

   source /python-venv/3.10/odoo16/bin/activate
   pip install -U pip
   deactivate
   exit

Download and install Odoo 16
----------------------------

Pour faire la suite au travers de ssh il va falloir mettre en place la clefs publique du server odoo  dans **github** 

Creation d'une paire de clef sur le server odoo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Copie de la clef publique sur github
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Download Odoo depuis github
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   mkdir -p /opt/odoo/odoo16
   sudo su - odoo git clone https://github.com/odoo/odoo.git --depth 1 --branch 16.0 /opt/odoo/odoo16/odoo16-server
   #sudo git clone -b 16.0 https://github.com/odoo/odoo.git /opt/odoo/odoo16/odoo16-server
   sudo chown -hR root:odoo /opt/odoo/odoo16  <-- root:odoo ou odoo16:root

Création d'un directory pour les customs addons

::

   mkdir /opt/odoo/odoo16/custom-odoo16
   chown -hR odoo16:root /opt/odoo/odoo16/custom-odoo16
   

Installation des prerequis pour odoo16
--------------------------------------

::

   sudo su - odoo16 -s /bin/bash
   source /python-venv/3.10/odoo16/bin/activate
   pip install wheel
   pip install -r /opt/odoo/odoo16/odoo16-server/requirements.txt
   pip install pylibdmtx # aide odoo à imprimer des data matrix barcodes
   
   wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
   sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb
   Je bloque car me demande le password du user odoo16
   Donc finalement je ne fait pas l'install du deb wkhtmltopdf comme cela. Mais tout simplement en user root. voir ci dessus.


Démarrage de odoo 16 en post install immédiat
#############################################

Je test en restant dans le virtual env.
   
   ::

      /opt/odoo/odoo16/odoo-bin

Je me connecte http://IP_server:8069 

J'efface toutes les bases de données créent avant. Je ne fais rien sur les bases de données

:: 
   
   Ctrl C deux fois pour stoper le server 
   exit

Création du fichier de configuration de odoo 16
***********************************************

Création
========
::

   nano /etc/odoo16.confi
   
      [options]
   #addons_path = /opt/odoo/odoo16/odoo/addons,/opt/odoo/odoo16/addons,/home/odoo16/addons_repo_name
   addons_path = /opt/odoo/odoo16/odoo116-server/addons, /opt/odoo/odoo16/custlon-addons
   admin_passwd = admin
   csv_internal_sep = ,
   data_dir = /home/odoo16/.local/share/Odoo
   db_host = False
   db_maxconn = 64
   db_name = False
   db_password = False
   db_port = False
   db_sslmode = prefer
   db_template = template0
   db_user = False
   dbfilter =
   demo = {}
   email_from = False
   from_filter = False
   geoip_database = /usr/share/GeoIP/GeoLite2-City.mmdb
   gevent_port = 8072
   http_enable = True
   http_interface =
   http_port = 8069
   import_partial =
   limit_memory_hard = 2684354560
   limit_memory_soft = 2147483648
   limit_request = 8192
   limit_time_cpu = 60
   limit_time_real = 120
   limit_time_real_cron = -1
   list_db = True
   # Logging
   log_db = False
   log_db_level = warning
   log_handler = :INFO
   log_level = info
   logfile =
   max_cron_threads = 2
   osv_memory_age_limit = False
   osv_memory_count_limit = 0
   pg_path =
   pidfile =
   proxy_mode = False
   reportgz = False
   screencasts =
   screenshots = /tmp/odoo_tests
   server_wide_modules = base,web
   smtp_password = False
   smtp_port = 25
   smtp_server = localhost
   smtp_ssl = False
   smtp_ssl_certificate_filename = False
   smtp_ssl_private_key_filename = False
   smtp_user = False
   syslog = False
   test_enable = False
   test_file =
   test_tags = None
   transient_age_limit = 1.0
   translate_modules = ['all']
   unaccent = False
   upgrade_path =
   websocket_keep_alive_timeout = 600
   websocket_rate_limit_burst = 10
   websocket_rate_limit_delay = 0.2
   without_demo = False
   workers = 0
   x_sendfile = False  

Set owner ::

   chown odoo16:root /etc/odoo16.conf

Set permission ::

   todo

geoip database, data_dir  cela sert à quoi? TODO 



Odoo unit service  file
***********************

est ce qu'il faut le mettre en oeuvre quand on travaill en virtual env?

::

   nano /lib/systemd/system/odoo16.service
   
   [Unit]
   Description=Odoo16
   Documentation=http://www.odoo.com
   [Service]
   # Ubuntu/Debian convention:
   Type=simple
   User=odoo16
   ExecStart=/opt/odoo/odoo16/odoo16-server/odoo-bin -c /etc/odoo16.conf
   [Install]
   WantedBy=default.target

   ou 
   
   [Unit]
   Description=Odoo16
   After=network.target postgresql.service
   
   [Service]
   Type=simple
   PermissionsStartOnly=true
   User=odoo16
   Group=odoo
   SyslogIdentifier=odoo16
   PIDFile=/run/odoo16/odoo16.pid
   ExecStartPre=/usr/bin/install -d -m755 -o odoo16 -g odoo /run/odoo16
   ExecStart=/python-venv/3.10/odoo16/bin/python /opt/odoo/odoo16/odoo-bin -c /etc/odoo16.conf --pid=/run/odoo16/odoo16.pid
   ExecReload=/bin/kill -s HUP $MAINPID
   ExecStop=/bin/kill -s QUIT $MAINPID
   
   [Install]
   Alias=odoo16.service
   WantedBy=multi-user.target

Notify systemd that a new unit file exist ::
   
   systemctl daemon-reload

Enable the service and ask it to run on boot ::
 
   sudo systemctl enable --now odoo16   <-- cela crée des symlink dans /etc/systemd/system
      

start your Odoo 16 using systemd ::

   systemctl start odoo16

Set permissions pas necessaire c'est déjà fait par le système ::

   sudo chmod 755 /etc/systemd/system/odoo16.service
   sudo chown root: /etc/systemd/system/odoo16.service

Furthermore, the installation of Odoo was successful if it is shown as active ::

   sudo systemctl status odoo16.service


Use the following command to restart the Odoo service if you have made any modifications to the add-ons so that your instance will reflect the updates ::

   sudo systemctl restart odoo16.service

https://<ip du server odoo 16 on le trouve sur hostinger>:8069

On obtient 


.. image:: icono/firstdbcreation.png
   :width: 400
   :alt: Alternative text
   
master password proposé : tmqw-ak6n-3xyh

Database name : dbmars24

email odoo16@ophtalmologie.org

password gkvnFWsvjH5x3Kh

.. image:: icono/firstlogin.png
   :width: 400
   :alt: Alternative text

On se logue avec l'email admin defini précedemment et on obtient

.. image:: icono/adminpageacceuil.png
   :width: 800
   :alt: Alternative text

Il n'y pas grand chose dans l'interface :

- apps :
   -  main apps
   -  theme stores
   -  Thrid-Party Apps
- settings :
   - users & company 
   
Creation d'un nouveau user
**************************

Il y a des: 

-  internal user 
-  portal user

Quelles sont les différences?

Je crée un nouveau user. je modifie le mot de passe dans le webgui odoo. 

je me connecte avec ce nouveau internal user.

Je n'ai pas d'application avec ce nouveau user.  Je me logue à nouveau sous admin pour installer des apps.

Project : Activate. 

Je vois apparaitre Discuss et Project. Dans Settings il a maintenant :
 
- General settings: on a une interface qui permet d'inviter de nouveau user. 
- Project

Avec le nouveau user j'ai :

- Discuss
- Project
- Apps

Post install configuration de odoo
##################################

Configuration du server de mail
*******************************

Je sais que le SMTP n'est pas configuré car je n'ai pas de mail si j'ai oublié mon mot de passe. 

settings / general settings / Custom email servers

J'ai utilisé mon server de mail à ophtalmologie.org. Le mail de Admin doit etre une adresse mail de ophtalmologie.org. Je rajoute

- odoo16@ophtalmologie.org dans les alias de mail sur hostinger. 
- bounce@ophtalmologie.org dans les alias sur hostinger et cela fonctionnne

Pour une invitation par mail je recois le mail 

.. image:: icono/mailinvitation.png
   :width: 800
   :alt: Alternative text

Installation du module sales
############################

TODO
####

Installation et creation du virtualenv


install pip3 et virtualenv::
  
   apt install python3-pip python3-venv

Il y a python3-venv et python3-virtualenv. Quelle différence? Depuis python 3.3 venv remplace virtualenv.


Creation du virtualenv et installation des requirements.

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

:: 

   deactivate
   which python # on verifie que l'on est bien retourné à la verison systeme de python
   python --version  # on verifie.



   
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

