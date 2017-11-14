## Switching from Teamcity's HSQLDB to PostgreSQL hosted by GCP
There are a few summaries in Teamcity documents about settings and procedures. Here are the links I used:
[https://confluence.jetbrains.com/display/TCD10/Migrating+to+an+External+Database#MigratingtoanExternalDatabase-db_full](https://confluence.jetbrains.com/display/TCD10/Migrating+to+an+External+Database#MigratingtoanExternalDatabase-db_full)
[https://confluence.jetbrains.com/display/TCD10/Setting+up+an+External+Database#SettingupanExternalDatabase-PostgreSQL](https://confluence.jetbrains.com/display/TCD10/Setting+up+an+External+Database#SettingupanExternalDatabase-PostgreSQL)


### Creating a GCP Cloud SQL Postgres instance

In GCP start a new PostgreSQL isntance. Whitelist the ip address of your teamcity server instance. Make a `teamcity` database and a `teamcity` user for that database. Also, in psql execute the following command:
```sql
create schema teamcity authorization teamcity;
```

There are a few database parameters that Google Cloud SQL doesn't allow you to set. I wasn't able to set any of the Teamcity recommended settings described [here](https://confluence.jetbrains.com/pages/viewpage.action?pageId=74845225#HowTo...-ConfigureNewlyInstalledPostgreSQLServer).


### Now for updating your teamcity instance that is running in a Docker container

You must stop teamcity server from running inside your docker container while making the update. Use `docker exec -it <your-contianer> /bin/bash` to get into your container. Then you'll need to run through the following commands to update to migrate:

Stop your server and make a postgresql properties copy from the template
```bash
/opt/teamcity/bin/teamcity-server.sh stop
cp /data/teamcity_server/datadir/config/database.postgresql.properties.dist /tmp/
```

Edit the template with your PostgreSQL details:
```bash
nano /tmp/database.postgresql.properties.dist 
```

Make sure you have a jdbc driver for postgres. If you don't you'll have to download one.
```bash
ls /data/teamcity_server/datadir/lib/jdbc/
java -version
cd /data/teamcity_server/datadir/lib/jdbc/
curl https://jdbc.postgresql.org/download/postgresql-42.1.4.jar --output postgresql-42.1.4.jar
```

Run the migration script `maintainDB.sh`. Make sure there aren't any errors. If you run you start your teamcity server instance with errors in the migration you could ruin your history or your configurations.
```bash
/opt/teamcity/bin/maintainDB.sh migrate -A /data/teamcity_server/datadir/ -T /tmp/database.postgresql.properties.dist 
cat /data/teamcity_server/datadir/config/database.properties

/opt/teamcity/bin/teamcity-server.sh start
```
