# Como a Internet Funciona: TCP/IP, DNS++
## 📌 Introdução

A Internet é muito mais do que apenas "usar o Google" ou "acessar o TikTok". Ela é uma infraestrutura global baseada em protocolos, camadas de rede, servidores e cabos físicos que conectam continentes e levam informações de ponta a ponta em milissegundos. Entender essa base é essencial para quem quer se aprofundar em hacking, segurança ou desenvolvimento.

---

## 1. Modelos de Rede: OSI e TCP/IP

### 🔧 Modelo OSI (Teórico)

- **Camada 7**: Aplicação (HTTP, SMTP)
- **Camada 6**: Apresentação (Criptografia, Codificação)
- **Camada 5**: Sessão (Gerência de conexão)
- **Camada 4**: Transporte (TCP, UDP)
- **Camada 3**: Rede (IP)
- **Camada 2**: Enlace (Ethernet, Wi-Fi)
- **Camada 1**: Física (Cabos, rádio, fibra)

### ⚙️ Modelo TCP/IP (Prático)

- **Aplicação** (HTTP, DNS, SMTP)
- **Transporte** (TCP, UDP)
- **Internet** (IP)
- **Acesso à rede** (Ethernet, Wi-Fi, cabo físico)

> 📌 TCP/IP é o modelo real usado na Internet hoje, o modelo OSI ainda é estudado mas ele se distingue muito do mundo real, onde quase tudo é gerenciado pela camada de aplicação, gerando a maioria das vulnerabilidades de segurança moderna que temos hoje.

---

## 2. O Protocolo TCP/IP

### 🏠 IP (Internet Protocol)

- Responsável por endereçar e encaminhar pacotes na rede.
- Exemplo: `192.168.0.1`, `8.8.8.8`, `2606:4700:4700::1001`

### 🧱 TCP (Transmission Control Protocol)

- Cria conexões confiáveis (3-way handshake).
- Garante que os dados cheguem completos e ordenados.

### ⚡UDP (User Datagram Protocol)

- Conexão não confiável, no que diz respeito a integridade. No entanto, mais rápida (usado em jogos, streaming, VPNs e etc).

### 🚪 Portas

- Identificam serviços em um mesmo IP.
- Ex: `80 = HTTP`, `443 = HTTPS`, `22 = SSH`, `53 = DNS`

---

## 3. DNS — Domain Name System

### 🤔 Por que usar nomes?

 IPs são difíceis de memorizar. `www.google.com` é mais fácil que `172.217.29.67` ou `2800:3f0:4001:80a::2004`.

### 🔍 Como funciona a resolução DNS?

1. Você digita `www.google.com`
2. Seu sistema consulta o **DNS Resolver** (geralmente da operadora ou algum DNS público como Cloudflare e Google)
3. O Resolver pergunta aos **Root Servers**
4. Eles indicam o **servidor TLD** (`.com`, `.org`, etc.)
5. O TLD informa o **servidor autoritativo**
6. O autoritativo responde com o IP

### 🧪 Demonstrando na prática

```
# Consultar os root servers
dig . NS ou Resolve-DnsName . -Type NS

# Consultar o TLD do domínio .br
dig br. NS ou Resolve-DnsName br. -Type NS

# Consultar os servidores autoritativos do domínio exemplo.br
dig exemplo.br NS ou Resolve-DnsName exemplo.br -Type NS

# Consultar o IP final de www.exemplo.br
dig www.exemplo.br A ou Resolve-DnsName www.exemplo.br -Type A
```

> No Brasil, o domínio ".br" é gerenciado pelo **Registro.br**. Eles mantêm os servidores autoritativos para domínios nacionais, como `gov.br`, `com.br`, `org.br`, etc.
### 📡 Root Servers

- São 13 servidores raiz (letras A até M)
- Espalhados em centenas de locais globalmente via anycast
- Ponto inicial de toda resolução de domínio

---

## 4. ASN — Autonomous System Numbers

### 👨‍💼 O que é um ASN?

- Um sistema autônomo é um grupo de IPs controlado por uma entidade única (empresa, operadora, governo).
- Cada ASN possui uma identificação única no mundo.

### 🛣️ BGP (Border Gateway Protocol)

- É o protocolo que permite ASNs trocarem rotas.
- A "Internet" é um emaranhado de ASNs se conectando.

### 🔎 Consultando ASN

- Use: `https://bgp.he.net/`
- Ou: `curl ipinfo.io` para ver ASN do seu IP

---

## 5. ISPs — Provedores de Internet

### 📡 O papel das operadoras

- Conectam você à internet global
- Resolvem DNS por padrão
- Encaminham todo o tráfego

### ⚠️ Riscos

- **DNS hijacking**: interceptar e redirecionar para IPs falsos
- **Vigilância** e **censura** por governos ou empresas
- **SSL stripping** e injeção de conteúdo

> 📌 ISPs são alvos muito mais fáceis do que Root Servers e têm grande poder na infraestrutura, podendo gerar um estrago terrível.

---

## 6. Cabos Submarinos e Backbone Global

### 🌊 Cabos submarinos

- Conectam continentes por baixo do oceano
- São responsáveis por mais de 95% do tráfego global
- Exemplo de mapa: [https://www.submarinecablemap.com](https://www.submarinecablemap.com/)

### 🏛️ Quem cuida dos cabos?

- Empresas como Google, Facebook, operadoras internacionais, governos
- Pontos estratégicos: Brasil-Portugal, EUA-Europa, Japão-Oceania

### 💣 Riscos reais

- Cortes intencionais ou acidentais causam lentidão ou blackout regional
- Espionagem e interceptação física já foram [documentadas](https://www.theguardian.com/world/2024/nov/22/wire-cutters-how-the-worlds-vital-undersea-data-cables-are-being-targeted)

---

## 7. Segurança e Controle

### 🔐 Root Servers

- Altamente redundantes e protegidos
- Quase impossíveis de "derrubar", mas alvos de DDoS

### ⚠️ ISPs/ASNs

- Facilidade de acesso, exploração ou cooptação
- Podem manipular DNS, certificados, tráfego

### 🛡️ Medidas de defesa

- Usar **DNS over HTTPS (DoH)** ou **DNS over TLS (DoT)**
- VPNs para ocultar tráfego da operadora
- RPKI para validar rotas BGP
- Criptografia end-to-end sempre que possível

---

## 📈 Roteiro de um pacote na internet

1. Você digita `google.com.br`
2. Seu navegador consulta o DNS
3. O DNS retorna o IP do servidor
4. Seu computador inicia uma conexão TCP com aquele IP (via ISP)
5. O pacote viaja por roteadores, cabos e ASN intermediários
6. O servidor responde e o conteúdo aparece na tela

---

## 🎓 Conclusão

- A Internet não é centralizada — é distribuída, mas com pontos críticos.
- Root Servers são fundamentais, mas as **operadoras e ASNs** são os principais **elos vulneráveis**.
- Conhecer essa base é essencial para quem deseja atuar em segurança ofensiva, engenharia reversa de rede ou desenvolvimento de sistemas resilientes.

---

## 🔗 Referências úteis

- [https://bgp.he.net/](https://bgp.he.net/)
- [https://ipinfo.io/](https://ipinfo.io/)
- [https://submarinecablemap.com/](https://submarinecablemap.com/)
- [https://www.cloudflare.com/learning/dns/](https://www.cloudflare.com/learning/dns/)
- [https://dnssec-analyzer.verisignlabs.com/](https://dnssec-analyzer.verisignlabs.com/)