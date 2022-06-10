relational fields
=================

https://www.cybrosys.com/blog/relational-fields-in-odoo

one2one
~~~~~~~
use many2one

many2one
~~~~~~~~

class ChildModel(models.Model):
        _name = 'child.model'
        
        child_field_1 = fields.Char(string="Child Field 1")
        child_field_2 = fields.Integer(string="Child Field 2")
        child_field_3 = fields.Boolean(string="Child Field 3")
        parent_field_id = fields.Many2one('parent.model', string="Parent ID")
        
one2many
~~~~~~~~

.. code-block:: python

    from odoo import models, fields
    
    
    class ParentModel(models.Model):
        _name = 'parent.model'
        
        parent_field_1 = fields.Char(string="Parent Field 1")
        parent_field_2 = fields.Integer(string="Parent Field 2")
        
        # If you want to display child data, you must create One2many field.
        # The One2many field used for inverse field from Many2one field
        child_field_ids = fields.One2many('child.model', 'parent_field_id', string="Child IDS")
    
    
    class ChildModel(models.Model):
        _name = 'child.model'
        
        child_field_1 = fields.Char(string="Child Field 1")
        child_field_2 = fields.Integer(string="Child Field 2")
        child_field_3 = fields.Boolean(string="Child Field 3")
        parent_field_id = fields.Many2one('parent.model', string="Parent ID")    
 
Il faut créer le parent_field_id en many2one
       
many2many
~~~~~~~~~   

 On est sur un model est on veut se relier à un autre model comodel
 
 field_ids = fields.many2many('comodel.name', String='Field Name')
 
  - comodel_name : nom du comodel avec lequel on veut creer une relation
  - relation c'est un nom optionel pour la table qui va stocker la relation dans la database.
  - column1 c'est un nom optionel qui se refere aux records de notre model dans la table relation
  - column2 c'est un nom optionel qui se refere aux records de notre comodel dans la table relation
  
  field_ids = fields.many2many('comodel.name', 'rel_name', 'model_id','comodel_id', string='AString')
  
  Si on ne fournit pas relation column1 et column2 les noms sont automatiquement crée par le systeme.
  