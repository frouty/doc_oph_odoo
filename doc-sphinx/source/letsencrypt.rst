Avec l'update des différents client web: chrome, ils refusent de se connecter sur le site ODDO. Je n'ai pas tres bien compris a quoi cela peut etre du. 

Je me demande si ce n'est pas un probleme de certificat. J'envisage d'utiliser letsencrypt. Mais je ne sais pas si cela le probleme.

Attention je fais cela sur une machine de test car letsencrypt install du python3 et je ne veux pas tout casser sur la machine en prod.

 - 1 je mets en place nginx sur une machine de test
    - sur le router je mets une Port forward rule : protocole tcp udp, source zone wan et wan6, external port 443, destination lan, internal IP du server, internal port vide
 - 2 https://kuendu.ddns.net j'ai comme d'habitude avec firefox le msg "Potential Security Risk Ahead"  et MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT
accepts the risk and continue et c'est bon.
- 3 j'ai aussi eu secure connection failed  Error code: SSL_ERROR_NO_CYPHER_OVERLAP. Je n'ai plus le bouton pour accepter les risques. C'était lié au fait que j'avais mal configureé nginx en mettant TLSv1.3 qui n'est pas supporté par la verions de nginx sur la machine de prod 
 - 4 avec chrome j'ai This site can’t provide a secure connectionkuendu.ddns.net uses an unsupported protocol.
ERR_SSL_VERSION_OR_CIPHER_MISMATCH
- 5 j'ai fait quelques modifications dans le fichier /etc/nginx/site-enable/openerp. Dans Chrome j'ai un nouveau message d'erreur:  NET:ERR_CERT_AUTHORITY_INVALID. j'ai un bouton advanced et là je peux poursuivre sur le site.


Depuis Firefox 78 le minimu TLS est TLS 1.2. Ma version de nginx (1.6) ne supporte pas le TLSv1.3