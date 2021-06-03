name: chapter-7
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Capítulo 7    
## Dynamic Database Secrets

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 7 introduces Vault's Database secrets engine which can dynamically generate short-lived credentials for various databases.

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: dynamic-database-secrets
# Dynamic Secrets: Protegendo Bancos de Dados

* Credenciais de acesso a bancos de dados geralmetne são de longa duração.
* A Database Secret Engine do Vault  gera dinamicamente credenciais de curta duração para bancos de dados.
* Suporta configuração de conexões e papéis com diferentes permissões e tempo de vida.
* Usuários ou aplicações requisitam credenciais que estão vinculadas a um papel específico registrado no Vault.
* Vault gerencia o ciclo de vida dessas credenciais, removendo-as automaticamente do banco de dados quando o tempo de vida (time-to-live) expira.

???
* Vault's Database secrets engine supports dynamic generation of short-lived credentials (usernames and passwords) for databases.
* This avoids storing long-lived or permanent credentials on app servers that can easily be compromised.
* Short-lived credentials are much more secure since ex-employees and others are very unlikely to know the current values.

---
name: database-engine-plugins
# Database Secrets Engine: Plugins
* Cassandra
* Elasticsearch
* Influxdb
* HanaDB
* MongoDB
* MSSQL
* MySQL/MariaDB
* PostgreSQL
* Oracle

???
* The database secrets engine has out-of-the-box plugins for many databases.
* Custom plugins can also be built.

---
name: database-engine-workflow
# Database Secrets Engine - Workflow
1. Habilitar uma instância de uma database secret engine.
1. Configurá-la com o plugin correto e uma URL de conexão, usando uma service account criada pelo Vault.
1. Criar um ou mais roles com TTLs e statements SQL que especificam as permissões requeridas.
1. Aplicações e usuários podem requisitar credenciais do Vault que são válidas para o TTL padrão da role, mas que podem ser renovadas até um TTL máximo.
1. Vault remove automaticamente credenciais expiradas.
1. Se credenciais foram vazadas, é possível revogá-las imediatamente.

???
* This slide lays out the basic workflow used for all of the Datbase secrets engine plugins.
* All of the plugins work the same basic way.
* A service account with permissions to manage users on the database server is required by each connection.
* User creation and revocation SQL statements are specified for roles to determine the permissions og generated users within various databases.
* Multiple connections and roles can be created for a single secrets engine instance to support connecting to multiple database servers with different levels of access.
* The TTL settings can be tuned to suit your needs.

---
# Configurando Database Secret Engine para o MySQL
1. Habilite a database secrets engine e algum path (ex.: /databases).
1. Configure o path com o plugin do Mysql, URL de conexão, nome do usuário, senha e roles permitidas.
1. Rotacione as credenciais de "root": Vault modifica a senha repassada na etapa 2, de maneira que humanos não a conhecem mais.
1. Crie roles que podem criar nova credenciais que são válidas para um período específico de tempo.

???
* These are the basic steps for configuring the mysql plugin with Vault's database secrets engine.
* The username and password set on the config path must already exist and have permission to manage users.

---
name: mysql-config-connection
class: compact
# Configurando Conexões para o MySQL
#### Rode esses comandos para habilitar a Database secrets engine e configurar uma conexão para uso com MySQL:
```bash
vault secrets enable -path=lob_a/workshop/database database

vault write lob_a/workshop/database/config/wsmysqldatabase \
    plugin_name=mysql-database-plugin \
    connection_url="{{username}}:{{password}}@tcp(localhost:3306)/" \
    allowed_roles="workshop-app","workshop-app-long" \
    username="hashicorp" \
    password="Password123"

vault write -force lob_a/workshop/database/rotate-root/wsmysqldatabase
```
#### Isto cria uma conexão chamada "wsmysqldatabase" registrada no servidor MySQL rodando em localhost.

???
* This slide shows the commands to enable the Database secrets engine and configure a connection for MySQL.
* We specified a number of things in the configuration:
    * The path someone would call: "lob_a/workshop/database"
    * The name of the database the role can interact with: wsmysqldatabase
    * The connection URL
    * The initial username and password
    * The roles that can be used with this connection
* We then rotated the password for the "root" user so that only Vault knows it.

---
name: rotating-root-credentials
class: compact
# Rotacionando as credenciais de Root para o MySQL
#### 1. Você **não** deve usar a senha do usuário root user do banco de dados; ao invés, crie um usuário separado com permissõesoes suficientes para criar outros usuários e mudar sua própria senha. Isto pode ser feito através do comando `GRANT ALL PRIVILEGES on *.* to 'usuario'@'%' with grant option;` para o usuário.
#### 2. O nome de usuário que é fornecido estaria vinculado ao host `'%'`. Então, crie um usuário como `'usuario'@'%'` ao invés de `'usuario'@'localhost'`.
#### 3. Se não deseja utilizar `'%'` como host para o usuário, você pode especiifcar `root_rotation_statements` quando estiver escrevendo  o caminho `<database>/config/<connection>`; por exemplo, você pode atribuir a `"ALTER USER '{{username}}'@'localhost' IDENTIFIED BY '{{password}}';"`.

