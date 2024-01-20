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

This role is designed to create Apache2 configurations tailored for various web applications. It offers a wide range of configuration options, allowing administrators to customize Apache2 settings based on their specific needs.

The role supports LDAP authentication, enabling administrators to integrate Apache2 with an LDAP server for user authentication. By specifying the LDAP URL, port, and domain components, administrators can easily set up LDAP-based authentication for their web applications.

It also provides options to set up virtual hosts for different server names and aliases, making it convenient to host multiple web applications on the same Apache2 server.

Administrators can define black-hole hosts, which will not be served by Apache2, effectively blocking access to specific domains.

SSL support is available, allowing administrators to configure HTTPS for their web applications. They can specify the SSL key and certificate files to enable secure connections.

Proxy pass functionality is also supported, enabling administrators to redirect requests to a backend server or another URL using the Apache2 proxy module.

The role offers fine-grained control over the ModSecurity module, allowing administrators to enable or disable specific rules based on their security requirements.

Furthermore, administrators can define rewrite rules to modify URLs and redirect specific requests to different locations.

The role also allows for URL-based exceptions from LDAP authentication, useful for scenarios where certain routes should not require authentication.

For administrators looking to manage multiple Apache2 configurations easily, this role is a valuable addition to the basic Apache2 installation. Its flexibility and extensive feature set make it a powerful tool for creating custom Apache2 setups to cater to various web application requirements.

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
add_apache_confs_http_listen_port: 80
add_apache_confs_https_listen_port: 443

add_apache_confs_apache_group: "www-data"
add_apache_confs_apache_user: "www-data"

add_apache_confs_ldap_url: "ldap.your.domain"
add_apache_confs_ldap_port: 389
add_apache_confs_ldap_dc: "DC=sub,DC=domain,DC=net"
add_apache_confs_auth_ldap_url: "ldap://{{ add_apache_confs_ldap_url }}:{{ add_apache_confs_ldap_port }}/OU=People,{{ add_apache_confs_ldap_dc }}?uid"
add_apache_confs_custom_ldap_base_header: "ldap-auth"

add_apache_confs_webmaster: "your.webmaster@domain.tld"
add_apache_confs_request_body: 0
add_apache_confs_security_body_limit: 1024000000

add_apache_confs_ssl_files_path: "/etc/apache2/ssl"
add_apache_confs_conf_path: "/etc/apache2/sites-available"
add_apache_confs_base_document_root: "/var/www/html"

add_apache_confs_configurations:
  - server:
      name: "my-apache-site-1.domain.tld"
      aliases:
        - "oneAlias.net"
        - "anotherAlias.com"
      webmaster: "{{ add_apache_confs_webmaster }}"
    ssl:
      enabled: true
      key: "{{ add_apache_confs_ssl_files_path }}/my-apache-site-1.domain.tld/my-apache-site-1.domain.tld.pem.key"
      crt: "{{ add_apache_confs_ssl_files_path }}/my-apache-site-1.domain.tld/my-apache-site-1.domain.tld.pem.crt"
    proxy:
      url: "https://my.proxy"
      pass: "/"
    security:
      disabled: true
      request_body: 4096000
      sec_rules_remove_by_id:
        - 980130
        - 933120
    rewrites:
      - ^/\.well-known/carddav /remote.php/dav [R=301,L]
      - ^/\.well-known/caldav /remote.php/dav [R=301,L]
      - ^/\.well-known/webfinger /index.php/.well-known/webfinger [R=301,L]
      - ^/\.well-known/nodeinfo /index.php/.well-known/nodeinfo [R=301,L]
    redirect: "http://my.newest.site.net"
    ldap:
      url: "{{ add_apache_confs_auth_ldap_url }}"
      attribute: "memberUid"
      attribute_is_dn: "off"
      dc: "{{ add_apache_confs_ldap_dc }}"
      groups:
        - admin
        - users
      exception_base_urls:
        - /api
    custom_confs:
        - name: "phpmyadmin.j2"
          alias:
            url: "/"
            path: "/usr/share/phpmyadmin"
          install_path: "/usr/share/phpmyadmin"
        
