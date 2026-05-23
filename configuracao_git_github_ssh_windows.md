# Configuração do Git e GitHub via SSH no Windows

Este documento registra todos os passos feitos para verificar, configurar e habilitar a conexão entre o computador local e o GitHub usando Git + SSH.

---

## 1. Verificar se o Git estava instalado

Primeiro foi aberto o terminal do Windows (`cmd`) e executado o comando:

```bash
git --version
```

Resultado obtido:

```bash
git version 2.41.0.windows.1
```

### O que isso significa?

Esse resultado confirma que o Git já estava instalado no computador e disponível no terminal.

---

## 2. Verificar o nome configurado no Git

Depois foi verificado o nome global configurado no Git:

```bash
git config --global user.name
```

Resultado obtido:

```bash
Wellington
```

### O que isso significa?

Esse nome será usado nos commits feitos pelo Git. Cada commit fica associado a um autor.

---

## 3. Verificar o e-mail configurado no Git

Em seguida foi verificado o e-mail global configurado no Git:

```bash
git config --global user.email
```

Resultado obtido:

```bash
tonzimcosta16@gmail.com
```

### O que isso significa?

Esse e-mail também fica associado aos commits. É importante que ele esteja ligado à conta do GitHub para manter a autoria dos projetos corretamente registrada.

---

## 4. Verificar se o GitHub CLI estava instalado

Foi executado o comando:

```bash
gh --version
```

Resultado obtido:

```bash
'gh' não é reconhecido como um comando interno
ou externo, um programa operável ou um arquivo em lotes.
```

### O que isso significa?

O GitHub CLI (`gh`) não estava instalado no computador.

Isso não impede o uso do Git com GitHub. É possível usar normalmente:

- Git pelo terminal;
- GitHub pelo navegador;
- VS Code integrado ao Git;
- autenticação via SSH.

---

## 5. Testar conexão SSH com o GitHub

Foi executado o comando:

```bash
ssh -T git@github.com
```

Na primeira tentativa, o terminal mostrou:

```bash
The authenticity of host 'github.com (4.228.31.150)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Foi digitado:

```bash
yes
```

Depois apareceu:

```bash
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

### O que isso significa?

A primeira parte confirmou que o computador passou a reconhecer o GitHub como servidor confiável.

A segunda parte indicou que ainda não havia uma chave SSH cadastrada no GitHub:

```bash
Permission denied (publickey)
```

Ou seja, o computador tentou se autenticar, mas o GitHub ainda não reconhecia uma chave pública autorizada.

---

## 6. Verificar a pasta `.ssh`

Foi executado o comando:

```bash
dir %USERPROFILE%\.ssh
```

Resultado inicial:

```bash
Pasta de C:\Users\coswe\.ssh

22/05/2026  13:44    <DIR>          .
22/05/2026  13:44    <DIR>          ..
22/05/2026  13:44                93 known_hosts
```

### O que isso significa?

A pasta `.ssh` existia, mas ainda não havia uma chave SSH criada.

O arquivo existente era apenas:

```bash
known_hosts
```

Esse arquivo registra servidores SSH já reconhecidos pelo computador, como o GitHub.

---

## 7. Criar uma chave SSH

Foi executado o comando:

```bash
ssh-keygen -t ed25519 -C "tonzimcosta16@gmail.com"
```

Esse comando cria um par de chaves SSH usando o algoritmo `ed25519`.

Durante o processo, o terminal perguntou onde salvar a chave:

```bash
Enter file in which to save the key (C:\Users\coswe/.ssh/id_ed25519):
```

A orientação foi apenas pressionar `Enter`, aceitando o local padrão.

Depois, ao tentar executar de novo, apareceu:

```bash
C:\Users\coswe/.ssh/id_ed25519 already exists.
Overwrite (y/n)?
```

### O que isso significa?

A chave SSH já havia sido criada. Não foi necessário sobrescrever.

---

## 8. Confirmar que a chave SSH foi criada

Foi executado novamente:

```bash
dir %USERPROFILE%\.ssh
```

Resultado obtido:

