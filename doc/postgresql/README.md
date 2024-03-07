# Comment changer le password d'un role?

Cela peut etre interessant quand on arrive plus à se connecter avec pgadmin.  
ALTER rolename PASSWORD 'a string';  

ou 
```
su
su postgres
psql
\password postgres
Saisissez le nouveau mot de passe :
Saisissez-le à nouveau :
```

# psql 
psql est le terminal interactif pour travailler avec postgresql.

# Comment retrouver le nom des databases?
su postgres
pgsql 
SELECT datname FROM pg_database;

# lister toutes les databases

\l

# Lister les tables d'une database en cours

\dt

# Lister les users

\du

# Comment vérifier que postgresql est en route
invoke-rc.d postgresql status
Running clusters: 9.1/main 

ou
service postgresql status
ou

lsof -i :5432
COMMAND   PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
postgres 2585 postgres    3u  IPv4   8037      0t0  TCP localhost:postgresql (LISTEN)

le : avant le port est important.

# Comment lister les users dans postgresql.

\du

# Comment quitter postgresql?

\q


# comment supprimer un role 

DROP ROLE lenomdurole;

# Comment voir les log de postgresql

tail -f  /var/log/postgresql/postgresql-9.4-main.log
