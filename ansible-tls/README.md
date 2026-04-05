# ansible-tls

Ansible-проект для автоматической установки и обновления TLS-сертификатов Let's Encrypt через Certbot + Nginx.

## Структура

```
ansible-tls/
├── ansible.cfg                          # Конфигурация Ansible
├── inventory/
│   └── hosts.yml                        # Инвентарь серверов
├── playbooks/
│   ├── install_cert.yml                 # Установка сертификата
│   └── renew_cert.yml                   # Обновление сертификата
└── roles/
    └── certbot/
        ├── tasks/
        │   ├── main.yml                 # Точка входа роли
        │   ├── install.yml              # Установка Certbot
        │   ├── obtain.yml               # Получение сертификата
        │   └── renew.yml                # Обновление сертификата
        ├── handlers/
        │   └── main.yml                 # Reload/Restart Nginx
        └── templates/
            └── nginx-ssl.conf.j2        # Шаблон Nginx с SSL
```

## Быстрый старт

### 1. Настроить inventory

Отредактируйте `inventory/hosts.yml` — укажите IP сервера, домен и email:

```yaml
all:
  hosts:
    webserver:
      ansible_host: 123.45.67.89
      domain_name: example.ru
      email: admin@example.ru
```

### 2. Установить сертификат

```bash
ansible-playbook playbooks/install_cert.yml
```

### 3. Обновить сертификат

```bash
ansible-playbook playbooks/renew_cert.yml
```

## Использование с AWX

1. Создайте **Project** и укажите Git-репозиторий с этим проектом
2. Создайте **Inventory** с переменными `domain_name` и `email`
3. Создайте **Job Template** для каждого playbook
4. Для автообновления добавьте **Schedule** на `renew_cert.yml` (раз в неделю)

## Требования

- Целевой сервер: Ubuntu 20.04+ с Nginx
- Домен с A-записью, указывающей на IP сервера
- Открытые порты 80 и 443