```bash
Pasta de C:\Users\coswe\.ssh

22/05/2026  13:48    <DIR>          .
22/05/2026  13:44    <DIR>          ..
22/05/2026  13:48               419 id_ed25519
22/05/2026  13:48               106 id_ed25519.pub
22/05/2026  13:44                93 known_hosts
```

### O que isso significa?

Agora a pasta `.ssh` continha os arquivos necessários:

```bash
id_ed25519
id_ed25519.pub
known_hosts
```

Explicação dos arquivos:

```text
id_ed25519      = chave privada, fica apenas no computador e nunca deve ser compartilhada.
id_ed25519.pub  = chave pública, pode ser cadastrada no GitHub.
known_hosts     = arquivo que registra servidores SSH confiáveis.
```

---

## 9. Exibir a chave pública

Foi executado o comando:

```bash
type %USERPROFILE%\.ssh\id_ed25519.pub
```

Resultado obtido:

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKsiCekeVoqZ1o6zeONgiYiXm8APkqxqWak8021QftpD tonzimcosta16@gmail.com
```

### O que isso significa?

Essa é a chave pública. Ela pode ser cadastrada no GitHub.

Atenção:

```text
A chave pública termina com .pub e pode ser compartilhada/cadastrada.
A chave privada não tem .pub e nunca deve ser compartilhada.
```

---

## 10. Cadastrar a chave pública no GitHub

A chave pública foi cadastrada no GitHub pelo navegador.

Caminho usado no GitHub:

```text
Foto do perfil
Settings
SSH and GPG keys
New SSH key
```

Configuração:

```text
Title: Meu notebook Windows
Key type: Authentication Key
Key: conteúdo completo do arquivo id_ed25519.pub
```

Depois foi clicado em:

```text
Add SSH key
```

### O que isso faz?

Isso permite que o GitHub reconheça o computador como autorizado para interagir com os repositórios da conta.

---

## 11. Testar novamente a conexão SSH

Depois de cadastrar a chave pública no GitHub, foi executado novamente:

```bash
ssh -T git@github.com
```

Resultado final:

```bash
Hi WellingtonCosta1! You've successfully authenticated, but GitHub does not provide shell access.
```

### O que isso significa?

Esse resultado confirma que a autenticação SSH com o GitHub funcionou.

A parte:

```bash
You've successfully authenticated
```

significa que o computador foi reconhecido com sucesso pelo GitHub.

A parte:

```bash
but GitHub does not provide shell access
```

não é erro. Apenas significa que o GitHub não permite acessar um terminal remoto. Para uso com Git, está tudo correto.

---

## 12. Estado final da configuração

Ao final do processo, o ambiente ficou assim:

```text
Git instalado: OK
Nome configurado no Git: OK
E-mail configurado no Git: OK
Pasta .ssh existente: OK
Chave SSH criada: OK
Chave pública cadastrada no GitHub: OK
Autenticação SSH testada: OK
```

---

## 13. Comandos usados no processo

```bash
git --version
git config --global user.name
git config --global user.email
gh --version
ssh -T git@github.com
dir %USERPROFILE%\.ssh
ssh-keygen -t ed25519 -C "tonzimcosta16@gmail.com"
dir %USERPROFILE%\.ssh
type %USERPROFILE%\.ssh\id_ed25519.pub
ssh -T git@github.com
```

---

## 14. Conceito principal

A configuração feita criou uma ponte segura entre o computador local e o GitHub.

A lógica é:

```text
Git controla versões no computador.
GitHub armazena os repositórios online.
SSH permite autenticar o computador com segurança.
```

Depois dessa configuração, o computador pode se comunicar com o GitHub para operações como:

```bash
git clone
git push
git pull
```

---

## 15. Próximos comandos importantes para estudar

Depois dessa etapa, os comandos principais do Git que devem ser estudados são:

```bash
git init
git status
git add .
git commit -m "mensagem"
git remote add origin
git push
git pull
git clone
git branch
git checkout
```

Esses comandos formam a base para trabalhar com projetos no GitHub e, depois, com ferramentas como Claude Code.
