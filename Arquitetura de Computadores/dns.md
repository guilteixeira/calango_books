# 🌐 Aprendendo DNS de verdade (ou não)

## 📌 Introdução

O DNS é o \"catálogo telefônico da Internet\".  
Permite que usemos nomes legíveis em vez de endereços IP.  
É distribuído, hierárquico e fundamental para quase tudo online.

---

## 1️⃣ Arquitetura e Hierarquia

### 🌳 Como o DNS é organizado

- **Root Servers**
	- 13 letras (A-M)
	- Anycast global
	- Ponto inicial de todas as consultas

- **TLD Servers**
	- Top-Level Domains como **.com, .org, .br**
	- Sabem quem gerencia cada domínio abaixo deles

- **Servidores Autoritativos**
	- Contêm a zona oficial do domínio
	- Resposta final para registros

- **Resolvers**
	- Normalmente do ISP ou públicos (**1.1.1.1**, **8.8.8.8**)
	- Consultam e cacheiam resultados

> 📌 Resolver faz o trabalho pesado

---

## 2️⃣ Fluxo de Resolução DNS

### 🔎 Passo a passo típico

1. Usuário digita `www.exemplo.com`
2. Sistema consulta o **Resolver local**
3. Resolver checa cache
4. Sem cache? Pergunta ao **Root Server**
5. Root responde com servidores **TLD (.com, .br)**
6. Resolver pergunta ao TLD
7. TLD responde com **servidor autoritativo**
8. Resolver pergunta ao autoritativo
9. Autoritativo responde com o **IP final**

---

### 🧭 Exemplo real

- `www.google.com.br`:
- Resolver -> Root
- Root -> TLD .br
- TLD .br -> registro.br autoritativo
- Autoritativo -> IP do servidor

### 🧪 Exemplos práticos

#### Com dig (Linux/macOS/WSL)

```bash
dig google.com.br A
dig google.com.br MX
dig google.com.br NS
dig google.com.br SOA
dig google.com.br TXT
```

#### Com Resolve-DnsName (Windows PowerShell)

```
Resolve-DnsName google.com.br -Type A
Resolve-DnsName google.com.br -Type MX
Resolve-DnsName google.com.br -Type NS
Resolve-DnsName google.com.br -Type SOA
Resolve-DnsName google.com.br -Type TXT
```


---

## 3️⃣ Tipos de Registros DNS

### 📑 Principais tipos
| Tipo  | Função                                                                |
| ----- | --------------------------------------------------------------------- |
| A     | Nome -> IPv4                                                          |
| AAAA  | Nome -> IPv6                                                          |
| NS    | Servidores autoritativos do domínio (Name Server)                     |
| SOA   | Info administrativa e autoridade da zona                              |
| MX    | Servidores de e-mail                                                  |
| CNAME | Apelido para outro nome (exemplo: www.google.com = google.com)        |
| TXT   | Dados livres (SPF, DKIM, outras assinaturas ou confirmações de posse) |
| SRV   | Serviços específicos (porta, protocolo)                               |
| PTR   | Resolução reversa (IP -> Nome)                                        |

---

## 4️⃣ Consultas DNS avançadas passo a passo

#### 🔎 Simulando a cadeia de resolução em passos

##### 1. Root Servers

```
dig . NS
Resolve-DnsName . -Type NS
```
##### 2. TLD .br

```
dig br. NS
Resolve-DnsName br. -Type NS
```
##### 3. Autoritativo para registro.br

```
dig registro.br NS
Resolve-DnsName google.com.br -Type NS
```
##### 4. IP final do site

```
dig www.registro.br A
Resolve-DnsName google.com.br -Type A
```

## 5️⃣ O DNS é o Cagueta da Internet

> 📞 "Não sei só o nome não, irmão... tenho o endereço, o CEP, o correio, o gerente, e até o contrato!"

- **A**: endereço IPv4
- **AAAA**: IPv6
- **MX**: quem recebe teu correio (e-mail)
- **NS**: quem gerencia teus dados
- **SOA**: quem é o dono oficial da zona
- **CNAME**: apelido pro vizinho
- **TXT**: fofocas (SPF, DKIM, verificações)
- **SRV**: serviços específicos (porta, protocolo)
- **PTR**: reverso, te deda de volta (IP -> Nome)

>✅ Mostra que o DNS não guarda só um caminho — ele entrega tudo o que precisa pra te achar, contatar e até autenticar.

## 6️⃣ Vetores de Ataque em DNS

### ⚠️ Possíveis ataques

- **DNS Cache Poisoning**
	- DNS Resolver guarda resposta maliciosa
	- Usuários redirecionados para IP falso
- **DNS Hijacking**
    - Alteração em provedores de hospedagem ou registrars
    - Mudança de NS ou MX
- **Sequestro de MX**
    - Redirecionar todos os e-mails para servidor controlado
    - Impacto catastrófico para empresas
- **Ataques MITM em ISPs**
    - DNS hijack em resolvers default
    - Injeção de publicidade ou censura
## 📌 Observação importante

> Não é necessário comprometer um root server ou TLD inteiro. Basta comprometer um **provedor de hospedagem ou registrar** para sequestrar o DNS **de um único site** — e isso já pode causar um caos total para serviços críticos.

## 7️⃣ Segurança e Boas Práticas

### 🛡️ Como se defender

- **DNSSEC**
    - Assina registros
    - Evita alterações não autorizadas
- **DNS over HTTPS (DoH) / DNS over TLS (DoT)**
    - Impede interceptação no caminho
- **Monitoramento**
    - Verificar mudanças em registros NS e MX
    - Alertas para alterações suspeitas
- **Bons hábitos**
    - Proteger contas de painel DNS com MFA
    - Usar registrars confiáveis
    - Verificar quem tem acesso administrativo
## 8️⃣ Ferramentas Úteis

- **CLI**
    - dig / drill (Linux/macOS/WSL)
    - Resolve-DnsName (PowerShell)
    - nslookup (Windows/macOS)
- **Web-based**
    - [Google Toolbox Dig](https://toolbox.googleapps.com/apps/dig/)
    - [DNSLookup.online](https://dnslookup.online/)
- **WHOIS**
    - Para checar ownership
## 📈 Resumão de uma consulta

> Você -> Resolver -> Root -> TLD -> Autoritativo -> IP final -> conexão TCP -> serviço desejado
## 🎓 Conclusão

- DNS é parte crítica da Internet
- Entender bem DNS é essencial para quem trabalha com segurança
- Entender registros, fluxo e ataques te dá vantagem para defesa (ou ofensiva)
