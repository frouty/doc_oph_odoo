shell
#####

::
   
    shell  -c odoo.conf -u oph_odoo16 -d odoo16-01
    
et on peut travailler au niveau ORM ::

   records = env['res.partner'].search([('gender','=','False')])
   len(records)
   