???
* We want to give some advice about rotating root credentials for the database secrets engine when using MySQL.

---
class:compact
# Configurando Roles para o MySQL
#### Rode este comando para configurar uma role para o MySQL:
```sql
vault write lob_a/workshop/database/roles/workshop-app-long \
    db_name=wsmysqldatabase \
    creation_statements="CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';
    GRANT ALL ON my_app.* TO '{{name}}'@'%';" \
    default_ttl="1h" \
    max_ttl="24h"
```
#### Isto define a configuração de uma role em cima da conexão ao banco "wsmysqldatabase", que irá gerar credenciais com uma TTL inicial de 1h, mas o tempo de vida total pode ser estendido até 24h.

???
* We specified a number of things:
    * The creation statements that define the capabilities of the userd that are created
    * The default time to live for generated users
    * The maximum duration for generated users

---
name: mysql-generate-creds
class:compact
# Gerando Credenciais de Banco de Dados
#### Rode este comando para gerar credenciais válidas para o banco de dados MySQL utilizando a role que foi configurada no slide anterior:
```bash
vault read lob_a/workshop/database/creds/workshop-app-long  
```
#### Isto deve retornar algo como:<br>
```bash
Key                Value
---                -----
lease_id           lob_a/workshop/database/creds/workshop-app-long/JeUGIL2xD6BzXSneqity8UmF
lease_duration     1h
lease_renewable    true
password           A1a-zy4ENaf2kwpzGk9t
username           v-token-workshop-a-DM0BJ3eMlMhbf
```

???
* Now, we can begin generating credentials for our MySQL database.

---
name: mysql-renew-revoke-creds
class:compact
# Renovando e Rovogando Credenciais
#### Rode este comando para renovar credenciais, substituindo o `<lease_id>` pelo lease_id correto:
```bash
vault write sys/leases/renew lease_id="<lease_id>" increment="120"  
```
#### Rode este comando para revogar credencias, substituindo o `<lease_id>` pelo lease_id correto:
```bash
vault write sys/leases/revoke lease_id="<lease_id>"
```
#### Você pode determinar o tempo de vida restante das credenciais:
```bash
vault write sys/leases/lookup lease_id="<lease_id>"
```

???
* These are the commands to renew and revoke Vault leases.
* When you run the `renew` command, Vault extends the lifetime of the credentials.
* When you run the `revoke` command, Vault revokes the lease and removes the credentials from the database server.
* It is also possible to determine the remaining lifetime of credentials.

---
name: lab-database-challenge-1
# 👩‍💻 Challenge 1: Habilitar a Database Secret Engine
* Neste lab, você irá habilitar database engine para MySQL e rotacionar as credenciais de root.
* você irá fazer isso na trilha [Vault Dynamic Database Credentials](https://play.instruqt.com/hashicorp/invite/sryhqfdm6sgx) do Instruqt.
* Instructions:
  * Clique no desafio "Enable the Database Secrets Engine" da trilha "Vault Dynamic Database Credentials".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.
???
* Instruct the students to do the "Enable the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them enable the Database secrets engine on the path "lob_a/workshop/database" and rotate the root credentials for it.

---
name: lab-database-challenge-2
# 👩‍💻 Challenge 2: Configurar a Engine
* Neste lab, você irá configurar uma conexão e duas roles para o banco de dados.
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "Configure the Database Secrets Engine" na trilha "Vault Dynamic Database Credentials".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Configure the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them configure a connection and two roles for the engine they created in the previous challenge.
* One role will have an initial TTL of 1 hour with a maximum TTL of 24 hours.
* The other will have an initial TTL of 3 minutes with a maximum TTL of 6 minutes.

---
name: lab-database-challenge-3
# 👩‍💻 Challenge 3: Gerar Credenciais
* Neste lab, você irá gerar e usar credenciais de acordo com as roles que foram configuradas no desafio anterior.
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "Generate and Use Dynamic Database Credentials" na trilha "Vault Dynamic Database Credentials".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.


???
* Instruct the students to do the "Generate and Use Dynamic Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them generate short-lived credentials for the MySQL database.
* They will use the `mysql` utility to connect to the database with those credentials.
* They will see that the credentials are deleted after 3 minutes and that logging into MySQL with them is blocked.

---
name: lab-database-challenge-4
# 👩‍💻 Challenge 4: Renovar a Revogar Credenciais
* Neste lab, você irá renovar e revogar credenciais geradas pela database secrets engine.
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "Renew and Revoke Database Credentials" na trilha "Vault Dynamic Database Credentials".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Renew and Revoke Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them extend the liftime of generated credentials with Vault's `sys/leases/renew` endpoint.
* They will also revoke credentials with Vault's `sys/leases/revoke` endpoint.


