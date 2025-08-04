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
- community.crypto
- community.general

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- Red Hat Enterprise Linux 10<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- RockyLinux 10
- OracleLinux 8
- OracleLinux 9
- OracleLinux 10
- AlmaLinux 8
- AlmaLinux 9
- AlmaLinux 10
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Debian 13 (Trixie)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS
- Fedora 41
- Fedora 42
- Alpine 3
- Docker dind (CI only)

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# OpenSSL packages
openssl_packages:
  - openssl

# Type of key to create
openssl_type: self-signed

# FQDN of the server to create it for
openssl_fqdn: "{{ inventory_hostname }}"

# Additional/alternate names
openssl_fqdn_additional: []

# Directory to put keys & certificates into
openssl_dir: /etc/ssl

# Key + cetificate paths
openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    mode: '0710'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

# SSL private key
openssl_server_key: "{{ openssl_dir }}/private/{{ openssl_fqdn }}.key"

# SSL certificate
openssl_server_crt: "{{ openssl_dir }}/certs/{{ openssl_fqdn }}.crt"

# SSL sign request
openssl_server_csr: "{{ openssl_dir }}/private/{{ openssl_fqdn }}.csr"

# Custom CA
openssl_ca_domain: example.com
openssl_ca_name: example-com
openssl_ca_dir: /etc/ssl-ca
openssl_ca_dirs:
  - path: "{{ openssl_ca_dir }}"
    mode: '0755'
  - path: "{{ openssl_ca_dir }}/private"
    mode: '0710'
  - path: "{{ openssl_ca_dir }}/certs"
    mode: '0755'
openssl_ca_key: "{{ openssl_ca_dir }}/private/ca-{{ openssl_ca_name }}.key"
openssl_ca_crt: "{{ openssl_ca_dir }}/certs/ca-{{ openssl_ca_name }}.crt"
openssl_ca_pass: my-very-secret
</pre></code>

### defaults/Alpine.yml
<pre><code>
# List of required OS packages
openssl_packages:
  - openssl

# List of cryptography packages
openssl_cryptography_packages:
  - py3-cryptography
</pre></code>

### defaults/family-Debian.yml
<pre><code>
# Certificate + key locations
openssl_dirs:
  - path: "{{ openssl_dir }}"
    mode: '0755'
  - path: "{{ openssl_dir }}/private"
    group: ssl-cert
    mode: '0710'
  - path: "{{ openssl_dir }}/certs"
    mode: '0755'

# List of required OS packages
openssl_packages:
  - openssl
  - ssl-cert
  - python3-pip

# List of cryptography packages
openssl_cryptography_packages:
  - python3-cryptography
</pre></code>

### defaults/family-RedHat-7.yml
<pre><code>
# Certificate + key locations
openssl_dir: /etc/pki/tls

# List of required OS packages
openssl_packages:
  - openssl
  - python-pip

# List of cryptography packages
openssl_cryptography_packages:
  - python-cryptography
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
# Certificate + key locations
openssl_dir: /etc/pki/tls

# List of required OS packages
openssl_packages:
  - openssl
  - python3-pip

# List of cryptography packages
openssl_cryptography_packages:
  - python3-cryptography
</pre></code>

### defaults/Fedora.yml
<pre><code>
# Certificate + key locations
openssl_dir: /etc/pki/tls

# List of required OS packages
openssl_packages:
  - openssl
  - python3-pip

# List of cryptography packages
openssl_cryptography_packages:
  - python3-cryptography
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'openssl'
  hosts: ca
  become: 'yes'
  vars:
    python38: false
    python39: false
  roles:
    - deitkrachten.python
  tasks:
    - name: Include role 'openssl'
      ansible.builtin.include_role:
        name: openssl
      vars:
        openssl_type: ca
- name: sample playbook for role 'openssl'
  hosts: servers
  become: 'yes'
  vars:
    python38: false
    python39: false
    openssl_fqdn: server.example.com
    openssl_fqdn_additional:
      - vhost1.example.com
      - vhost2.example.com
  roles:
    - deitkrachten.python
  tasks:
    - name: Include role 'openssl'
      ansible.builtin.include_role:
        name: openssl
      vars:
        openssl_type: server
</pre></code>
