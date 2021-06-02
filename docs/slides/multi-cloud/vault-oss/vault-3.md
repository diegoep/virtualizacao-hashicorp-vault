name: Chapter-3
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Capítulo 3      
## Rodando um servidor Vault para Produção

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

Chapter 3 focuses on running a production Vault server

---
layout: true

.footer[
- Copyright © 2021 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-production-serves
# Running a Production Vault Server
* Rodar um servidor Vault no modo "Prod" envolve múltiplos passos:
  * Especificar a configuração num arquivo de propriedades
  * Iniciar o servidor
  * Iniciar o servidor para pegar as `unseal keys` e um token raiz inicial
  * Fazer `unseal` do servidor Vault utilizando as `unseal keys`.

???
* Describe the steps to run a production Vault server.

---
name: configuring-vault
# Configurando os Servidores Vault
* Os arquivos de configuração do Vault podem ser especificados em [HCL](https://github.com/hashicorp/hcl) ou JSON.
* Configuração comum inclui:
  * listener
  * storage
  * seal
  * log_level
  * ui
  * api_addr
  * cluster_addr

???
* Discuss Vault configuration files and common settings.

---
name: running-vault
# Iniciando um servidor Vault para Produção
* Você pode usar o comando `vault server` para iniciar um servidor Vault para produção
* Para isto, basta não utilizada a opção `-dev`.

???
* Describe the command to run a Vault production server.

---
name: initializing-vault
# Inicializando Clusters Vault
* Lembre-se que um cluster Vault roda múltiplos servidore Vault.
* Cada cluster Vault cluster deve ser inicializado uma vez.
* Isto é feito através do comando `vault operator init`.
* O número de chaves compartilhadas e thresholds pode ser especificado com as opções `-key-shares` e `key-threshold`.
* O comando retorna as `unseal keys` e o token raiz inicial para o cluster.

???
* Describe Vault's `init` command

---
name: unsealing-vault
# Fazendo unseal de servidores Vault
* Cada Vault deve ser desbloqueado (unseal) cada vez que é inicializado
* Não é possível utilizado o servidor até que ele seja desbloqueado
* Isto é feito com o comando `vault operator unseal`, usando as `unseal keys` retornadas quando o cluster foi iniciado.

???
* Describe Vault's `unseal` command.

---
name: vault-status-command
# Determinando o estado de um servidor Vault
* Use o comando `vault status` para pegar informações sobre um servidor Vault.
* Isto irá informar se o servidor Vault está selado ou liberado.
* Isto também irá informar o seguinte:
  * número de chaves compartilhadas e key thresholds
  * se está rodando no modo HA mode (clustering)
  * se o servidor está rodando como performance standby.

???
Describe the `vault status` command