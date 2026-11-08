# Medusa Brute Force Security Lab

Laboratório prático de auditoria de segurança com foco em enumeração de serviços, testes controlados de autenticação e análise de vulnerabilidades utilizando Kali Linux, Medusa e Metasploitable 2.

## Sobre o projeto

Este projeto documenta a construção e execução de um laboratório controlado de segurança cibernética, explorando técnicas de reconhecimento, enumeração e testes de autenticação em serviços de rede e aplicações web.

O objetivo é compreender como determinadas falhas de configuração e o uso de credenciais fracas podem comprometer a segurança de sistemas, além de identificar medidas de prevenção e mitigação.

## Objetivos

- Estudar técnicas de reconhecimento e enumeração.
- Compreender ataques de força bruta e Password Spraying.
- Realizar testes controlados nos serviços FTP e SMB.
- Analisar autenticação em aplicações web utilizando DVWA.
- Identificar exposição de usuários, compartilhamentos e credenciais.
- Documentar evidências e resultados obtidos.
- Apresentar medidas de segurança e mitigação.

## Ambiente de laboratório

| Componente | Utilização |
|---|---|
| Kali Linux | Sistema de testes |
| Metasploitable 2 | Máquina de laboratório vulnerável |
| DVWA | Aplicação web para testes |
| VirtualBox | Ambiente de virtualização |
| Medusa | Testes de autenticação |
| Nmap | Reconhecimento de serviços |
| Enum4linux | Enumeração SMB |
| smbclient | Validação de acesso SMB |

## Etapas desenvolvidas

### 1. Reconhecimento e enumeração
- Identificação de serviços e portas abertas.
- Enumeração de usuários e compartilhamentos SMB.
- Análise de informações expostas pelo serviço.

### 2. Testes de autenticação
- Testes controlados de força bruta em FTP.
- Enumeração SMB e Password Spraying.
- Validação manual de credenciais.
- Análise de autenticação web utilizando DVWA.

### 3. Mitigação
- Identificação dos riscos encontrados.
- Recomendações para proteção de serviços SMB e FTP.
- Análise de políticas de senhas e permissões.
- Recomendações de monitoramento e controle de acesso.

## Estrutura do projeto

```text
medusa-brute-force-security-lab/
│
├── reconnaissance/
│   └── reconhecimento.md
│
├── attacks/
│   ├── ftp-medusa.md
│   └── dvwa-analise.md
│
├── mitigation/
│   └── README.md
│
├── wordlists/
├── images/
│
└── README.md
```

## Resultados

Durante os testes foram identificadas credenciais válidas em ambiente de laboratório, exposição de informações de usuários SMB e permissões de acesso a compartilhamentos.

Os resultados foram registrados e acompanhados de recomendações técnicas de segurança.

## Escopo e responsabilidade

Todos os testes foram realizados em ambiente virtual controlado, utilizando máquinas destinadas à prática de segurança cibernética.

Este projeto possui finalidade exclusivamente educacional e de demonstração de conhecimentos em auditoria de segurança.

## Status

**Projeto concluído — documentação técnica das etapas desenvolvidas.**


## ASS: Adriano costa ##