storage backend 
  - all AES 256, - what happens 5 - 10 years down the line when that's not good enough?
  - one per cluster
secret backend
  - can be multiple 
  - "kind of a layer on top of storage"
  - PKI acts as a full CA
  - transit - allows running in & out of vault without ever exposing plaintext
audit backend
  - can be multiple 
  - must succeed before any other access works
auth backend
  - how a user or machine authenticates
  - token returned on user auth.

Generic secret backend being renamed to key-value

root token used to set up other auth backends, then delete

generic backend automounted at /secret, though it can be mounted elsewhere

vault -h   #CLI help
vault path-help  # API help

cubbyhole/

policy
======
default ACLS:
root (superuser)
default (most tokens will have this)
  - renew self
  - view own cubbyhole

default deny
default policy does not have permissions to create policy

vault policy-write base ./base.hcl
vault write sys/policy/base rules=@base.hcl

Seth blog post on vault.io re vault configuration

```
v token-create -policy=base
Key             Value
---             -----
token           98d07b3b-3245-92bb-ead2-5301c623d29c
token_accessor  84232912-6a8f-7138-835a-c65b826afd89
token_duration  768h0m0s
token_renewable true
token_policies  [base default]
```
v auth $token

dynamic secrets
===============

v mount database
postgres, mongo, mssql, rabbitmq, etc, etc, plugins too

 vault write database/config/postgresql \
> plugin_name=postgresql-database-plugin \
> allowed_roles=readonly \
> connection_url=postgresql://postgres@localhost/myapp


The following warnings were returned from the Vault server:
* Read access to this endpoint should be controlled via ACLs as it will return the connection details as is, including passwords, if any.

web -> vault 
vault -> sql_server
web <- vault (user, password for sql)
web -> sql_server

vault write database/roles/readonly db_name=postgresql creation_statements=@readonly.sql default_ttl=1h max_ttl=24h
Success! Data written to: database/roles/readonly
training@hashicorp > vault read database/creds/readonly
Key             Value
---             -----
lease_id        database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
lease_duration  1h0m0s
lease_renewable true
password        A1a-9qyyx5u2trry9r9v
username        v-token-readonly-tw78r9ss2q3q87810u8y-1505751629

psql -U postgres
Welcome to PostgreSQL for HashiCorp training!
Type \q to exit.

postgres > \du
                                                       List of roles
                    Role name                     |                         Attributes                         | Member of
--------------------------------------------------+------------------------------------------------------------+-----------
 postgres                                         | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 v-token-readonly-spx50v4910st2u8xu6q2-1505751572 | Password valid until 2017-09-18 17:19:32+00                | {}
 v-token-readonly-tw78r9ss2q3q87810u8y-1505751629 | Password valid until 2017-09-18 17:20:29+00                | {}

training@hashicorp > cat readonly.sql
CREATE ROLE "{{name}}" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO "{{name}}";

vault renew database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
vault revoke database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
Success! Revoked the secret with ID 'database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143', if it existed.
training@hashicorp > vault revoke -prefix database/creds/readonly
Success! Revoked the secret with ID 'database/creds/readonly', if it existed.


Leases
======
Leases are hierarchical - get a token, create some tokens - revoke top level token,
and all below that are revokes. Applies to TTL also

Workflow Questions
==================
Is there logging in before getting tokens?
I give the apps vault logins with limited abilities 
How do I limit those abilities?
I give devs logins with limited abilities

best-practice:
renew at half of the time of the lease. (ie 10 m lease, renew at 5)
attempt to reread a new token if renew fails

root/sudo userr can make an "orphan" token (not a child of the parent)

web would have a vault token and a sql token, *both* will need to be renewed.

auth backends always create orphan tokene
persistent token - TTL but no MAX_TTL

use_counts (use it only n times. can also have TTL)

Auth backends
============
* v auth-enable
* configure backend (varies)
* map authentication to a policy

vault auth-enable userpass
  or
vault write sys/auth/userpass type=userpass

vault auth-enable -path=training-userpass userpass
vault write sys/auth/training-userpass type=userpass

