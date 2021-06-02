name: vault-basics-lab
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Vault - Laboratórios Básicos  

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
These slides introduce the Vault Basics track.

---
name: getting-started-with-instruqt
# Fazendo Labs na plataforma Instruqt
* [Instruqt](https://instruqt.com/) é a plataforma utilizada pelos workshops da Hashicorp
* Labs Instruqt rodam em trilhas ("tracks") que são divididas em desafios ("challenges").
* Se você nunca usou Instruqt antes, comece cmo este [tutorial](https://play.instruqt.com/instruqt/tracks/getting-started-with-instruqt).
* Do contrário, vá para o próximo slide
* O Lab "Vault Basics" cobre os conceitos vistos nos Capítulos 2-6.

???
* We'll be using the Instruqt platform for labs in this workshop.
* Don't worry if you've never used it before: there is an easy tutorial that you can run through in 5-10 minutes.
---
name: lab-vault-basics-challenge-1
# 👩‍💻 Challenge 1: Vault CLI
* Neste lab, você irá rodar alguns comandos da Vault CLI.
* Você deve fazer isso no primeiro "challenge", chamado "The Vault CLI", da trilha [Vault Basics](https://play.instruqt.com/hashicorp/invite/qfwncq62zsxu) do Instruqt.

???
* Now, you can try running some Vault CLI commands yourself in the first challenge of our first Instruqt track in this workshop.
* This lab covers concepts introduced in chapters 2-6.

---
name:lab-vault-basics-challenge-1-instructions
# 👩‍💻 Challenge 1: Instruções
* Inicie a trilha "Vault Basics" clicando no botão "Start" do desafio "Vault CLI".
* Enquanto o desafio carrega, leia o texto que é exibido.
* Clique no botão verde "Start" para iniciar o desafio "Vault CLI".
* Siga as intruções no lado direito do desafio.
* Depois de completar todos os passos, clique no botão verde "Check" para verificar se você fez tudo corretamente.
* Você pode também clicar no botão "Check" para saber detalhes sobre o que precisa ser feito.

???
* Give the students some instructions for starting their first challenge.
* This also includes instructions for checking that they did everything right.
* Students can also click the green "Check" button to get reminders of what they should do next.

---
name: lab-vault-basics-challenge-2
# 👩‍💻 Challenge 2: Rodando um servidor Vault no modo "Dev"
* Neste lab, você irá rodar seu primeiro servidor vault no modo "Dev"
* Você irá também escrever o seu primeiro segredo no Vault e usar a UI
* Instruções:
  * Se a trilha não iniciar sozinha, clique no desafio "Your First Secret" da trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Your First Secret" challenge of the "Vault Basics" track.
* This challenge has them run a Dev server, write a secret to the KV v2 secrets engine that was automatically enabled, and use the Vault UI.

---
name: lab-vault-basics-challenge-3
# 👩‍💻 Challenge 3: Use a Vault HTTP API
* Neste lab, você irá usar a API Vault do HTTP.
* Você irá primeiro checar a saúde do seu servidor Vault.
* Você irá então ler seu segredo `my-first-secret` do Vault.
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "The Vault API" na trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.
???
* Instruct the students to do the challenge, "The Vault API", in the "Vault Basics" track.

---
name: lab-vault-basics-challenge-4
# 👩‍💻 Challenge 4: Rode um servidor Vault no modo "Prod"
* Neste lab, você urá rodar seu primeiro servidor Vault no modo "Prod".
* Você irá aprender a inicializar e fazer unseal de um servidor Vault.
* Instructions:
  * Se a trilha não começar sozinha, clique no desafio "Run a Production Server" na trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Run a Production Server" challenge of the "Vault Basics" track.
* This challenge has them examine a Vault server configuration file, run a Prod server, initialize it, and unseal it.
* Remind students to save their unseal key and root token.

---
name: lab-vault-basics-challenge-5
# 👩‍💻 Challenge 5: Use a secret Engine KV v2
* Neste lab, você irá habilitar e usar a secret engine KV v2.
* Note que o path será `kv` ao invés de `secret`.
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "Use the KV V2 Secrets Engine" na trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Use the KV V2 Secrets Engine" challenge of the "Vault Basics" track.
* This challenge has them enable an instance of the KV v2 secrets engine.
* Emphasize that the path will be `kv` instead of `secret` as was the case for the challenges with the Dev mode server.

---
name: lab-vault-basics-challenge-6
# 👩‍💻 Challenge 6: Método de autenticação Userpass
* Neste lab, você irá habilitar e usar o método de autenticação Userpass
* Instruções:
  * Se a trilha não começar sozinha, clique no desafio "Use the Userpass Auth Method" na trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Use the Userpass Auth Method" challenge of the "Vault Basics" track.
* This challenge has them enable an instance of the Userpass auth method.
* It also demonstrates that Vault is "deny by default" since the Userpass user that they create will not have any access to secrets yet.

---
name: lab-vault-basics-challenge-7
# 👩‍💻 Challenge 7: Vault Policies
* Neste lab, você irá usar as Vault policies para atribuir acesso de diferentes usuários a diferentes secrets.
* Instructions:
  * Se a trilha não começar sozinha, clique no desafio "Use Vault Policies" na trilha "Vault Basics".
  * Em seguida clique no botão verde "Start" para iniciar.
  * Siga as instruções do desafio.
  * Clique no botão verde "Check" quando finalizar.

???
* Instruct the students to do the "Use Vault Policies" challenge of the "Vault Basics" track.
* This challenge has them create a second user and create and associate policies with their 2 users.
* It then has them valdiate that each user can only access their own secrets.
