========================
 Les password dans odoo
========================
Il y a trois password dans odoo:

1 db_password pour se connecter à la base de données, a postgresql.
2 admin_password ou Master password pour gérer toutes les bases de données.
3 le password qu'un user a pour administrer sa base de données. C'est stocké dans la base de donnée.

Pour voir les admin password:

* sudo -sHu postgres
* psql -d unedatabasedeodoo
* SELECT login, password FROM res_users;


=========================
 Creation d'une database
=========================

A la creation de la database il est demandé le Master password c'est admin_passwd =  que l'on trouve dans le fichier de conf: (/etc/odoo/odoo7-server.conf)

**Choose a password** on prend ce que l'on veut. Il nous sera demandé pour ouvrir la nouvelle base de données qui vient d'être créée.
