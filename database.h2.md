### How to dump SQL from a H2 database file 

1. `cd` into the directory where h2-x.x.xx.jar or download it. H2 jar contains a class to run scripts under  the package tools.
2. Create file called `query.sql` and copy/ paste this command. This command instruction to dump the databases schema and data to a file called `db-dump.sql`:

```sh
SCRIPT TO 'db-dump.sql'
```

3. Run the command:

```sh
java -cp h2-x.xx.xxx.jar org.h2.tools.RunScript -url "jdbc:h2:file:full_path/db_file_name" -user sa -password sa -script query.sql -showResults
```

That should be pretty much everything. You will get the complete SQL dump in `db-dump.sql` now. If you get an error, H2 will tell you what is going wrong. Perhaps you got the URL or credentials wrong?!