# Arborescence de développement
Elle est identique à celle de production pour pouvoir développer des scripts sh qui automatisent les taches.  
En plus on a une arborescence de dev  

## Arborescence de dev

- /opt/ODOO/
    - odoo7
        - odoo7 (git repo officiel de odoo)
        - oph_odoo7 (git repo uniquement le dev de l'ophtalmologie)
        - filestorage7
    - odoo8
        - odoo8 (git repo officiel de odoo)
        - oph_odoo8 (git repo du dev de l'ophtalmologie)
        - filestorage8
    - filestorage TODO est-ce que le même filestorage peut etre utiliser par les différentes verions d'odoo?

## Arborescence de production 
- /opt/odoo
    - odoo7
        - odoo7 (git repo)
        - oph_odoo7 (git repo de l'ophtalmologie)
        - filestorage7
    - odoo8
        - odoo8 (git repo)
        - oph_odoo8 (git repo de l'ophtalmologie)
        - filestorage8
    - filestorage TODO est-ce que le même filestorage peut etre utiliser par les différentes verions d'odoo?
    

# Install the odoo on your local dev machine

- 1 make a fork of odoo to git@github.com:frouty/odoo.git in github.com

- 1 clone a version of odoo in your local machine
    - `git clone git@github.com:frouty/odoo.git --branch 8.0 odoo80`

so I get an odoo80 dir with the branch 8.0 of odoo

# How to update the local odoo filesystem  from main odoo repository
Add a path to the original forked repository  and name it upstream.  
To get the url of your main repository go to you fork repository on github web, look for "forked from odoo/odoo" click on the link.On the new page: "clone or download" copy/paste that url.
- 1 cd path/to/odoo8
- 2 `git remote add upstream   git@github.com:odoo/odoo.git` 

Check

`git remote -v`
 
you should see your origin and upstream remotes

Now sync your fork:
* fetch the branches from the upstream repository.
    - `git fetch upstream  <branch name> ie odoo8.0`
    * prend un temps fou.
    * pas sur que l'on peut fether juste une branch    
* checkout to the branch your interested
    - `git checkout -b branch.locale.qui.nous.interess upstream/branch_name`
     

* merge the changes from upstream/[branch you're interested]
* now update the fork on github

```
git fetch upstream 
git checkout [branch you're interested]
git merge upstream/[branch you're interested]
git push origin
```
## Configuration de eclipse
* Run configurations
* Name : choose a name you like
* Project : choose the project you want
* Main Module : browse to project. Choose **odoo.py**
* Arguments : --addons-path addons
* Apply


# Comment lancer plusieurs instances d'odoo
python odoo.py --xmlrpc-port=8070
jamais testé/


# Apres la mise à jour vers debian 9 
j'ai un probleme de dependance.  
report_aeroo a besoin de python-uno 2.7 qui n'est plus supporte

https://www.odoo.com/forum/help-1/question/still-not-able-to-get-module-uno-python-2-7-69405

## j'essaie 
sudo apt-get install libreoffice python-genshi python-cairo python-lxml python-setuptools
sudo apt-get install libreoffice-script-provider-python
easy_install uno
me dit que 
% easy_install uno
Searching for uno
Best match: uno 0.3.3
Processing uno-0.3.3-py2.7.egg
uno 0.3.3 is already the active version in easy-install.pth

Using /usr/local/lib/python2.7/dist-packages/uno-0.3.3-py2.7.egg
Processing dependencies for uno
Finished processing dependencies for uno

et effectivement cela ne regle pas le problem. 

Je pense qu'il faut downgrader libreoffice.

## j'abandonne l'idée de faire marcher la V7 sur debian 9.

# Debugging 
 `import pdb;pdb.set_trace()`
 
# je cherche la valeur de ref

`
<field name="inherit_id" **ref**="base.view_partner_form"/\> 
`

mode debug --> Edit form view --> External ID == ref


```
            <data>
            <xpath expr="//notebook/page[@string='Journals']" position="replace">
            </xpath>
            <xpath expr="//field[@name='target_move']" position="after">
                <field name="display_account"/>
                <newline/>
            </xpath>
            </data>
</field>
```
# Comment supprimer le "create and edit"
Dans un champ : many2one
le widget ressemble à celui d'un champ selection

on peut supprimer le "create and edit" avec :

```xml
<xpath expr="//field[@name='title']" position="attributes">
                        <attribute name="widget">selection</attribute><!-- pour éliminer "create et modifier"-->
                        <!--<attribute name="required">True</attribute>NEMARCHEPAS -->                
</xpath>
```

# Comment vérifier un champ et afficher une pop up?

voir la méthode : 
```python
def check_partners_email
```    
le module base_calendar n'existe pas dans la 8.0  

# Le format des dates dans Odoo
On trouve le format des dates dans : openerp/tools/misc.py
On a les constantes :

```python
DEFAULT_SERVER_DATE_FORMAT = "%Y-%m-%d"  
DEFAULT_SERVER_TIME_FORMAT = "%H:%M:%S"
DEFAULT_SERVER_DATETIME_FORMAT = "%s %s" % (DEFAULT_SERVER_DATE_FORMAT,DEFAULT_SERVER_TIME_FORMAT)
```

Comment on appelle ces constantes: 

from openerp.tools import DEFAULT_SERVER_DATE_FORMAT, DEFAULT_SERVER_DATETIME_FORMAT


# calendar.event
start fields.function(_compute, .......required = True....)  
stop fields.function(_compute, ...... required = True)  

partner_id Partner many2one  
partner_ids Attendees many2many  

rrule : recurrent rule  

# le logging
Je vais essayer le logging au lieu de mettre des prints

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)
```

aprés cela s'utilise comme cela :
~~~python
logger.info('blablabla')
logger.debug('Records: %s', records)
logger.info('updating records')
~~~

Il y a plusieurs niveaux de logging:  
- info  
- debug  
- warning  
- error  
- critical  


## logger.setLevel()
Spécifie le plus bas niveau de messages de log qui sera traité. DEBUG est le plus bas niveau et critical le plus haut. EG si le level est INFO, les INFO, WARNING, ERROR, CRITICAL seront traitées et DEBUG ignorées..


# Champ selection et default
Qd on a un champ selection avec comme selection la liste de tuple: 
~~~python
STATE_SELECTION = [
        ('needsAction', 'Needs Action'),
        ('tentative', 'Uncertain'),
        ('declined', 'Declined'),
        ('accepted', 'Accepted'),
    ]
    
'state': fields.selection(STATE_SELECTION, 'Status', readonly=True, help="Status of the attendee's participation"),    
~~~
On peut utiliser *default* comme cela:
~~~python
  _defaults = {
        'state': 'needsAction',
    }
~~~
  on passe donc la key
  
# Method overriding - Super

~~~python
class Parent(object):
	def __init__(self):
		self.value = 5
	def get_value(self):
		return self.value
		
class Child(object):
	def get_value(self):
		return self.value +1
~~~

On réécrit tout simplement la méthode avec le meme nom.

Qd on surcharge une méthode il faut se demander:

- si l'on veut filtrer les arguments : pre-traitement  
- si l'on veut filtrer les résultats : post-traitement.  
- ou les deux  

* prefiltering
~~~python
import datetime

class Logger(object):
    def log(self, message):
        print message

class TimestampLogger(Logger):
    def log(self, message):
        message = "{ts} {msg}".format(ts=datetime.datetime.now().isoformat(),
                                      msg=message)
        super(TimestampLogger, self).log(message)
~~~

* post-processing

~~~python
import os

class FileCat(object):
    def cat(self, filepath):
        f = file(filepath)
        lines = f.readlines()
        f.close()
        return lines

class FileCatNoEmpty(FileCat):
    def cat(self, filepath):
        lines = super(FileCatNoEmpty, self).cat(filepath)
        nonempty_lines = [l for l in lines if l != '\n']
        return nonempty_lines 
~~~

La syntaxe c'est:
`super(NomChildClass, self).nomdelafonctionparent(args)`


### model_obj.read(cr,uid,ids(liste des records à lire), [liste des champs à lire],context=context)

return une liste de dictionnaire de la forme:

[{'start_datetime': False, 'start_date': '2016-05-30', 'id': 1}, {'start_datetime': '2016-05-29 21:00:00', 'start_date': False, 'id': 13}]


### model_obj.browse(cr,uid,[liste des id des records]ids,context=context)

return in iterable 
for record in browse_obj:
	record.fieldname1.id
	record.fieldname2
	.... 

	
### pour récupérer un model

`self.pool[nom_ du_model]`


# Report

pour créer un report sur un model particulier il faut créer:

- Ce report
- report template

Si on veut accéder à plusieurs models il faut créer un custom reports class qui nous acces �� plusieurs modeles dans le template.

Il y a un fichier  addons/views/report_invoice.xml qui contient les templates.

Il y a un fichier addons/account_report.xml avec
~~~xml
 <report 
	id= ...
	model=...
	string=...
	xml='account/report/....'
	xsl='
	/>
~~~
# Liens sur les reports

- https://www.odoo.com/documentation/8.0/reference/qweb.html  
- https://www.odoo.com/documentation/8.0/reference/reports.html  

- http://www.solibre.info/odoo_doc/_build/html/howtos/backend.html

# Buttons
- http://www.slideshare.net/openobject/odoo-smart-buttons

~~~xml
<button string="Opportunities"..
		name="..."  
		context="..."..
		class="oe_inline oe_stat_button"..
		type="action"/>
~~~
Avec cela on a un petit carré avec la "string"..

Pour rajouter une petite icone.  
~~~xml
<button string="Opportunities"..
		name="..."  
		context="..."..
		icon="fa-star"..
		class="oe_inline oe_stat_button"..
		type="action"/>
~~~
pour voir les petites icons: ..
- http://fontawesome.io/cheatsheet/..

# syntaxe
~~~xml
<button>
tout ce qu'on veut
</button>
~~~
On peut mettre du html..
~~~xml
<button  class=".." name=".." icon="fa-star" type="..">
<p>bonjour</p>
</button>
~~~
On aura un cadre avec marqué bonjour.  

On peut mettre du html et du dynamique:  
~~~xml
<button class=".." name=".." icon="fa-star" type="..">
 <span><field name="opportunity_count"/>Opportunities</span>
~~~

Pour un champ one2many. Si on veut compter le nombre de phone calls.    
phonecall_ids in res.partner  
1- Ajouter un champ fonctionnel phonecall_count to res.partner  
2- Ajouter attrs widget="statinfo"
~~~xml
<button class=".." name=".." icon="fa-star" type=".." context="..">
 <field string="Calls" name="phonecall_count" widget="statinfo"/>
~~~
Comment on fait ce comptage:
~~~python 
def _phonecall_count(self, cr, uid, ids, field_name, arg, context=None):
        res = {}
        for partner in self.browse(cr, uid, ids, context):
            res[partner.id] = len(partner.phonecall_ids)
        return res
~~~
On est dans l'objet res.partner...
field_name retourne : string 'phonecall_count' qui est le nom du champ function :  
~~~python
 'phonecall_count': fields.function( _phonecall_count, string = "Phonecalls", type = "integer" ),
~~~
arg est None  
self.browse(cr,uid,ids,context) est un itérable avec tous les records de res.partner correspondants à liste:ids  
Donc il faut itérer sur cet objet.  
partner.phonecall_ids on appelle le champ phonecall_ids de l'objet res.parner et on récupere un itérable de crm.phonecall (c'est défini dans 'phonecall_ids': fields.one2many( 'crm.phonecall', 'partner_id', 'Phonecalls' ),)  
On récupere un objet qui contient tous les records phonecall.crm de res.partner correspondant à l'id.

Dans la méthode _opportunity_meeting_phonecall_count:..
~~~python
res = dict( map( lambda x: ( x, {'opportunity_count': 0, 'meeting_count': 0} ), ids ) )
~~~
retourne (si ids=[1,2,3,4])  
~~~python
{1: {'meeting_count': 0, 'opportunity_count': 0},
 2: {'meeting_count': 0, 'opportunity_count': 0},
 3: {'meeting_count': 0, 'opportunity_count': 0},
 4: {'meeting_count': 0, 'opportunity_count': 0}}
~~~


# Views
- https://www.odoo.com/documentation/8.0/reference/views.html

# update a module.

 What you should do is

Update the module that you want to upgrade in the addons folder
Restart the Odoo-server service (unfortunately this is still necessary as python need to create the compiled version of the py files).
If you have new modules, click the Setting >> Modules >> Update Module List menu and follow through
    Then from Setting >> Modules >> Installed Modules, select the modules you just updated, and click on Upgrade button.

Note that 2 is required if you change any of the py files.  If you don't change py files, you can skip it.  Yes, some error may occurs if you radically change the structure of model in the update module.  Hence, it is better not to remove any existing fields or change their type.

Note on 4: if you don't change any data files (XML or CSV) included in the module and you don't change any model structure (no fields or defaults or sql_constraint or constraint [etc....] added/removed/modified) and only change method implementation, you may get away without doing 4.

Additional Note: if you define a data record in the old version of the module and you don't require it anymore int he new version of the module, add a <delete model="model_id" id="xml_id_of_record_to_delete"/> into the XML file to properly remove the data.

# libreoffice - fields. 
Sous libreoffice 6.1.5.2 la mise en place des fields dans les templates semble etre différente.
## j'essaie
l'icone avec des lignes et un carré bleu en bas à droite / More fields / 
On retrouve plusieurs tab : document / Cross references / Functions / DocInformations / Variables / Database.

Functions / Input field / Refence le code qui va bien  / Insert / 

# les attachments 
/opt/odoo/odoo7-server/openerp/filestore pointe vers /var/odooattachment
on y trouve un repertoire par base de donnée. 
Qd on crée une nouvelle base de donnée le repertoires des attachments n'est pas créer.
Il faut utiliser l'arborescence de la base de donnée source et en faire une copie avec le meme nom que la nouvelle base de données.
C'est pas sur j'ai fait une nouvelle base de donnée en production et je retrouve les attachments meme les anciens. 
