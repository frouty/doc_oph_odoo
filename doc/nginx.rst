=======
 nginx
=======

===============
 Enable a site
===============

sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/

Test de la config : nginx -t


================
 Disable a site
================

rm /etc/sites-enabled/odoo

C'est ce que j'ai fait. Et maintenant odoo fonctionne. Il semble qu'il ne faille pas configurer de site-enable pour que cela marche.
Dans la machine de prod le sites-enabled odoo pointe vers une IP qui n'existe pas. Donc je pense que nginx ne sert pas.
