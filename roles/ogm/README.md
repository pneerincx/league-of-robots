# OGM

Role deploys the backup and backup-management scripts for the Optical Genome Mapping.
It is entirely controlled by a variable `ogm_servers` in `group_vars/[stack_name]/vars.yml`.

Prerequisites
 - backup server can access OGM machine (firewall opened and login configured)
   - login credentials are defined by variables `server` and `user`
 - the permanent storage location on backup server (destination of backup data) is already configured
   - `psql_dump_location` defines the location on the OGM machine where the PostgreSQL database is dumped before the backup server picks it up.
   

## Configuration

Structure of the variable

```
    ogm_servers:
      - server: bas1.umcg.nl  # FQDN of OGM machine to backup
        user: ADMINIT         # username on OGM machine
        prm_location: /groups/umcg-ogm/prm67     # destination for backups on backup server
        backup_commands:
          - label: "pg_dump: create"
            command: sudo -u postgres bash -c 'cd; pg_dump --no-owner --no-privileges IrysView_Dev | /usr/bin/gzip -f -6' > pg_dump/$(date +%Y%m%d-%H%M%S).sql.gz
          - label: "pg_dump: remove old"
            command: find /home/ADMINIT/pg_dump/ -type f -mtime +4 -name '*.sql.gz' -exec rm -f {} \;
        backup_source_dirs:
          - /home/bionano/access/web/Server/databaseFiles/molecules_files
          - /var/log
          - /home/bionano/access/web/Server/Log
          - /home/bionano/access/web/Server/anchorFiles
          - /home/ADMINIT/pg_dump
        psql_dump_location: /home/ADMINIT/pg_dump
```

 - `backup_commands` is a list of commands that are executed on an OGM machine before the data is copied to the backup server.
 - `backup_source_dirs` is a list of directories on the OGM machine, which are rsync-ed to the backup server.


## Debugging

All logs are stored in the system's journal with a tag `ogm_backup`.