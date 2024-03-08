Emailing in odoo
################

Regarder Ophtalmology/Request, Ophtalmology/IOL Order

Regarder Settings/Email/Template pour les templates des emails. 

Faire les modifications en base de données puis faire les tests puis mettre tout cela dans /oph_odoo/oph/edi/request_action_data.xml
C'est le fichier dans lequel on trouve les templates des mails.

Résumé de la résolution de Gros problemes
*****************************************
Mise en place de la 2FA et creation d'un app password dans google
=================================================================
Dans google account je mets en route le 2  step authentification
Dans la barre de recherche je tape "app password"
Je tombe sur la page App password 
Select app : custom je mets odoo clique generate.
copy du password
on va dans odoo general seting/outgoing email/password ce que l'on vient de copier et test de la connection : Connection test succeeded everything seems properly set up.

Le mot de passe de app password est dans le gestionnaire de mots de passe. 

Dans configuration / general setting /configure outgoing mail servers
paramétrer le server smtp avec : 
   - priority: la plus basse (donc haute priority)
   - server smtp : smtp.gmail.com
   - port smtp: 465 
   - deboggage decoche 
   - connection security SSL/TLS
   - username: email
   - password: dans le gestionnaire de password.

hostinger smtp
==============

password le password du compte email 

le login xyz@ophtalmologie.org.

le port 465 ne marche pas en swaks,

le port 587 qui marche en swaks ne marche pas en odoo. le port 587 semble marcher avec thunderbird. 

Gros problemes
==============
Je n'arrive plus à envoyer des mails. 

D'ou mise en place de la double authentification sur google mise en place d'un code avec app password.

Je n'arrive à envoyer les mails depuis odoo que sous admin

avec l'objet iol_order, sous user  en enlevant tous les abonnés j'envoie uniquement à francois par Gmail 2FA.
 Je vois que automatiquement odoo rajoute le user et le destinataire dans les abonnés.
 Le mail n'arrive pas.

 avec l'objet iol_order, sous user  en enlevant tous les abonnés j'envoie uniquement à francois 
 par mailgun
 Je vois que automatiquement odoo rajoute le user et le destinataire dans les abonnés.
 Le mail n'arrive pas.
 Je change le port to 587
 le mail n'arrive pas 
 securite de la connection je mets debogage dans odoo et je change SSL/TLS ->  TLS(starttls)
 enregister test de connection ca prend du temps contrairement à d'habitude. timeout
 securite de la connection à aucun pareil ca prend un temps fou et timeout
 
 
 Dans thunderbird si je change le smtp vers mailgun en default je n'ai pas 
 l'impression que mailgun fonctionne. Les mails arrivent aux destinataire mais rien 
 dans le log de mailgun. je me deman si cela ne passe pas par gmail.
 
 swaks
 ~~~~~
cat ~/.swaksrc
-s smtp.mailgun.com
-tls
-au postmaster@sandboxbedfc73aa96947c5b430639577c3c3a9.mailgun.org
-ap 895f7c1c5478b69ed5f1b3acb04f799c-523596d9-a57f14f2
-f francois.oph@gmail.com
-p 587

$swaks -t francois.oph@gmail.com --body 'de swak' --h-Subject 'DE SWAK 3eme'

 En CLI swaks ne marche pas timeout. J'ai mis le port à 587 et c'est bon . Les autres ports 25 et 465
 ne fonctionne pas.  
 
 Attention --h-Subject avec un grand S et --h-subject
 sinon ca envoie comme sujet test avec la date. Cela peut etre utile. 
 
 Donc j'ai configuré correctement mailgun. 
 
 
 
 En admin depuis odoo
 ~~~~~~~~~~~~~~~~~~~~
 Mailgun
 -------
 mailgun setting/email/outgoing server port 587
  SSL/TLS
  test connection --> ssl unknown protocol.
 TLS/STARTTLS test connection c'est long Gateway time out.
 Je repasse sous lfs et là pareil le meme msg d'erreur./
 
 en cli avec swaks je vois que gunmail fonctionne.
 
 peut etre un probleme  de port. gunmail ecoute sur 25-587-465
 
 J'essaie 25 dans odoo Test connection c'est long -> Time out
 *465*  -> test succeeded
 
 iol order
 #########
 
 j'enleve tous les followers
 j'envoie a un destinataire par gunmail. et rien ne se passe. 
 
 le port 25 et 465 odoo me dit ok pour le test connection mais en CLI swaks ne marche
 le port 587 marche en swaks mais test connection me dit ne marche pas.
 
 changement de l'alias domain --> smtp gunmail domain 
 port 587 dans outgoing mail server 
 iol_order email --> rien en se passe
 port 465 
 iol_order emain --> rien ne se passe. 
 port 25 
 iol_order email --> timeout
 j'essaie 2525 sans y croire. --> timeout
 
 l'email avec odoo et gunmail ne marche pas sous admin
 
 gmail en app password.
 ---------------------
 Test connection test succeeded.
 
 iol order
 #########
 
 timeout
 je repasse alias domain à localhost.
 timeout
 
 Modifications de l'alias domain
 ===============================
 J'ai trouvé ceci
 
Avatar
Amr Essam
25 juin 2013

i faced the same problem and solved it by doing the following:
Changing the alias domain in General Settings to be as my domain name smtp 
port be 2525 connection security be (TLS/STARTTLS) finally, 
the priority be less than localhost or other outgoing mail

general settings/alias Domain :loalhost -> domain name gunmail
je monte la priorité de gunmail
test connection
port 25 ne marche pas timeout.
port 465 test succeed
port 587 connection test failed. 

J'ai l'impression que cela n'a rien changé je fais un reboot.

reboot
======
En admin
~~~~~~~~
test de connection
------------------
gmail app password port 465 test succeed
gunmail port 465 test succeed. 
gunmail port 587 test failed.
gunmail port 25 test failed.


iol_order
---------
gmail app password  en admin envoie a deux recipients. OK
si je mets francois.oph@gmail.com le mail n'y arrive pas. 
si je ne mets pas d'autre récipient le mail est bien envoyer à tous les follower mais pas au user. 

En user standard
~~~~~~~~~~~~~~~~
Test connection gmail app password test succeed.
iol order responsable: francois meme que le user. email ok
voyons si francois peut envoyer un mail qd responsable n'est pasfrancois mais administrator. email ok
le responsable de l'objet ne pose pas de probleme pour l'envoie de mail.

je mets gmail app password en priority apres localhost. cela ne marche plus. Je le remets en 1. Et ok pour iol order
j'essaie pour request. Ca marche mais cela envoie aussi un mail a cabinet.goeen@gmail.com adresse qui n'existe pas .  je ne trouve pas ou j'ai mis cela.
