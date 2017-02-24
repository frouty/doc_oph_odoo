- [ ] Je voudrais dans la vue oph_agenda_factory pour les responsable n'afficher que un group?
Ici ce serait le group doctor.

----
CATEG_ids et tag c'est la meme chose le mieux serait d'utiliser categ_ids qui existe.
state dans calendar_event.il n'y a que 2 states possible comment je peux en rajouter?
ou encore dans un champ selection comment rajouter des items.
-------

calendar_event field tag c'est pour cs / tech /close ....on pourrait le remplacer par categ_ids
state c'est pour draft/open/busy/in/wait/out...




- [ ] Comment faire pour avoir des propositions quand je tape les premieres lettres du nom d'un patient dans la vue crm_meeting tree?


- [ ] Dans la kanban view des calendar.event je voudrais voir le champ : "fullname" "meeting subject" et "age".
Pas besoin d'afficher la date de naissance dans la kanban calendar view mais reste utile dans la 
vue tree et form.

- [ ] Dans la kanban de calendar.event je voudrais voir aussi le champ "meeting subject" uniquement si cela ne rend pas la vue illisible car trop d'informations.

- [ ] Dans la kanban l'idéal serait: fullname(age):name du meeting




- [ ] sales - customer meeting - est ce que je peux avoir l'ouverture de la vue calendar kanban avec un filter sur uniquement les rendez vous disponible (pour la secretaire) et sur tous les rendez vous pour les medecins
(les medecins par contre doivent voir les crenaux libres, occupés pas les cancel) pour pouvoir decider ou non de forcer)

- [ ] les crenaux cancel doivent apparaitre qd on fait une tree view par partner. tree view partner on doit voir tous les crenaux quelques soit leur statut.

- [x]  Sales : le champ date de naissance reste requis meme si collegue.

		- ca marche maintenant  mais je ne sais pas pourquoi cela ne marchait pas.

- [ ] Sales si je cree une company (is a company) - save -the following fields are invalide date of birth. Il faut decocher "customer". Bon c'est pas terrible. Comment mettre une methode qui 
passe "customer" à False . Mais ce n'est pas là que l'on rentre les multicompany.

- [ ] Comment cela se passe maintenant pour les reports?  On peut voir cela avec le wizard : oph_etat_factory.


- [x] Dans la vue res.partner - create j'ai tout de suite la pop up choose a gender.
		- SOLVE

- [x] Inject ne marche pas : SOLVED

- [x] Pas de vue tree des rendez vous. Comment l'obtenir: SOLVED

- [x] La methode on_change gender ne marche pas:  SOLVED

- [x] Bug dans la vue calendar form pas moyen de mettre de tono, kerato, slit lamp exam......
bug dans la vue calendar form quand je veux ajouter des ..._ids. (tono_ids, protocole_ids...) c'est un probleme dans in_compute.
SOLVE

- [ ] Dans la vue res.partner - create -save pourquoi j'ai deux fois nom et prenom.
J'ai Nom Prenom et Fullname

- [x] Comment on fait pour que certains champs s'affichent ou pas si Edit ou pas Edit.
pour qu'un champ ne s'affiche pas dans une form view mais uniquement en mode edit **class="oe_edit_only"**
DONE


- [x] **sale - partner - meeting** j'ai un filtre qui colle : attendee 6 et donc il faut l'enlever pour voir les crénaux. Solved


# dans oph_agenda_factory

On peut aussi rendre invisible un champ en fonction d'une valeur d'un autre champ:

`<field name="discount" attrs="{'invisible':[('price_unit','=',0)]}" groups="sale.group_discount_per_so_line"/>`