Grafana server
=========

[![Build Status](https://travis-ci.org/TalAntR/ansible-grafana.svg?branch=master)](https://travis-ci.org/TalAntR/ansible-grafana)

This is an Ansible role for grafana server. By default it uses DEB and RPM
[repositories](https://packagecloud.io/grafana/stable) from Grafana vendor to
install the latest versions of agent and plugins.


Requirements
------------

This role has been tested with Ansible 2.1 only. It's also supposed that
you are using the merge behaviour for variables (please, see about
[hash_behaviour=merge](http://docs.ansible.com/ansible/intro_configuration.html#hash-behaviour)
for details)


Role Variables
--------------

This role declares and uses the configurations variables in a hash under the
_grafana_ key (besides a variable grafana_version). This is a description 
for main variables which you may want to change.


  * _grafana_version_ is a desired version of grafana server;

  * _grafana.ldap_ is a section to declare settings to enable LDAP auth,
    the settings will be rendered in a TOML configuration which is specified
    with _grafana.selvice.options.auth.ldap.config_file_ parameter.

  * _grafana.service_ is a section to declare global settings for grafana
    server;

Dependencies
------------

This role doesn't depend from other ansible roles


Example Playbook
----------------

An example of how to use setup grafana server:

    - hosts: localhost
      roles:
        - role: grafana
          grafana_version: 3.1.0
          grafana:
            service:
              options:
                server:
                  protocol: http
                users:
                  allow_sign_up: false
                  login_hint: "LDAP account"
                auth:
                  ldap:
                    enabled: true
            # LDAP settings which will be rendered in a TOML file
            # http://docs.grafana.org/installation/ldap/
            ldap:
              verbose_logging: true
              servers:
                - host: "vpn-000.example.lo"
                  port: 389
                  group_search_filter: "(&(objectClass=posixGroup)(memberUid=%s))"
                  group_search_base_dns: ["ou=Group,dc=example,dc=lo"]
                  search_filter: "(&(objectClass=inetOrgPerson)(uid=%s))"
                  search_base_dns: ["dc=example,dc=lo"]
                  attributes:
                    name: "givenName"
                    surname: "sn"
                    username: "cn"
                    member_of: "cn"
                    email: "mail"
                  group_mappings:
                    - group_dn: "admins"
                      org_role: "Admin"
                      org_id: 1
                    - group_dn: "users"
                      org_role: "Viewer"
                      org_id: 1

License
-------

MIT


Author Information
------------------

Anton Talevnin <talantr@gmail.com>
