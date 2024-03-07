one2many
########

https://gist.github.com/CakJuice/fcc3bcdb3d20457b31fdef0e56466c47

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