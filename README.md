toni.openresty
==============

Ansible role to setup openresty as reverse proxy. Its part of a bigger stack to deploy Bigbluebutton in an privacy first setup which uses Oauth2 for authentication. Therefore it is necessary to have lua available.
Openresty will be setup as docker container. It is possible to serve the nodes host network, use it on a single machine by docker-compose or as docker stack in swarm environments. It integrates with Keycloak and the configuration of vhost(s) with authentication, logs, ssl and proxy can be easily done by snippets.

Requirements
------------

Roles:
  - toni.buildimages

Role Variables
--------------

```yaml
nginx_run_as: "docker"   # Possible values: docker / service / streamproxy / stack
nginx_access_loglevel: "production"
nginx_error_loglevel: 'warn'
configure_firewall: true
enable_clear_dockerlogs: false  # All docker logs will be deleted on sunday night at 3 a clock
nginx_network_driver: "bridge" # overlay in swarm 
nginx_resolver: "127.0.0.11" 
openresty_session_name: "__Secure-session_resty"
openresty_session_secret: "623q4hR311t36VsC3Da3H7922IC0073T" # dummy to be replaced 
auth_provider_domain: "key.{{ domain }}" # where to reach keycloak provider
keycloak_realm: "Intern" # the realm which nginx is using in keycloak
keycloak_client_name: "Nginx" # client name in keycloak
keycloak_client_secret: "d3367b2d-85fb-46c0-9fcb-15e2dfeb4d9b" # dummy to be replaced: the secret which is used by openresty to authenticate against keycloak
keycloak_redirect: "https://lab.{{ domain }}/" 
auth_bypass: true # if set to true vhost authentication can be bypassed by specified ips
#auth_bypass_ips: ""
nginx_ssl_stream_config: false # Openresty will be used in stream mode - p.e. necessarry for turn server behind reverse proxy
openresty_image: "openresty:1.19.3.1-alpine-fat"
force_images: true #local image will removed and copied again to the ansible node - even if the tag does not change
```

Example Playbook
----------------
```yaml
- hosts: swarm-01
  remote_user: root
  gather_facts: yes
  roles:
    - {
      role: openresty,
      nginx_network_name: "nginx_net",
      nginx_network_driver: "overlay",
      nginx_run_as: "stack",
      force_images: "True"
      }
```

License
-------

MIT

Author Information
------------------

T.Nissen
