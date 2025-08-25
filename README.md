# cheg-ansible-collection

Ansible Collection with roles for home using

Коллекция Ansible ролей для домашнего использования

## Description / Описание

This collection contains Ansible roles designed for home server and workstation management. Each role is focused on specific functionality and can be used independently or in combination with other roles.

Эта коллекция содержит Ansible роли, предназначенные для управления домашними серверами и рабочими станциями. Каждая роль сосредоточена на конкретной функциональности и может использоваться независимо или в сочетании с другими ролями.

## Roles / Роли

### baseos

**English:**
Base operating system configuration role. Provides basic system setup including SSH user management and essential package installation.

**Русский:**
Роль базовой настройки операционной системы. Обеспечивает базовую настройку системы, включая управление SSH пользователями и установку основных пакетов.

**Features / Возможности:**
- SSH user creation and management / Создание и управление SSH пользователями
- Basic package installation (mc, atop, etc.) / Установка базовых пакетов (mc, atop и др.)
- System hardening / Усиление безопасности системы
- User sudo privileges management / Управление sudo привилегиями пользователей
- Firewall services disabled (iptables will be used instead) / Службы файрвола отключены (будет использоваться iptables)

### postgresql

**English:**
Single-node PostgreSQL 17 installation and configuration role. Provides database server setup with user and database management capabilities.

**Русский:**
Роль установки и настройки PostgreSQL 17 в односерверном режиме. Обеспечивает настройку сервера базы данных с возможностями управления пользователями и базами данных.

**Features / Возможности:**
- PostgreSQL 17 server installation / Установка сервера PostgreSQL 17
- Database user creation and management / Создание и управление пользователями базы данных
- Database creation and configuration / Создание и настройка баз данных
- Connection security configuration (local and external) / Настройка безопасности подключений (локальных и внешних)
- Russian locale and timezone support / Поддержка русской локали и таймзоны
- Custom data and WAL directories configuration / Настройка кастомных путей для данных и WAL
- Automated backup and cleanup configuration / Автоматическое резервное копирование и очистка

## Requirements / Требования

- Ansible 2.9+
- Python 3.6+
- Target systems: Ubuntu 20.04+, CentOS 8+, RHEL 8+
- community.postgresql collection (>=4.1.0)

## Installation / Установка

```bash
# Clone the repository
git clone https://github.com/your-username/cheg-ansible-collection.git
cd cheg-ansible-collection

# Install collection locally
ansible-galaxy collection build
ansible-galaxy collection install cheg-ansible-collection-*.tar.gz
```

**Note:** External dependencies (like `community.postgresql`) will be automatically installed by Ansible Galaxy.

**Примечание:** Внешние зависимости (такие как `community.postgresql`) будут автоматически установлены Ansible Galaxy.

## Usage / Использование

### Using as a dependency in your project / Использование как зависимость в вашем проекте

Add to your `requirements.yml`:
```yaml
collections:
  - name: cheg.ansible_collection
    version: ">=1.0.0"
    source: "https://github.com/your-username/cheg-ansible-collection"
```

**Note:** Dependencies like `community.postgresql` will be automatically resolved by Ansible Galaxy when installing this collection.

**Примечание:** Зависимости, такие как `community.postgresql`, будут автоматически разрешены Ansible Galaxy при установке этой коллекции.

Create your own inventory file (e.g., `inventory.yml`):
```yaml
all:
  children:
    servers:
      hosts:
        your-server:
          ansible_host: YOUR_SERVER_IP
          ansible_user: root
```

### Using roles directly / Использование ролей напрямую

```yaml
# playbook.yml
---
- hosts: servers
  roles:
    - baseos
    - postgresql
```

### Using collection / Использование коллекции

```yaml
# playbook.yml
---
- hosts: servers
  roles:
    - cheg.baseos
    - cheg.postgresql
```

## Role Variables / Переменные ролей

### baseos variables

```yaml
# SSH users to create
baseos_ssh_users:
  - name: admin
    password: "{{ vault_admin_password }}"
    ssh_key: "{{ vault_admin_ssh_key }}"
    sudo: true

# Packages to install
baseos_packages:
  - mc
  - atop
  - htop
  - vim
  - curl
  - wget
```

### postgresql variables

```yaml
# PostgreSQL version
postgresql_version: "17"

# PostgreSQL data directories (optional)
postgresql_data_directory: "/var/lib/postgresql/data"
postgresql_wal_directory: "/var/lib/postgresql/wal"

# Database users
postgresql_users:
  - name: app_user
    password: "{{ vault_app_password }}"
    databases: ["app_db"]

# Databases to create
postgresql_databases:
  - name: app_db
    owner: app_user
    encoding: UTF8
    template: template0
    lc_collate: ru_RU.UTF-8
    lc_ctype: ru_RU.UTF-8
```

## License / Лицензия

MIT License

## Author / Автор

Created by CHeGIVaRO

## Contributing / Вклад в проект

Contributions are welcome! Please feel free to submit a Pull Request.

Вклады приветствуются! Пожалуйста, не стесняйтесь отправлять Pull Request.
