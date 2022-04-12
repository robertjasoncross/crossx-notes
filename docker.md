Docker


*To reference a Postgres DB backup on the local hardrive for restoring into a container database you must use bash on the command line to execute a string command that references the container context.*
``` *.\sh-session
docker compose -f local.yml exec db /bin/sh -c "PGPASSWORD=postgres psql --username postgres --host localhost archfndr" < ./db/backups/af_latest_no_geo.sql
```

*If you try and access the local disk from a bash shell in the container, the file will not be available as it is not in the container context. So, instead wou would want to copy the file over into the container context in the Dockerfile. Then, a bash shell within the container will be able to view the file wherever it has been copied to* 

*This command would fail with authorization as the user is not in the container where the database is. The postgres user is in the wrong context.
``` *.\sh-session
docker compose -f local.yml exec db psql -U postgres -h localhost database_name < database_backup.sql
```

* Accessing a shell in the ccontainer context will only allow you to see the files you have copied over as defined in the Dockerfile
``` *.\sh-session
docker compose -f local.yml exec db /bin/sh
```
