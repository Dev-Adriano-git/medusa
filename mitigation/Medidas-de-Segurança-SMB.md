# Mitigation — Medidas de Segurança SMB

**Projeto:** Laboratório de Auditoria de Segurança  
**Alvo:** Metasploitable 2  
**Serviço analisado:** SMB/Samba

## 1. Vulnerabilidades identificadas

- Permissão de sessões SMB anônimas.
- Enumeração de usuários sem autenticação.
- Compartilhamento `tmp` acessível anonimamente.
- Credenciais previsíveis (`msfadmin:msfadmin`).
- Acesso ao compartilhamento administrativo `ADMIN$`.
- Utilização do protocolo SMBv1.

## 2. Medidas de mitigação

| Risco identificado | Medida recomendada |
|---|---|
| Sessões anônimas | Desabilitar acesso SMB sem autenticação. |
| Enumeração de usuários | Restringir consultas e informações expostas pelo serviço. |
| Compartilhamentos expostos | Revisar permissões de leitura e escrita. |
| Senhas previsíveis | Utilizar senhas fortes, únicas e complexas. |
| Acesso administrativo | Restringir o acesso ao `ADMIN$` a administradores autorizados. |
| SMBv1 | Desabilitar SMBv1 e utilizar versões modernas do protocolo. |
| Tentativas repetidas de autenticação | Implementar monitoramento, limitação de tentativas e políticas de bloqueio. |

## 3. Recomendações adicionais

- Manter Samba e sistema operacional atualizados.
- Aplicar segmentação de rede e regras de firewall.
- Monitorar logs de autenticação SMB.
- Aplicar o princípio do menor privilégio.
- Realizar auditorias periódicas nas permissões dos compartilhamentos.

## 4. Conclusão técnica

A auditoria demonstrou riscos relacionados à exposição de informações SMB, permissões inadequadas e utilização de credenciais previsíveis.

A aplicação das medidas recomendadas contribui para reduzir a superfície de ataque, dificultar acessos não autorizados e fortalecer a segurança dos serviços de compartilhamento de arquivos.