v write auth/userpass/users/nbirnel password=foobar policies=base

cat contractor.hcl
path "database/creds/readonly" {
  capabilities = [ "read" ]
}

v write auth/userpass/users/nbirnel password=foobar policies=base

cat contractor.hcl
vault auth -method=userpass username=sandy
Password (will be hidden):



database/creds/readonly
Key             Value
---             -----
lease_id        database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
lease_duration  1h0m0s
lease_renewable true
password        A1a-9qyyx5u2trry9r9v
username        v-token-readonly-tw78r9ss2q3q87810u8y-1505751629

psql -U postgres
Welcome to PostgreSQL for HashiCorp training!
Type \q to exit.

postgres > \du
                                                       List of roles
                    Role name                     |                         Attributes                         | Member of
--------------------------------------------------+------------------------------------------------------------+-----------
 postgres                                         | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 v-token-readonly-spx50v4910st2u8xu6q2-1505751572 | Password valid until 2017-09-18 17:19:32+00                | {}
 v-token-readonly-tw78r9ss2q3q87810u8y-1505751629 | Password valid until 2017-09-18 17:20:29+00                | {}

training@hashicorp > cat readonly.sql
CREATE ROLE "{{name}}" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO "{{name}}";

vault renew database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
vault revoke database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143
Success! Revoked the secret with ID 'database/creds/readonly/74a17532-5d04-1f75-3c0c-0b23dbc2a143', if it existed.
training@hashicorp > vault revoke -prefix database/creds/readonly
Success! Revoked the secret with ID 'database/creds/readonly', if it existed.


Leases
======
Leases are hierarchical - get a token, create some tokens - revoke top level token,
and all below that are revokes. Applies to TTL also

Workflow Questions
==================
Is there logging in before getting tokens?
I give the apps vault logins with limited abilities
How do I limit those abilities?
I give devs logins with limited abilities

best-practice:
renew at half of the time of the lease. (ie 10 m lease, renew at 5)
attempt to reread a new token if renew fails

root/sudo userr can make an "orphan" token (not a child of the parent)

web would have a vault token and a sql token, *both* will need to be renewed.

auth backends always create orphan tokene
persistent token - TTL but no MAX_TTL

use_counts (use it only n times. can also have TTL)

Auth backends
============
* v auth-enable
* configure backend (varies)
* map authentication to a policy

vault auth-enable userpass
  or
vault write sys/auth/userpass type=userpass

vault auth-enable -path=training-userpass userpass
vault write sys/auth/training-userpass type=userpass

Audit backends
==============
sensitive info HMACed

v audit-enable file file_path=/workstation/vault/audit.log

Operating Vault
==============
Config file: define 1 storage backend, 1+ listensers
scheduler ( nomad, k8s, etc) or systemd/upstart
Need to initialize before running.
ls -a

# Use the file storage - this will write encrypted data to disk.
storage "file" {
  path = "/workstation/vault/data"
}

# Listen on a different port (8201), which will allow us to run multiple
# Vault's simultaneously.
listener "tcp" {
  address     = "127.0.0.1:8201"
  tls_disable = 0
}

File rarely used in prod, since you can't get HA

/etc/systemd/system/vault.service:
[Unit]
Description=Vault Remote
Documentation=https://www.vaultproject.io/docs/
Requires=network-online.target
After=network-online.target

[Service]
Environment=GOMAXPROCS=8
Restart=on-failure
ExecStart=/usr/local/bin/vault server -config=/workstation/vault/config.hcl
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target

Once per storage backend:
vault init -address=http://127.0.0.1:8201
Unseal Key 1: BohZ9OjRpNogmZOC7Q6GthXj/lrDgJbLoNT9Q26JoNAq
Unseal Key 2: P8aIRuYZ6PsUeu9wetRN00/j9SS+wRpMFUDEjHQA+wTp
Unseal Key 3: ymkEYQMowgi1D6ATnVV1AcnhtMmOwyua/uva0gtBkUfH
Unseal Key 4: R/dleog/+NNIi9VlWD0ElgQjNLAR3oUbUIUQIchkOQxR
Unseal Key 5: QpLFYbHdzCfQYobWGqp2fp7V6DEmlPmKXzK0akLbciGD
Initial Root Token: 6eccb4e9-89ea-808f-bc10-6ff057ed844c

