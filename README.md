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
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS



Role Variables
--------------
<pre><code>


openssl_type:          self-signed

openssl_fqdn:          "{{ inventory_hostname }}"

openssl_dir:           /etc/ssl
openssl_server_key:    "{{ openssl_dir }}/private/{{ openssl_fqdn }}.key"
openssl_server_crt:    "{{ openssl_dir }}/certs/{{ openssl_fqdn }}.crt"
openssl_server_csr:    "{{ openssl_dir }}/certs/{{ openssl_fqdn }}.csr"
</pre></code>


Example Playbook
----------------

<pre><code>


- name: Converge
  hosts: all
  vars: null
  tasks:
    - name: Include role 'ansible-role-openssl'
      include_role:
        name: ansible-role-openssl
</pre></code>
