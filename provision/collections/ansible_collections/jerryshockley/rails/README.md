# Ansible Rails

![Ansible Lint](https://github.com/jerryshockley/ansible-rails/workflows/Ansible%20Lint/badge.svg)

The Ansible collection for Ruby on Rails provision.

## Requirements

* Ansible 2.9+

## Usage

Add `jerryshockley.rails` to your `requirements.yml`

```yaml
# ...

collections:
- jerryshockley.rails
```

Install Collection

```bash
ansible-galaxy collection install -r requirements.yml
```

Copy the playbooks from [examples](https://github.com/jerryshockley/ansible-rails/blob/master/docs/examples) to your project.

Add default variables to `group_vars/all.yml` (strong suggestion use `ansible-vault` to encryption it)

```yml
app_user: deploy
app_group: deploy

nginx_config_template: nginx.conf.j2
nginx_extra_modules:
  - passenger

ruby_version: 2.7.1
bundler: yes
bundler_version: 2.1.4
postgres: yes

postgres_username: deploy
postgres_database: my_app

deploy_key: [DEPLOY_KEY]
authorized_keys:
  - [YOUR_KEYS]
```

> The customize `nginx.conf.j2` template please copy from examples to ensure your enabled passenger

Run playbooks to setup your project.

```bash
ansible-playbook -i [INVENTORY] setup.yml
```

Use `update.yml` playbook to update configure after change variables

```bash
ansible-playbook -i [INVENTORY] update.yml
```

The application root will configure to `/srv/app`, you can use Capistrano to deploy your application.

### Playbooks

* [`setup.yml`](https://github.com/jerryshockley/ansible-rails/blob/master/docs/examples/setup.yml) will used for setup the server.
* [`update.yml`](https://github.com/jerryshockley/ansible-rails/blob/master/docs/examples/update.yml) will used for update the configurations.

## Variables

    TODO

## Roadmap

* [ ] Ubuntu Support
* [ ] Setup Sidekiq Service
* [ ] Let's Encrypt Support
* [ ] Better `nginx.conf` template
* [ ] Configure environment variables to `/etc/environment`
