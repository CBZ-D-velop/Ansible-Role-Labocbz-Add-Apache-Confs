# Ansible role: labocbz.add_apache_confs

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Apache2](https://img.shields.io/badge/Tech-Apache2-orange)
![Tag: HTTPD](https://img.shields.io/badge/Tech-HTTPD-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Modsecurity](https://img.shields.io/badge/Tech-Modsecurity-orange)
![Tag: QOS](https://img.shields.io/badge/Tech-QOS-orange)
![Tag: Deflate](https://img.shields.io/badge/Tech-Deflate-orange)
![Tag: Proxy/ReverseProxy](https://img.shields.io/badge/Tech-Proxy%2FReverseProxy-orange)
![Tag: OWASP](https://img.shields.io/badge/Tech-OWASP-orange)
![Tag: Certbot](https://img.shields.io/badge/Tech-Certbot-orange)

An Ansible role create and add Apache2 HTTP/HTTPS confs to your server.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
apache_ldap_url: "ldap.your.domain"
apache_ldap_port: 389
apache_ldap_dc: "DC=sub,DC=domain,DC=net"
apache_auth_ldap_url: "ldap://{{ apache_ldap_url }}:{{ apache_ldap_port }}/OU=People,{{ apache_ldap_dc }}?uid"
apache_custom_ldap_base_header: "ldap-auth"

apache_webmaster: "your.webmaster@domain.tld"

apache_http_listen_port: 80
apache_https_listen_port: 443

apache_mod_security_available: true
apache_mod_deflate_available: true
apache_mod_pagespeed_available: true

apache_ssl_files_path: "/etc/letsencrypt/live"
apache_conf_path: "/etc/apache2/sites-available"
apache_base_document_root: "/var/www/html"

apache_limit_request_body: 1024000000

apache_configurations:
  - server_name: "my.https.website.net"
    server_aliases:
      - oneAlias.net
      - anotherAlias.com
    is_black_hole: true
    ssl: true
    key_file: "{{ apache_ssl_files_path }}/my.https.website.net/my.https.website.net.key"
    crt_file: "{{ apache_ssl_files_path }}/my.https.website.net/my.https.website.net.crt"
    proxy_pass_url: "https://my.proxy"
    proxy_pass_port: 8080
    sec_rule_engine_detection_only: true
    sec_rules_remove_by_id:
      - 980130
      - 933120
    rewrite_rules:
      - ^/\.well-known/carddav /remote.php/dav [R=301,L]
      - ^/\.well-known/caldav /remote.php/dav [R=301,L]
      - ^/\.well-known/webfinger /index.php/.well-known/webfinger [R=301,L]
      - ^/\.well-known/nodeinfo /index.php/.well-known/nodeinfo [R=301,L]
    redirect_host: "http://my.newest.site.net"
    ldap_groups:
      - admin
      - users
    ldap_exception_base_urls:
      - /api

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
---
inv_apache_ldap_url: "ldap.your.domain"
inv_apache_ldap_port: 389
inv_apache_ldap_dc: "DC=sub,DC=domain,DC=net"
inv_apache_auth_ldap_url: "ldap://{{ inv_apache_ldap_url }}:{{ inv_apache_ldap_port }}/OU=People,{{ inv_apache_ldap_dc }}?uid"
inv_apache_custom_ldap_base_header: "ldap-auth"

inv_apache_webmaster: "your.webmaster@domain.tld"

inv_apache_http_listen_port: 80
inv_apache_https_listen_port: 443

inv_apache_ssl_files_path: "/etc/apache2/ssl"
inv_apache_conf_path: "/etc/apache2/sites-available"
inv_apache_base_document_root: "/var/www/html"


inv_apache_configurations:
  - server_name: "my.https.website.domain.tld"
    server_aliases:
      - oneAlias.net
      - anotherAlias.com
    is_black_hole: true
    ssl: true
    key_file: "{{ inv_apache_ssl_files_path }}/my.https.website.domain.tld/my.https.website.domain.tld.key"
    crt_file: "{{ inv_apache_ssl_files_path }}/my.https.website.domain.tld/my.https.website.domain.tld.crt"
    proxy_pass_url: "https://my.proxy"
    proxy_pass_port: 8080
    sec_rule_engine_detection_only: true
    sec_rules_remove_by_id:
      - 980130
      - 933120
    rewrite_rules:
      - ^/\.well-known/carddav /remote.php/dav [R=301,L]
      - ^/\.well-known/caldav /remote.php/dav [R=301,L]
      - ^/\.well-known/webfinger /index.php/.well-known/webfinger [R=301,L]
      - ^/\.well-known/nodeinfo /index.php/.well-known/nodeinfo [R=301,L]
    redirect_host: "http://my.newest.site.net"
    ldap_groups:
      - admin
      - users
    ldap_exception_base_urls:
      - /api

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.add_apache_confs"
  tags:
    - "labocbz.add_apache_confs"
  vars:
    apache_ldap_url: "{{ inv_apache_ldap_url }}"
    apache_ldap_port: "{{ inv_apache_ldap_port }}"
    apache_ldap_dc: "{{ inv_apache_ldap_dc }}"
    apache_auth_ldap_url: "{{ inv_apache_auth_ldap_url }}"
    apache_custom_ldap_base_header: "{{ inv_apache_custom_ldap_base_header }}"
    apache_webmaster: "{{ inv_apache_webmaster }}"
    apache_http_listen_port: "{{ inv_apache_http_listen_port }}"
    apache_https_listen_port: "{{ inv_apache_https_listen_port }}"
    apache_ssl_files_path: "{{ inv_apache_ssl_files_path }}"
    apache_configurations: "{{ inv_apache_configurations }}"
    apache_base_document_root: "{{ inv_apache_base_document_root }}"
  ansible.builtin.include_role:
    name: "labocbz.add_apache_confs"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-17: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
