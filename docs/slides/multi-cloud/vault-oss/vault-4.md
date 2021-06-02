name: chapter-4
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Capítulo 4      
## Vault Secrets Engines

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 4 introduces Vault secrets engines
* It focuses on the KV v2 engine.

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-secrets-engines-1
# Vault Secrets Engines

.center[![:scale 65%](images/vault-secrets-engines.png)]
.center[Vault inclui vários tipos de Secret Engine.]

???
* Use this screenshot from the Vault UI to talk about Vault's many secrets engines but note that the next slide lists them too.
* Some are for storing static secrets.
* Others can dynamically generate secrets such as database and cloud credentials.
* There is even one called "Transit" that provides encryption as a service.

---
name:vault-secrets-engines-2
# Vault Secrets Engines Importantes
* Key/Value (KV)
* PKI
* SSH
* TOTP
* Databases
* AWS, Azure, and Google
* Active Directory
* Transit

???
Spend some time pointing out what some of these do:
* KV - Used to manage generic, static secrets. KV v2 supports versioning.
* PKI - Used to generate dynamic X.509 certificates
* SSH - Take all the pain and drudgery out of securing your SSH infrastructure. Vault can provide key signing services that make securing SSH a snap.
* TOTP - The TOTP tool allows Vault to either act as a code-generating device for MFA logins or to provide TOTP server capabilities for MFA infrastructure.
* Databases - Generate dynamic, short-lived database credentials.
* Cloud credentials engines - Generate dynamic, short-lived cloud credentials for major clouds.
* Active Directory - Vault can rotate AD passwords.
* Transit - Implement's Vault's encryption-as-a-service. Provides an API that can handle all your encryption and decryption needs, based on policy, so that you don't have to manage a complicated key infrastructure.

---
name: enabling-secrets-engines
# Habilitando Secrets Engines

* Maior parte das secrets engines do Vault  precisam ser explicitamente habilitadas.
* Isto é feito através do comando `vault secrets enable`.
* Cada secrets engine tem um path padrão.
* Paths alternativos podem ser especificados para múltiplas instâncias:<br> `vault secrets enable -path=aws-east aws`
* Paths personalizados devem ser estepecificados para nos comnados da CLI e chamadas da API:<br>
`vault write aws-east/config/root`<br>
instead of<br>
`vault write aws/config/root`

???

* Talk about enabling secrets engines.
* Talk about default and custom paths
* Explain the examples

---
name: vault-kv-engine
# Vault's KV Secrets Engine
* Vault's KV secrets engine tem na verdade duas 2 versões:
  * KV v1 (sem versionamento)
  * KV v2 (com versionamento)
* Vault não habilita por padrão nenhuma das instâncias de KV secrets engine para servidores no modo "Prod".
* Neste caso, será preciso habilitas essa secret engine manualmente

???
* We already used Vault's Key/Value (KV) engine in the second challenge of the "Vault Basics" Instruqt track that had been automatically enabled for the "Dev" mode server.
* But we'll need to mount it ourselves for the "Prod" mode server.

---
name: vault-kv-commands
# KV Secrets Engine - Comandos
* Use este comando para montar uma instância das engines KV v2 no path padrão `kv`:<br>
`vault secrets enable -version=2 kv`
* Os comandos `vault kv` permitem interagir com as KV engines.
  * `vault kv list` lista segredos num path específico.
  * `vault kv put` escreve um segredo num path específico.
  * `vault kv get` lê um segredo de um path específico.
  * `vault kv delete` remove um segredo de um path específico.
* Outros subcomandos `vault kv` operam em versões de segredos KV v2.

???

* Describe how to mount an instance of the KV v2 secrets engine.
* Describe the various `vault kv` subcommands.
