# Bosh Vault Release
A Bosh release for the [Bosh Vault config server](https://github.com/Zipcar/bosh-vault).

## Job Configuration
The `bosh-vault` job can be configured with the following properties:
  - `bosh_vault.config` which expects a raw yaml block of configuration for the [bosh-vault binary](https://github.com/Zipcar/bosh-vault)
  - `bosh_vault.tls` which expects an array of cert data that it will use to generate certificates in `/var/vcap/jobs/bosh-vault/tls/`

Here's an example of a functional job entry:
```
      - name: bosh-vault
        release: bosh-vault
        properties:
          bosh_vault:
            config: |
              api:
                address: "0.0.0.0:1337"
                draintimeout: 10
              tls:
                cert: "/var/vcap/jobs/bosh-vault/tls/config-server/cert.pem"
                key: "/var/vcap/jobs/bosh-vault/tls/config-server/key.pem"
              log:
                level: "DEBUG"
              uaa:
                enabled: true
                address: "https://uaa.server.address:8443"
                ca: "/var/vcap/jobs/bosh-vault/tls/uaa-ca/cert.pem"
                skipverify: false
              vault:
                address: https://vault.server.address:8200
            
            tls:
              - name: "uaa-ca"
                cert: |
                  -----BEGIN CERTIFICATE-----
                  ...
                  -----END CERTIFICATE-----
                key: ""
              - name: "config-server"
                cert: |
                  -----BEGIN CERTIFICATE-----
                  ... 
                  -----END CERTIFICATE-----
                key: |
                  -----BEGIN RSA PRIVATE KEY-----
                  ...
                  -----END RSA PRIVATE KEY-----

```