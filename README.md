[![CI](https://github.com/de-it-krachten/ansible-role-openssl/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-openssl/actions?query=workflow%3ACI)


# ansible-role-openssl

Role install openssl and set-up keys & certificates

#### TODO

* Create server certificate sign-request for external CA
* Setup optional internal Certificate Authority
* Setup client/server certificates, signed by the internal CA

Platforms
--------------

Supported platforms

- CentOS 7
- CentOS 8
- RockyLinux 8
- AlmaLinux 8
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS



Role Variables
--------------
<pre><code>
# OpenSSL packages
openssl_packages:
  - openssl

# Type of key to create
openssl_type:          self-signed

# FQDN of the server to create it for
openssl_fqdn:          {{ inventory_hostname }}

# Directory to put keys & certificates into
openssl_dir:           /etc/ssl

# SSL private key
openssl_server_key:    {{ openssl_dir }}/private/{{ openssl_fqdn }}.key

# SSL certificate
openssl_server_crt:    {{ openssl_dir }}/certs/{{ openssl_fqdn }}.crt

# SSL sign request
openssl_server_csr:    {{ openssl_dir }}/certs/{{ openssl_fqdn }}.csr
</pre></code>


Example Playbook
----------------

<pre><code>
- name: sample playbook for role 'openssl'
  hosts: all
  vars:
  tasks:
    - name: Include role 'openssl'
      include_role:
        name: openssl
</pre></code>
