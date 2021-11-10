# ansible-role-openssl

## Self-signed-certificate

```
---
- hosts: all
  roles:
    - ansible-role-openssl
```

## TODO

- Create server certificate sign-request for external CA
- Setup optional internal Certificate Authority
- Setup client/server certificates, signed by the internal CA
