#Comment changer le password d'un role?

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

# Comment retrouver le nom des databases?
su postgres
pgsql 
SELECT datname FROM pg_database;

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