```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
---
inv_add_apache_confs_http_listen_port: 80
inv_add_apache_confs_https_listen_port: 443

inv_add_apache_confs_ldap_url: "ldap.your.domain"
inv_add_apache_confs_ldap_port: 389
inv_add_apache_confs_ldap_dc: "DC=sub,DC=domain,DC=net"
inv_add_apache_confs_auth_ldap_url: "ldap://{{ inv_add_apache_confs_ldap_url }}:{{ inv_add_apache_confs_ldap_port }}/OU=People,{{ inv_add_apache_confs_ldap_dc }}?uid"
inv_add_apache_confs_custom_ldap_base_header: "ldap-auth"

inv_add_apache_confs_webmaster: "your.webmaster@domain.tld"
inv_add_apache_confs_request_body: 0
inv_add_apache_confs_security_body_limit: 1024000000

inv_add_apache_confs_ssl_files_path: "/etc/apache2/ssl"
inv_add_apache_confs_conf_path: "/etc/apache2/sites-available"
inv_add_apache_confs_base_document_root: "/var/www/html"

inv_add_apache_confs_configurations:
  - server:
      name: "my-apache-site-1.domain.tld"
      aliases:
        - "oneAlias.net"
        - "anotherAlias.com"
      webmaster: "{{ inv_add_apache_confs_webmaster }}"
    ssl:
      enabled: true
      key: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-1.domain.tld/my-apache-site-1.domain.tld.pem.key"
      crt: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-1.domain.tld/my-apache-site-1.domain.tld.pem.crt"
    proxy:
      url: "https://my.proxy"
      pass: "/"
    security:
      disabled: true
      request_body: 4096000
      sec_rules_remove_by_id:
        - 980130
        - 933120
    rewrites:
      - ^/\.well-known/carddav /remote.php/dav [R=301,L]
      - ^/\.well-known/caldav /remote.php/dav [R=301,L]
      - ^/\.well-known/webfinger /index.php/.well-known/webfinger [R=301,L]
      - ^/\.well-known/nodeinfo /index.php/.well-known/nodeinfo [R=301,L]
    redirect: "http://my.newest.site.net"
    ldap:
      url: "{{ inv_add_apache_confs_auth_ldap_url }}"
      attribute: "memberUid"
      attribute_is_dn: "off"
      dc: "{{ inv_add_apache_confs_ldap_dc }}"
      groups:
        - admin
        - users
      exception_base_urls:
        - /api

  - server:
      name: "my-apache-site-2.domain.tld"
      aliases:
        - "localhost"
        - "127.0.0.1"
    ssl:
      enabled: true
      key: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-2.domain.tld/my-apache-site-2.domain.tld.pem.key"
      crt: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-2.domain.tld/my-apache-site-2.domain.tld.pem.crt"
    proxy:
      url: "https://www.google.com:443/"
      pass: "/"

  - server:
      name: "my-apache-site-3.domain.tld"
    ssl:
      enabled: true
      key: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-3.domain.tld/my-apache-site-3.domain.tld.pem.key"
      crt: "{{ inv_add_apache_confs_ssl_files_path }}/my-apache-site-3.domain.tld/my-apache-site-3.domain.tld.pem.crt"
    custom_confs:
        - name: "phpmyadmin.j2"
          alias:
            url: "/"
            path: "/usr/share/phpmyadmin"
          install_path: "/usr/share/phpmyadmin"
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
    add_apache_confs_http_listen_port: "{{ inv_add_apache_confs_http_listen_port }}"
    add_apache_confs_https_listen_port: "{{ inv_add_apache_confs_https_listen_port }}"
    add_apache_confs_ldap_url: "{{ inv_add_apache_confs_ldap_url }}"
    add_apache_confs_ldap_port: "{{ inv_add_apache_confs_ldap_port }}"
    add_apache_confs_ldap_dc: "{{ inv_add_apache_confs_ldap_dc }}"
    add_apache_confs_auth_ldap_url: "{{ inv_add_apache_confs_auth_ldap_url }}"
    add_apache_confs_custom_ldap_base_header: "{{ inv_add_apache_confs_custom_ldap_base_header }}"
    add_apache_confs_webmaster: "{{ inv_add_apache_confs_webmaster }}"
    add_apache_confs_request_body: "{{ inv_add_apache_confs_request_body }}"
    add_apache_confs_security_body_limit: "{{ inv_add_apache_confs_security_body_limit }}"
    add_apache_confs_ssl_files_path: "{{ inv_add_apache_confs_ssl_files_path }}"
    add_apache_confs_conf_path: "{{ inv_add_apache_confs_conf_path }}"
    add_apache_confs_base_document_root: "{{ inv_add_apache_confs_base_document_root }}"
    add_apache_confs_configurations: "{{ inv_add_apache_confs_configurations }}"
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

### 2023-08-15: Big update

* Role now can use the v2 version of install_apache from labocbz
* Role have now a module based approche instead of code repetition for included j2 based of vars
* Role can now have custom, in the ./templates/custom you can put your j2 custom conf and import these conf directly via the custom_confs.name property
* Role have a better render aspect in the final conf
* Some fix are present
* Header, Pagespeed, Cookies, etc are now global conf, in the install_apache role, but you can do your by create a custom conf !
* Better var naming
* If a conf use HTTPS, role redirect trafic to HTTPS directly instead of rendering 2 files completly (see code for more details)
* As an example, a phpmyadmin custom_conf is present, but not working because its required more vars, so it's just an example, please check if before use it

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-11-16: Redirection

* HTTPS redirection is now based on %{SERVER_PORT}

### 2023-11-28: Logs and Redirect

* Fix the redirection
* Add ENV for no log

### 2023-12-14: System users

* Role can now use system users and address groups

### 2023-27-12: Preserve host

* You can now disable the preverse host option (in cause on empty request / logs)

### 2024-01-20: http-check

* Added a conf for /http-check as LB can check the app
* Added a log exclusion for the check

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