Vault initialized with 5 keys and a key threshold of 3. Please
securely distribute the above keys. When the vault is re-sealed,
restarted, or stopped, you must provide at least 3 of these keys
to unseal it again.

Vault does not store the master key. Without at least 3 keys,
your vault will remain permanently sealed.

So this looks like a problem: I have all 5 keys.
vault unseal -address=http://127.0.0.1:8201 

Recommend multiple people watch the init (keys and root)

Vault HA
========
one for per HA storage backend
init the first one
unseal all of them
HA, not load balancing. Backend must be able to lock. (Consul -> 3 vaults)

Standby mode machines forward to active node. 

Regenerate Root
===============
after a subset of admins have 'sudo' (real sudo?) access, recommend revoke
the root token.
A few items may still need root-token.

A quorum of unseal keys can generate a new root.
vault generate-root
- unseal the vault
- generate a one-time-password
- each quorum user runs vault generate-root
  use OTP fo decrypt the root passwrord

 vault generate-root -genotp -address=http://127.0.0.1:8201
 OTP: vH2aAtdGo51EsoJZJXVukg==
vault generate-root -address=http://127.0.0.1:8201 -otp=vH2aAtdGo51EsoJZJXVukg==
vault generate-root -address=http://127.0.0.1:8201
vault generate-root -address=http://127.0.0.1:8201
Encoded root token: bYMqLyD3Fo8IL+BHQ/aqhw==
decrypt with otp how?

API
===

vault write secret/foo bar=1
curl $VAULT_ADDRESS/v1/secret/ \
  --request POST \ 
  --header = "X-Vault-Token: blahblah" \
  --data '{"bar":"1"}'

  list = LIST
  read = GET


curl $VAULT_ADDR/v1/auth/userpass/login/sandy --request POST  --data '{"password": "training"}'
{"request_id":"a84eac9c-e397-a0f6-9a72-8d92a51a3d7a","lease_id":"","renewable":false,"lease_duration":0,"data":null,"wrap_info":null,"warnings":null,"auth":{"client_token":"a9e25712-7fd1-d499-50bc-493de900e287","accessor":"5c60b733-55ae-00a8-7d18-3db178c93772","policies":["contractor","default"],"metadata":{"username":"sandy"},"lease_duration":2764800,"renewable":true}}

Consul Template
===============
github/hashicorp/consul-template
doesn't need consul...
needs a vault token 

writes files, which might be reloaded on change

wrap application in a consul-template call

cat config.yml.tpl
---
{{- with secret "database/creds/readonly" }}
username: "{{ .Data.username }}"
password: "{{ .Data.password }}"
database: "myapp"
{{- end }}
VAULT_TOKEN='fa0c04cf-2ed1-97ff-76b0-1b8bf478303d' consul-template --template="config.yml.tpl:config.yml"

{{ with secret "secret/training" }}

{{ range $key, $value := .Data }}
{{- $key }}:{{ $value }}
{{ end }}
{{ end }}
~

Envconsul
=========
envconsul offers environmental variables to apps, for (load-time) env-variable

VAULT_TOKEN=b06845df-f6aa-1f1b-a044-379a7e509b5b envconsul -upcase -secret database/creds/readonly ./app.sh
2017/09/18 20:50:26.230066 looking at vault database/creds/readonly
My connection info is:

  username: "v-token-readonly-2tw58w2wtqu48uu1wr04-1505767826"
  password: "A1a-28qtrw3xq2rv059y"
  database: "my-app"

cat app.sh
#!/usr/bin/env bash

cat <<EOT
My connection info is:

  username: "${DATABASE_CREDS_READONLY_USERNAME}"
  password: "${DATABASE_CREDS_READONLY_PASSWORD}"
  database: "my-app"
EOT

