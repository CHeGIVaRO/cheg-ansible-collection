# Ansible Collection for Home Use

Коллекция Ansible ролей для домашнего использования.

## Роли

### baseos
Базовая настройка системы:
- Создание SSH пользователей
- Установка базовых пакетов (mc, atop, htop, vim, curl, wget)
- Настройка SSH (отключение root, только ключи)
- Отключение ufw/firewalld (для использования iptables)
- Настройка timezone и locale

### postgresql
Установка и настройка PostgreSQL 17:
- Установка PostgreSQL 17 из официальных репозиториев
- Создание пользователей и баз данных
- Настройка внешнего доступа (listen_addresses: *)
- Поддержка русского языка и timezone
- Автоматические бэкапы и очистка
- Настройка кастомных путей для данных и WAL

## Использование

```yaml
# playbook.yml
---
- hosts: servers
  become: true
  
  roles:
    - baseos
    - postgresql
```

## Переменные

### baseos
```yaml
baseos_ssh_users:
  - name: admin
    password: "{{ vault_admin_password }}"
    ssh_key: "{{ vault_admin_ssh_key }}"
    sudo: true

baseos_packages:
  - mc
  - atop
  - htop
  - vim
  - curl
  - wget
```

### postgresql
```yaml
postgresql_version: "17"
postgresql_admin_user: "postgres"
postgresql_admin_password: "{{ vault_postgres_admin_password }}"

postgresql_users:
  - name: app_user
    password: "{{ vault_app_password }}"
    databases: ["app_db"]

postgresql_databases:
  - name: app_db
    owner: app_user
    encoding: UTF8
    lc_collate: ru_RU.UTF-8
    lc_ctype: ru_RU.UTF-8
```

## Зависимости

- `community.postgresql` >= 4.1.0

## Поддерживаемые ОС

- Ubuntu 20.04+
- Ubuntu 24.04+
- Debian 11+
- CentOS/RHEL 7+
- Rocky Linux 8+

## Автор

CHeGIVaRO
