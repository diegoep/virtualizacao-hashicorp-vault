name: chapter-6
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Capítulo de 6   
## Vault Policies

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 6 introduces Vault Policies

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-policies
# Vault Policies
* Policies restringem os segredos à determinados usuários e aplicações
* Seguem uma prática de menos privilégio, *negando* acesso por padrão
* Administradores do Vault podem explicitamente atribuir aceso a usuários e aplicações para paths específicos com policies.
* Além de especificar paths, policies podem também especificar um conjunto de `capabilities` para esses paths (ex.: leitura, escrita, atualização, etc)
* Policies são escritas na linguagem HCL (HashiCorp Configuration Language).

---
name: vault-policy-example
# Exemplo de Policy
* Aqui está um exemplo de Vault policy:
```hcl
# Permite Tokens a terem acesso às suas propriedades
path "auth/token/lookup-self" {
    capabilities = ["read"]
}
```
* Note that this policy does not allow tokens to change their own properties.
???
* This policy allows tokens to look up their own properties

---
name: vault-policy-paths-capabilities
# Paths de Policies e Capabilities
* O path de uma policy mapeia um path da API do Vault.
* As capabilities mais comuns são: `create`, `read`, `update`, `delete`, e `list`, que correspondem aos verbos HTTP como POST e GET.
* Duas outras capabilities não correspondem a verbos HTTP:
  * `sudo` - permite acesso a paths que são protegidos com acesso de root.
  * `deny` - nega acesso a paths e sobrescreve o que foi definido por outras capabilities.

???
* Explain policy paths and capabilities

---
name: vault-policy-commands
# Vault Policy - Comandos da CLI
* Policies podem ser adicionadas a servidores Vault usando CLI, UI ou API.
* O comando para adicionar uma policty pela CLI é `vault policy write`.
* Exemplo de comando que cria uma policy ["lob-A-dept-1" de um arquivo HCL file "lob-A-dept-1-policy.hcl":<br>
`vault policy write lob-A-dept-1 lob-A-dept-1-policy.hcl`
* Exemplo de comando que associa esta policy com um usuário Userpass:<br>
`vault write auth/userpass/users/joe/policies policies=lob-A-dept-1`

???
Describe the most important Vault CLI commands for policies.