[![CI](https://github.com/de-it-krachten/ansible-role-openssl/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-openssl/actions?query=workflow%3ACI)


# ansible-role-openssl

Manage openssl and set-up keys & certificates

#### TODO

* Create server certificate sign-request for external CA
* Setup optional internal Certificate Authority
* Setup client/server certificates, signed by the internal CA



## Dependencies

#### Roles
None

#### Collections
- community.general
- community.crypto

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- AlmaLinux 8
- AlmaLinux 9
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Fedora 35
- Fedora 36

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# OpenSSL packages
openssl_packages:
  - openssl

# Type of key to create
openssl_type:          self-signed

# FQDN of the server to create it for
openssl_fqdn:          "{{ inventory_hostname }}"

# Additional/alternate names
openssl_fqdn_additional: []

# Directory to put keys & certificates into
openssl_dir:           /etc/ssl

# SSL private key
openssl_server_key:    "{{ openssl_dir }}/private/{{ openssl_fqdn }}.key"

# SSL certificate
openssl_server_crt:    "{{ openssl_dir }}/certs/{{ openssl_fqdn }}.crt"

# SSL sign request
openssl_server_csr:    "{{ openssl_dir }}/private/{{ openssl_fqdn }}.csr"
</pre></code>


### vars/Fedora.yml
<pre><code>
openssl_dir: /etc/pki/tls

openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    mode: '0755'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

openssl_packages:
  - openssl
  - python3-pip
</pre></code>

### vars/family-RedHat.yml
<pre><code>
openssl_dir: /etc/pki/tls

openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    mode: '0755'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

openssl_packages:
  - openssl
  - python3-pip
</pre></code>

### vars/family-Debian.yml
<pre><code>
openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    group: ssl-cert
    mode: '0710'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

openssl_packages:
  - openssl
  - ssl-cert
  - python3-pip
  - python3-cryptography
</pre></code>

### vars/family-RedHat-7.yml
<pre><code>
openssl_dir: /etc/pki/tls

openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    mode: '0755'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

openssl_packages:
  - openssl
  - python-pip
</pre></code>



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'openssl'
  hosts: all
  become: "{{ molecule['converge']['become'] | default('yes') }}"
  vars:
    openssl_fqdn: server.example.com
    openssl_fqdn_additional: ['vhost1.example.com', 'vhost2.example.com']
  tasks:
    - name: Include role 'openssl'
      ansible.builtin.include_role:
        name: openssl
</pre></code>
