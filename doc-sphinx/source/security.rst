Droit d'accés
=============

Dans le dossier security du module on crée :

    - 1  ir.model.access.csv  qui va définir les controle d'acces pour le module
        - id identifier unique de son choix
        - Name: description 
        - model_id:id
        - group_id:id : nom unique pour le groupe
        - perm_read : 1 pour le droit d'écrire, 0 pour non
        - perm_write
        - perm_create
        - perm_unlink delete acces or not.
    - 2 création d'un  fichier xml dans le dossier security. C'est dans ce fichier que l'on va définir l'id du group qui l'on utilise dans le fichier ir.model.access.csv