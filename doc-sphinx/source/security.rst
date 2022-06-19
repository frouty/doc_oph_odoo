Droit d'accés
=============

Dans le dossier security du module on crée :

    - 1  ir.model.access.csv  qui va définir les controle d'acces pour le module
        - id identifier unique de son choix, ce qu'on veut
        - Name: description ce qu'on veut
        - model_id:id syntax model_nom_de_la_class du model
        - group_id:id : nom unique pour le groupe oph.iddugroup que l'on défini dans le xml sous security. Pour notre module oph.id dans le xml
        - perm_read : 1 pour le droit de lire , 0 pour non
        - perm_write
        - perm_create
        - perm_unlink delete acces or not.
        
    - 2 création d'un  fichier xml dans le dossier security. C'est dans ce fichier que l'on va définir 
    l'id du group que l'on utilise dans le fichier ir.model.access.csv
    
    Il faut aussi mettre ir.model.access.csv dans __openerp__