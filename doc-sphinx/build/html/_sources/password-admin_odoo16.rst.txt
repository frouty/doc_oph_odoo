Les password dans odoo
######################
Il y a trois password dans odoo:

   - 1 db_password pour se connecter à la base de données, à postgresql.
   - 2 admin_password ou Master password pour gérer toutes les bases de données.
   - 3 le password qu'un user a pour administrer sa base de données. C'est stocké dans la base de donnée.

Pour voir les admin password:

* sudo -sHu postgres
* psql -d unedatabasedeodoo
* SELECT login, password FROM res_users;


Creation d'une database
#######################

A la creation de la database il est demandé le Master password c'est admin_passwd =  que l'on trouve dans le fichier de conf: (/etc/odoo/odoo7-server.conf)

**Choose a password** on prend ce que l'on veut. Il nous sera demandé pour ouvrir la nouvelle base de données qui vient d'être créée.

Probleme de password avec le user admin
---------------------------------------

Apres avoir installé odoo qd tu rentres l'email du user admin tu peux te loguer avec cet email

Perte du password admin
-----------------------

::

   pip install passlib
   python3
      from passlib.context import CryptContext
      CryptContext(['pbkdf2_sha512']).encrypt('fQ8xxoSs$SE!jJ')
      # copier la sortie dans le press papier
      exit()
    
   sudo -sHu postgres
   psql -l # liste toutes les databases
   psql -d <database name>  # on se connecte à la database
   <database name>=#select id, login, password from res_users; #liste les users.
   #  REPLACE """HASH"""" by the result of the previous command

    <database name>=# UPDATE res_users SET password='', password_crypt='< le hash copié dans le press papier' WHERE id=1;
   
  Mais dans ma base de donnée le user id 1 s'appelle __system__ du coup  je ne fais rien.  J'installe une nouvelle database. A la création de la database on choisi un master password et un password.  