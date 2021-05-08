Document attachment
===================
Storage
-------

In OpenERP v7, by default, attachments are stored in the database. You may choose to store them on the filesystem by setting an ir.config.parameter (Settings->Technical->Parameters-System parameters) named ir_attachment.location

Example if you set ir_attachment.location to file:///filestore

They will be stored in the filesystem at openerp root_path/filestore, the new system uses sha1 to generate the filename so that duplicate files don't take more space.

Cela permet de stocker les documents dans le systeme de fichier sous : /opt/odoo/odoo7-server/openerp/filestore.

On crée un lien filestore --> /var/odooattachment. Cela facile le backup de tous les documents attachés.

Sous filestore on a un dossier par base de données. 

Quand on crée une nouvelle base de données ce système de fichier n'est pas créé. Il faut le faire manuellement avec un rsync-copy.

Quand on délete une database je ne suis pas sur que lle filestore soit effacé. TODO. 

Comment faire pour que le report aeroo créé soit enregistré en attachment?
--------------------------------------------------------------------------
Settings - Technical - Aeroo - Report - Selection du report qui t'intéresse, dans sa form view Attachments Save as Attachment Prefix mettre : (object.state in ('open','paid')) and ('AT'+(object.number or '').replace('/','')+'.pdf'). J'ai pas bien compris mais bon. En plus il y le +'.pdf' mais si c'est pas un pdf on fait comment? Donc TODO.