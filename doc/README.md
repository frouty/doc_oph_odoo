# lancement de odoo
## V8
$cd path/to/odoo/
$./openerp-server --config=/etc/pathto.conf' 
## V7
% cd /home/lof/odoo7
 % ./openerp-server --addons-path  addons
  % ./openerp-server --addons-path  addons,/home/lof/ODOO/oph_odoo7/extra-addons ne marche pas  
  % ./openerp-server --addons-path  addons,/home/lof/ODOO/oph_odoo7/extra-addons/oph,/home/lof/ODOO/oph_odoo7/extra-addons/aeroo OK  
# ressources

https://webkul.com/blog/beginner-guide-odoo-clicommand-line-interface/

–db-filter :it parameter which can use for hides databases that do not match regular expression. Cela peut etre utile pour éviter d'avoir des databases incompatibles avec la version.

# un exemple de script d'installation de odoo.

https://raw.githubusercontent.com/lukebranch/odoo-install-scripts/master/odoo-saas4/ubuntu-14-04/odoo_install.sh