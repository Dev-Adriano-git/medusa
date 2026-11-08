# Auditoria de Autenticação FTP com Medusa

**Projeto:** Laboratório de Auditoria de Segurança  
**Ferramenta principal:** Medusa v2.3  
**Sistema de origem:** Kali Linux  
**Sistema analisado:** Metasploitable 2  
**Serviço:** FTP  
**Porta:** 21  
**Status:** Teste concluído

---

## 1. Contextualização

Esta etapa do laboratório teve como finalidade avaliar o mecanismo de autenticação de um serviço FTP utilizando a ferramenta Medusa.

A atividade foi conduzida em ambiente virtual próprio, com foco na identificação de possíveis fragilidades relacionadas ao uso de credenciais padrão e na compreensão do funcionamento de ferramentas de auditoria de autenticação.

## 2. Ambiente de execução

| Componente | Identificação |
|---|---|
| Sistema de origem | Kali Linux |
| IP de origem | 192.168.56.102 |
| Sistema de destino | Metasploitable 2 |
| IP de destino | 192.168.56.101 |
| Serviço analisado | FTP |
| Porta | TCP/21 |
| Servidor FTP | vsftpd 2.3.4 |
| Ferramenta | Medusa v2.3 |

## 3. Escopo e autorização

Todos os procedimentos foram realizados exclusivamente nas máquinas virtuais próprias, destinadas ao estudo de segurança da informação.

O escopo compreendeu a identificação do serviço, análise do mecanismo de autenticação, utilização de listas controladas de credenciais e validação dos resultados.

Nenhum sistema externo foi incluído nos testes.

## 4. Reconhecimento da ferramenta

Antes da execução, realizei uma análise das funcionalidades disponíveis no Medusa, identificando sua versão e os principais parâmetros de utilização.

**Versão identificada:** Medusa v2.3.

**Módulo selecionado:** FTP (`ftp.mod`, versão 2.1).

Parâmetros estudados:

| Parâmetro | Finalidade |
|---|---|
| `-h` | Define o endereço do host de destino |
| `-u` | Define um usuário específico |
| `-p` | Define uma senha específica |
| `-U` | Informa um arquivo de usuários |
| `-P` | Informa um arquivo de senhas |
| `-M` | Seleciona o módulo de autenticação |
| `-n` | Define uma porta específica |
| `-s` | Habilita conexão SSL, conforme suporte do módulo |
| `-v` | Controla o nível de detalhamento da saída |

## 5. Análise do módulo FTP

Durante a consulta às informações do módulo, identifiquei suporte aos seguintes modos:

- **NORMAL:** conexão FTP convencional.
- **EXPLICIT:** negociação de TLS explícito.
- **IMPLICIT:** conexão FTPS com TLS desde o início.

A identificação desses modos permitiu compreender que a ferramenta pode trabalhar com diferentes configurações de FTP e FTPS, dependendo da implementação do serviço de destino.

No ambiente analisado, o serviço identificado na porta 21 foi o vsftpd 2.3.4.

## 6. Verificação manual do serviço

Para confirmar a disponibilidade do serviço, utilizei o cliente FTP do Kali Linux:

```bash
ftp 192.168.56.101
```

**Resposta observada:**

```text
Connected to 192.168.56.101.
220 (vsFTPd 2.3.4)
```

O servidor respondeu à conexão e apresentou sua identificação, solicitando posteriormente um nome de usuário.

**Análise:** a comunicação com o serviço FTP foi estabelecida corretamente, confirmando sua disponibilidade no endereço analisado.

## 7. Teste de autenticação com Medusa

Após a identificação do serviço, preparei listas controladas de usuários e senhas para avaliar o mecanismo de autenticação.

**Comando executado:**

```bash
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
```

**Resultado obtido:**

```text
ACCOUNT FOUND: [ftp] Host: 192.168.56.101
User: msfadmin Password: msfadmin [SUCCESS]
```

**Credencial identificada:**

| Campo | Resultado |
|---|---|
| Serviço | FTP |
| Usuário | msfadmin |
| Senha | msfadmin |
| Resultado | SUCCESS |

## 8. Validação manual do resultado

Para verificar a consistência do resultado apresentado pelo Medusa, realizei uma nova conexão utilizando o cliente FTP.

**Comando:**

```bash
ftp 192.168.56.101
```

**Credenciais utilizadas:**

- Usuário: `msfadmin`
- Senha: `msfadmin`

**Resposta do servidor:**

```text
331 Please specify the password.
230 Login successful.
```

A resposta `230 Login successful` confirmou que a autenticação foi aceita pelo servidor.

Essa validação manual confirmou o resultado obtido anteriormente com o Medusa.

## 9. Análise técnica

O teste demonstrou que o serviço FTP aceitava uma credencial padrão conhecida do ambiente Metasploitable 2.

A utilização de credenciais previsíveis representa um risco de segurança, principalmente quando serviços de autenticação permanecem acessíveis pela rede sem controles adequados.

Outro ponto relevante é que o FTP convencional não oferece criptografia para proteger as credenciais durante a transmissão. Isso pode permitir sua exposição em uma rede onde o tráfego possa ser interceptado.

## 10. Recomendações de segurança

Com base nos resultados, foram identificadas as seguintes medidas de proteção:

- Substituir imediatamente as credenciais padrão.
- Implementar senhas fortes e exclusivas.
- Desabilitar autenticação anônima quando não necessária.
- Restringir o acesso ao serviço por regras de firewall.
- Implementar controles contra tentativas repetidas de autenticação.
- Monitorar e registrar tentativas de acesso.
- Priorizar SFTP, baseado em SSH, ou FTPS, baseado em FTP com TLS, conforme os requisitos da infraestrutura.

## 11. Conclusão

A atividade permitiu avaliar o funcionamento da autenticação FTP utilizando o Medusa e compreender a importância da validação manual dos resultados apresentados por ferramentas automatizadas.

Foi confirmada a utilização de credenciais padrão no serviço analisado, evidenciando uma fragilidade de configuração no ambiente de laboratório.

Além da execução técnica, o exercício contribuiu para desenvolver conhecimentos relacionados à enumeração de serviços, autenticação, análise de respostas do servidor e identificação de medidas de segurança.

**Resultado da atividade:** autenticação FTP validada com sucesso em ambiente controlado.