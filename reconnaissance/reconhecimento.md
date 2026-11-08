## 3. Identificação das Máquinas

| Máquina | Endereço IP |
|---|---|
| Kali Linux | 192.168.56.102 |
| Metasploitable 2 | 192.168.56.101 |

## 4. Teste de Conectividade

Comando utilizado:

ping -c 3 192.168.56.101

Resultado:
Comunicação estabelecida com sucesso.

Observação:
Foram enviados 3 pacotes ICMP e a comunicação
entre as máquinas foi confirmada.


## 5. Varredura de Portas com Nmap

### Comando utilizado

nmap -sV 192.168.56.101

### Resultado

- Host ativo: 192.168.56.101
- Portas abertas: 24
- Portas fechadas: 976
- Ferramenta: Nmap 7.99

### Serviços de interesse

| Porta | Serviço | Versão |
|---|---|---|
| 21 | FTP | vsftpd 2.3.4 |
| 22 | SSH | OpenSSH 4.7p1 |
| 139 | SMB | Samba 3.X - 4.X |
| 445 | SMB | Samba 3.X - 4.X |
| 2121 | FTP | ProFTPD 1.3.1 |
| 3306 | MySQL | 5.0.51a |

### Descoberta crítica

A porta 1524 apresentou um serviço
identificado pelo Nmap como Metasploitable
root shell (bindshell).

Esse serviço representa um risco crítico
de segurança e será registrado para análise.

### Análise

Foram identificados diversos serviços
de versões antigas, que precisam de
avaliação de segurança.

### Status

Reconhecimento inicial concluído.


## 6. Enumeração de Serviços FTP e SMB

### Comando utilizado

nmap -sV -p 21,139,445,2121 --script=banner,smb-os-discovery,smb-protocols 192.168.56.101

### Resultados FTP

- Porta 21: vsFTPd 2.3.4
- Porta 2121: ProFTPD 1.3.1
- Ambos responderam com banner 220.

### Resultados SMB

- Porta 139: Samba 3.X - 4.X
- Porta 445: Samba 3.0.20-Debian
- Grupo de trabalho: WORKGROUP
- Sistema identificado: Unix
- Nome do computador: metasploitable
- Domínio: localdomain

### Protocolo identificado

SMBv1 (NT LM 0.12).

### Análise de segurança

Foi identificada a utilização do SMBv1,
um protocolo legado que apresenta riscos
de segurança.

Recomenda-se desativar o SMBv1 e utilizar
versões modernas do protocolo SMB.

### Conclusão

A enumeração identificou dois serviços FTP
e dois pontos de acesso SMB.


## 7. Verificação de Acesso Anônimo FTP

### Comando utilizado

nmap -p 21,2121 --script ftp-anon 192.168.56.101

### Resultado

Porta 21:
- Estado: Open
- Acesso anônimo: Permitido
- Código FTP: 230

Porta 2121:
- Estado: Open
- Acesso anônimo: Não confirmado pelo teste.

### Vulnerabilidade identificada

O serviço FTP da porta 21 permite
autenticação anônima.


### Recomendação

- Desativar acesso anônimo quando não necessário.
- Restringir permissões de leitura e escrita.
- Utilizar SFTP ou FTPS para transferência segura.
- Monitorar logs de autenticação.

### Status

Acesso anônimo confirmado na porta 21.


## 8. Enumeração de compartilhamentos SMB


### Comando utilizado
nmap -p 139,445 --script smb-enum-shares 192.168.56.101

### Resultados encontrados

Foram identificados cinco compartilhamentos:

- ADMIN$: sem acesso anônimo identificado.
- IPC$: acesso anônimo READ/WRITE.
- opt: sem acesso anônimo identificado.
- print$: sem acesso anônimo identificado.
- tmp: acesso anônimo READ/WRITE.

### Análise técnica

O serviço SMB apresentou compartilhamentos com permissões
de leitura e escrita para acesso anônimo.

Essa configuração representa um risco de segurança,
pois permite operações sem autenticação adequada.

### Conclusão

A enumeração identificou possíveis permissões excessivas
nos compartilhamentos IPC$ e tmp.

É necessário validar os acessos e recomendar restrições
de permissões e autenticação.

### Teste de escrita anônima SMB

Comando utilizado:
mkdir teste_auditoria

Resultado:
Diretório criado com sucesso no compartilhamento tmp,
sem fornecimento de usuário ou senha.

Evidência:
teste_auditoria D 0

Conclusão:
Foi comprovada a permissão de escrita para acesso
anônimo no compartilhamento SMB.

Impacto:
Um usuário não autenticado pode criar diretórios
no compartilhamento, representando risco de exposição
e alteração indevida de dados.