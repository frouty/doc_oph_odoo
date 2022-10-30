Invitation email
################
Annuler les invitations lors de la création d'un rendez vous
************************************************************

Customer/Sales and purchase / opt-out coché ne marche pas. 

Settings / Technical / Parameters / system parameter / calendar.block_mail True ne marche pas
Settings / Technical / Parameters / system parameter / calendar.block_mail True 0 records ne marche pas

J'ai ajouté ir.config_parameter dans les group human/ressources et doctors mais cela ne marche pas. 
J'ai restart le service ne marche pas 
Il faudra chercher def _send_mail_to_attendees n'existe pas sous 7.0
J'ai trouvé en faisant une recherche sur les mots du message dans le mail :
addons/base_calendar/base_calendar.py la méthode _send_mail de la class calendar_attendee que je vais surcharger pour qu'elle ne crée pas d'enregistrement dans mail.mail.



pour la périodicité des mails.
******************************

Technichal / scheduler / scheduled action / email Queue manager.