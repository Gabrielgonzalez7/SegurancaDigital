# Atividade de Segurança — 14/10

**Integrantes:** Andrisa e Gabriel Gonzalez

## **RODADA 1**

| Porta | Serviço e versão | Avaliação |
|---|---|---|
| 21/tcp | FTP — vsftpd 2.3.4 | Suspeito e vulnerável |
| 22/tcp | SSH — OpenSSH 9.6 | Verificar vulnerabilidades conhecidas |
| 80/443 | HTTP/HTTPS — Apache 2.4.49 | Versão antiga |
| 3306/tcp | MySQL 5.7.33 | Versão antiga |
| 8080/tcp | Apache Tomcat/Coyote | Depende da versão |

a) Do bloco autorizado pelo cliente

b) **Dentro do escopo autorizado:**

| Domínio | Endereço IP |
|---|---|
| `www.exemplo.com` | `198.51.100.10` |
| `loja.exemplo.com` | `198.51.100.10` |
| `vpn.exemplo.com` | `198.51.100.12` |

**Fora do escopo:**

| Domínio | Endereço IP | Motivo |
|---|---|---|
| `dev.exemplo.com` | `203.0.113.50` | Fora do bloco autorizado |

# **RODADA 2**

| Serviço | CVE | Vulnerabilidade | Confiança |
|---|---|---|---|
| vsftpd 2.3.4 | CVE-2011-2523 | Execução remota de código (RCE) | Alta |
| Apache 2.4.49 | CVE-2021-42013 | Path traversal e possível RCE | Média |
| Tomcat | CVE-2026-87022 | WebSocket Smuggling | Baixa |
| MySQL 5.7.33 | CVE-2021-2146 | RCE | Baixa |
| OpenSSH 9.6 | CVE-2023-48795 | Ataque Terrapin | Média |

O Tomcat precisa de atenção porque sua versão exata ainda não foi identificada.


## **RODADA 3**

| Ordem | Serviço | Justificativa |
|---|---|---|
| 1º | Apache 2.4.49 | Possibilidade de acessar arquivos e enviar arquivos maliciosos |
| 2º | vsftpd 2.3.4 | Possibilidade de executar comandos remotamente |
| 3º | Tomcat | Possível vazamento de dados |
| 4º | OpenSSH 9.6 | Possibilidade de acesso indevido ao servidor |
| 5º | MySQL 5.7.33 | Possibilidade de acessar ou alterar dados do banco |

A priorização considera os impactos que as falhas podem causar no sistema.

## **RODADA 4**

| Serviço | Tipo | Possível forma de exploração | O que confirmar |
|---|---|---|---|
| vsftpd 2.3.4 | Remoto | Backdoor conhecido no serviço | Confirmar a versão e se a porta 21 está acessível |
| Apache 2.4.49 | Web/remoto | Exploit de path traversal/RCE | Verificar se o `mod_cgi` está ativo |
| Tomcat | Web | Uso de senha padrão no painel administrativo | Confirmar a versão e as configurações de login |
| MySQL 5.7.33 | Remoto, dependendo da autenticação | Uso de credenciais fracas ou expostas | Verificar a segurança das credenciais |

a) O vsftpd 2.3.4, pois essa versão possui um backdoor conhecido que pode facilitar a invasão.

b) Sim, o Tomcat pode exigir duas etapas: primeiro, conseguir acesso ao painel administrativo; depois, enviar um arquivo malicioso para executar comandos no servidor.

## **RODADA 5**

| Pergunta | Cálculo | Resposta |
|---|---|---|
| Bits de host | 32 − 27 | 5 bits |
| Total de endereços | 2⁵ | 32 |
| Hosts utilizáveis | 32 − 2 | 30 |
| Endereço de rede | Primeiro endereço | `198.51.100.0` |
| Broadcast | Último endereço | `198.51.100.31` |
| Faixa de hosts | Entre rede e broadcast | `198.51.100.1` até `198.51.100.30` |

O bloco `/27` possui 32 endereços, dos quais 30 podem ser utilizados por hosts. O endereço `.0` representa a rede e o `.31` é o broadcast.

O escopo autorizado para os testes é o bloco `198.51.100.0/27`, respeitando os limites definidos pelo contratante.

| Prefixo | Bits de host | Total de endereços | Hosts utilizáveis |
|---|---:|---:|---:|
| /28 | 4 | 16 | 14 |
| /26 | 6 | 64 | 62 |

## **RODADA 6**

a) Ordem de ataque e explorador atribuído

| # | Serviço / falha | Explorador atribuído | Defesa recomendada |
| --- | --- | --- | --- |
| 1 | Apache 2.4.49 (CVE-2021-42013) | Exploit de path traversal / RCE | Atualizar o Apache para uma versão corrigida e segura |
| 2 | vsftpd 2.3.4 (CVE-2011-2523) | Backdoor de execução remota de código (RCE) | Atualizar o vsftpd ou substituir por um servidor FTP seguro |
| 3 | Tomcat (CVE-2026-87022) | Exploit de WebSocket Smuggling / Painel administrativo | Atualizar o Tomcat e proteger o painel com autenticação forte e restrição de IP |
| 4 | OpenSSH 9.6 (CVE-2023-48795) | Ataque Terrapin | Atualizar o OpenSSH para mitigar falhas no protocolo |

b) A principal prioridade de defesa é a **atualização imediata dos softwares com vulnerabilidades críticas de execução remota de código (RCE)**, especialmente o **Apache 2.4.49** e o **vsftpd 2.3.4**, que apresentam os maiores riscos de comprometimento total do servidor. Adicionalmente, recomenda-se implementar uma política rigorosa de gerenciamento de patches, restringir o acesso a painéis administrativos e monitorar os serviços expostos no escopo autorizado.
