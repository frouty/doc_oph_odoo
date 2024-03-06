Installation et mise en route du server ODDO
############################################

installation de ohmyzsh
***********************


faire google search : https://ohmyz.sh/

apt install zsh mate wget

installation des fonts
======================

Allez sur https://github.com/ryanoasis/nerd-fonts/releases/tag/v2.1.0

choisir un fichier

wget ce fichier zip

unzip le zip telechargé.

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
   - /opt/odoo/odoo16
Customs addons seront installés dans:
   - /opt/odoo/custom-odoo16....

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

Create a log directory and set permissions
------------------------------------------
::

   sudo mkdir /var/log/odoo16
   sudo chown odoo16:root /var/log/odoo16

Running ODOO in virtual environnement
*************************************

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

Pour faire cela au travers de ssh il va falloir mettre en place la clefs publique du server odoo  dans **github** 

Creation d'une paire de clef sur le server odoo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Copie de la clef publique sur github
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Download Odoo depuis github
^^^^^^^^^^^^^^^^^^^^^^^^^^^


::

   mkdir -p /opt/odoo/odoo16
   sudo git clone -b 16.0 https://github.com/odoo/odoo.git /opt/odoo/odoo16
   sudo chown -hR root:odoo /opt/odoo/odoo16  <-- root:odoo ou odoo16:root

Création d'un directory pour les customs addons

::

   mkdir /opt/odoo/custom-odoo16
   chown -hR odoo16:root /opt/odoo/custom-odoo16
   

Installation des prerequis pour odoo16
--------------------------------------

::

   sudo su - odoo16 -s /bin/bash
   source /python-venv/3.10/odoo16/bin/activate
   pip install wheel
   pip install -r /opt/odoo/odoo16/requirements.txt
   pip install pylibdmtx
   
   wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
   sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb
   Je bloque car me demande le password du user odoo16
   Donc finalement je ne fait pas l'install du deb wkhtmltopdf comme cela. Mais tout simplement en user root. voir ci dessus.


Démarrage de odoo 16 en post install immédiat
*********************************************

Je test en restant dans le virtual env.
   
   ::

      /opt/odoo/odoo16/odoo-bin

Je me connecte http://IP_server:8069 

master password proposé : tmqw-ak6n-3xyh

password gkvnFWsvjH5x3Kh

Odoo service file
=================

est ce qu'il faut le mettre en oeuvre quand on travaill en virtual env?

::

   nano /etc/systemd/system/odoo16.service
   
   [Unit]
   Description=Odoo16
   Documentation=http://www.odoo.com
   [Service]
   # Ubuntu/Debian convention:
   Type=simple
   User=odoo16
   ExecStart=/opt/odoo/odoo16/odoo-bin -c /etc/odoo16.conf
   [Install]
   WantedBy=default.target

Set permissions ::

   sudo chmod 755 /etc/systemd/system/odoo16.service
   sudo chown root: /etc/systemd/system/odoo16.service

Furthermore, the installation of Odoo was successful if it is shown as active ::

   sudo systemctl status odoo16.service

Finally, use the following command to start the Odoo service automatically after restarting the server ::
   
   sudo systemctl enable odoo16.service

Use the following command to restart the Odoo service if you have made any modifications to the add-ons so that your instance will reflect the updates ::

   sudo systemctl restart odoo16.service


Post install configuration de odoo
##################################

je me connecte avec mon adress mail et je suis en administrateur. 

Il n'y pas grand chose dans l'interface : discuss, calendar, Contacts, Apps, Settings. 


Configuration du server de mail
*******************************

Je sais que le SMTP n'est pas configuré car je n'ai pas de mail si j'ai oublié mon mot de passe. 

settings / general settings / Custom email servers

J'ai utilisé mon server de mail à ophtalmologie.org. Le mail de Admin doit etre une adresse mail de ophtalmologie.org.

Le server outgoing me dit ok lors du test de connection mais lorsque j'essaie d'inviter new user cela ne fonctionne pas. Pas de message d'erreur. Pas de mail chez le destinataire.

TODO 

Installation du module sales
****************************

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

