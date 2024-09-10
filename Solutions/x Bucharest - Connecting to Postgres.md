---
dg-publish: true
---

# "Bucharest": Connecting to Postgres
**Level:** Easy
**Description:** A web application relies on the PostgreSQL 13 database present on this server. However, the connection to the database is not working. Your task is to identify and resolve the issue causing this connection failure. The application connects to a database named _app1_ with the user _app1user_ and the password _app1user_.  
  
Credit [PykPyky](https://twitter.com/PykPyky)

**Test:** Running `PGPASSWORD=app1user psql -h 127.0.0.1 -d app1 -U app1user -c '\q'` succeeds (does not return an error).

---
### Notes and solution:

The solution is to check the configuration file _pg_hba.conf_ for the SQL server located in _/etc/postgresql/$version/main/_, there you have to delete or comment all the configuration lines with the word _reject_.
