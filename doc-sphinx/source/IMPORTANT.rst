ATTENTION
=========
La machine de dev doit pouvoir se connecter au RT5100 
pour cela il faut que le vpn soit opérationnel.

 $ systemctl start  openvpn-client@ODOO7_SPARE_DEV.service

Si le VPN n'est pas actif le message d'erreur n'est pas du tout évident on just un couldn't load module oph.