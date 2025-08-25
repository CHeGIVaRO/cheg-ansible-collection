# PostgreSQL Role

Single-node PostgreSQL installation and configuration role for home servers.

## Description

This role provides complete PostgreSQL server setup including installation, configuration, user management, database creation, and backup configuration.

## Features

- PostgreSQL server installation from official repositories
- Automatic configuration optimization
- Database user creation and management
- Database creation and configuration
- PostgreSQL extensions installation
- Automated backup configuration with cron jobs
- Comprehensive logging configuration
- Firewall configuration
- Support for Ubuntu/Debian and CentOS/RHEL systems

## Requirements

- Ansible 2.9+
- Target systems: Ubuntu 20.04+, CentOS 8+, RHEL 8+
- Python 3.6+ with psycopg2 module
- community.postgresql collection (>=4.1.0)

## Compatibility

This role is tested and compatible with:
- PostgreSQL 17
- community.postgresql collection 4.1.0+
- Ansible 2.9+

## Role Variables

### postgresql_version
PostgreSQL version to install (default: "17")

### postgresql_config
Main PostgreSQL configuration parameters:
- `listen_addresses`: Network interfaces to listen on (default: "*")
- `port`: PostgreSQL port (default: 5432)
- `max_connections`: Maximum number of connections (default: 100)
- `shared_buffers`: Shared memory buffer size (default: "128MB")
- `effective_cache_size`: Effective cache size (default: "256MB")
- And many more performance tuning parameters

### postgresql_users
List of database users to create. Each user can have:
- `name`: Username
- `password`: User password (use vault for security)
- `privileges`: Database privileges (optional)
- `role_attr_flags`: Role attributes (optional)
- `databases`: List of databases to grant access to (optional)

Example:
```yaml
postgresql_users:
  - name: app_user
    password: "{{ vault_app_password }}"
    privileges: "ALL"
    role_attr_flags: "CREATEDB,NOSUPERUSER"
    databases: ["app_db"]
```

### postgresql_databases
List of databases to create. Each database can have:
- `name`: Database name
- `owner`: Database owner (default: postgres)
- `encoding`: Character encoding (default: UTF8)
- `template`: Template database (default: template0)
- `lc_collate`: Collation (default: ru_RU.UTF-8)
- `lc_ctype`: Character classification (default: ru_RU.UTF-8)

Example:
```yaml
# Custom data directories
postgresql_data_directory: "/var/lib/postgresql/data"
postgresql_wal_directory: "/var/lib/postgresql/wal"

# Databases
postgresql_databases:
  - name: app_db
    owner: app_user
    encoding: UTF8
    template: template0
    lc_collate: ru_RU.UTF-8
    lc_ctype: ru_RU.UTF-8
```

### postgresql_extensions
List of PostgreSQL extensions to install:
```yaml
postgresql_extensions:
  - pg_stat_statements
  - uuid-ossp
  - hstore
```

### postgresql_default_locale
Default locale for PostgreSQL (default: "ru_RU.UTF-8")

### postgresql_default_timezone
Default timezone for PostgreSQL (default: "Europe/Moscow")

### postgresql_default_encoding
Default encoding for databases (default: "UTF8")

### postgresql_data_directory
Custom data directory for PostgreSQL (optional). If not specified, uses default system path.

### postgresql_wal_directory
Custom WAL directory for PostgreSQL (optional). If not specified, uses default system path.

### postgresql_backup
Backup configuration:
- `enabled`: Enable automated backups (default: false)
- `path`: Backup directory (default: "/var/backups/postgresql")
- `retention_days`: Days to keep backups (default: 7)
- `cron_schedule`: Cron schedule for backups (default: "0 2 * * *")

### postgresql_logging
Logging configuration:
- `enabled`: Enable logging (default: true)
- `log_destination`: Log destination (default: "stderr")
- `logging_collector`: Enable logging collector (default: true)
- And many more logging parameters

## Example Playbook

```yaml
---
- hosts: database_servers
  vars:
    postgresql_version: "17"
    postgresql_users:
      - name: app_user
        password: "{{ vault_app_password }}"
        privileges: "ALL"
        databases: ["app_db"]
    postgresql_databases:
      - name: app_db
        owner: app_user
        encoding: UTF8
    postgresql_extensions:
      - pg_stat_statements
      - uuid-ossp
    postgresql_backup:
      enabled: true
      path: "/var/backups/postgresql"
      retention_days: 7
  roles:
    - postgresql
```

## Security Notes

- Always use Ansible Vault for storing database passwords
- The role configures PostgreSQL to listen on all interfaces by default
- Review and customize authentication methods in pg_hba.conf
- Consider using SSL connections for production environments
- Configure firewall rules to restrict access to PostgreSQL port
- Regularly update PostgreSQL to the latest security patches

## Performance Tuning

The role includes basic performance tuning parameters suitable for small to medium workloads. For production environments, consider:

- Adjusting memory parameters based on available RAM
- Tuning WAL settings for your workload
- Configuring connection pooling
- Setting up replication for high availability

## Backup and Recovery

When backup is enabled, the role creates:
- Automated backups using pg_dumpall with compression
- Backup files with timestamps
- Automatic cleanup of old backups (based on retention_days)
- Comprehensive backup logs for monitoring
- Single cron job that handles both backup and cleanup

## License

MIT
