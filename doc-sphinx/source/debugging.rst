Debugging
#########
 :: 
 
   import pdb;pdb.set_trace()

On le place où on veut dans le code python
Et ensuite on lance le server en mode console ::

   service odoo-server stop
   /opt/odoo/odoo7-server/openerp-server -c /path/to/config/file