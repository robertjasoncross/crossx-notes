#### Docker - Digital Ocean - Bash - Postgres - Numpy - Python - Jupyter - ReactJS ####

### Docker - Postgres
**Undestanding the container context is key when executing commands locally and in the containers**

* *If you try and access the local disk from a bash shell in the container, the file will not be available as it is not in the container context. So, instead wou would want to copy the file over into the container context in the Dockerfile. Then, a bash shell within the container will be able to view the file wherever it has been copied to.*

* *To reference a Postgres DB backup on the local hardrive for restoring into a container database you must use bash on the command line and execute a string command that references the container context while giving local context (not within the container) file reference.*


``` *.\sh-session
docker compose -f local.yml exec db /bin/sh -c "PGPASSWORD=postgres psql --username postgres --host localhost archfndr" < ./db/backups/af_latest_no_geo.sql
```
* *If you would rather execture the commands within the container then the files on the local disk must be ccopies into the container in the Dockerfile, otherwise they will not be seen in the container context*


```Dockerfile
ADD /a_backup.sql /db/backups/a_backup.sql
```


* *This command would fail with authorization as the user is not in the container where the database is. The postgres user is in the wrong context. Although, in any context specific scenario this should work just fine*
``` *.\sh-session
docker compose -f local.yml exec db psql -U postgres -h localhost database_name < database_backup.sql
```

*Accessing a shell in the ccontainer context will only allow you to see the files you have copied over as defined in the Dockerfile. So, psql in the container context will only see those files that were copied there per the Dockerfile *
``` *.\sh-session
docker compose -f local.yml exec db /bin/sh
```

### Numpy ###

*Possible Errors*
- OpenBLAS errors possibly caused by errors with libraries
- Some scenarios of possible conflicting libraries iwht others like SciPy 

*Numpy can cause a lot of problems in certain Docker scenarios and I found this information on the Numpy site to be helpful*

https://numpy.org/install/

https://numpy.org/doc/stable/user/troubleshooting-importerror.html

NumPy packages & accelerated linear algebra libraries

NumPy doesn’t depend on any other Python packages, however, it does depend on an accelerated linear algebra library - typically Intel MKL or OpenBLAS. Users don’t have to worry about installing those (they’re automatically included in all NumPy install methods). Power users may still want to know the details, because the used BLAS can affect performance, behavior and size on disk:


The NumPy wheels on PyPI, which is what pip installs, are built with OpenBLAS. The OpenBLAS libraries are included in the wheel. This makes the wheel larger, and if a user installs (for example) SciPy as well, they will now have two copies of OpenBLAS on disk.

### Bash ###
*Shells*

Ubuntu /bin/bash

Alpine /bin/sh
