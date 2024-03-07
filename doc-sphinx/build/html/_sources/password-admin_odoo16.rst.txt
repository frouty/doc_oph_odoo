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