# Como a Internet Funciona: Descomplicando o modelo TCP/IP
## 📌 Introdução

A Internet é muito mais do que apenas "usar o Google" ou "acessar o TikTok". Ela é uma infraestrutura global baseada em protocolos, camadas de rede, servidores e cabos físicos que conectam continentes e levam informações de ponta a ponta em milissegundos. Entender essa base é essencial para quem quer se aprofundar em hacking, segurança ou desenvolvimento.

---

## 1️⃣ Modelos de Rede: OSI e TCP/IP

### 🔧 Modelo OSI (Teórico)

- **Camada 7**: Aplicação (HTTP, SMTP)
- **Camada 6**: Apresentação (Criptografia, Codificação)
- **Camada 5**: Sessão (Gerência de conexão)
- **Camada 4**: Transporte (TCP, UDP)
- **Camada 3**: Rede (IP)
- **Camada 2**: Enlace (Data Link) (Ethernet, Wi-Fi)
- **Camada 1**: Física (Cabos, rádio, fibra)

> *Didático, mas, definitivamente não reflete a realidade.*

### ⚙️ Modelo TCP/IP (Prático)

- **Aplicação** (HTTP, DNS, SMTP)
- **Transporte** (TCP, UDP)
- **Internet** (IP)
- **Acesso à rede** (Ethernet, Wi-Fi, cabo físico)

> 📌 TCP/IP é o modelo real usado na Internet hoje, o modelo OSI ainda é estudado mas ele se distingue muito do mundo real, onde quase tudo é gerenciado pela camada de aplicação, gerando a maioria das vulnerabilidades de segurança moderna que temos hoje.

> A maioria dos ataques modernos mira na **Camada de Aplicação.**

---
## 2️⃣ O Protocolo TCP/IP

### 🏠 IP (Internet Protocol)

- Responsável por endereçar e encaminhar pacotes na rede.
- Exemplo: `192.168.0.1`, `8.8.8.8`, `2606:4700:4700::1001`
### 🧱 TCP (Transmission Control Protocol)

- Cria conexões confiáveis com [3-way handshake](https://gitbook.ganeshicmc.com/redes/three-way-handshake) (SYN, SYN-ACK, ACK).
- Garante que os dados cheguem completos e ordenados.
### ⚡UDP (User Datagram Protocol)

- Conexão não confiável, no que diz respeito a integridade. No entanto, mais rápida (usado em jogos, streaming, VPNs e etc).

---
## 3️⃣ Portas comuns

  - **80: HTTP**
  - **443: HTTPS**
  - **22: SSH**
  - **53: DNS**
  - **25/587/465: SMTP**

---
## 4️⃣ ASN e BGP

### 👥 O que é ASN?

- Autonomous System Number
- Conjunto de IPs gerenciado por uma entidade única (ISP, empresa, governo)
### 🌐 BGP (Border Gateway Protocol)

- \"Mapa\" que conecta ASNs
- Internet = emaranhado de ASNs trocando rotas
### 🔎 Ferramentas

- [bgp.he.net](https://bgp.he.net)
- `curl ipinfo.io` para ver seu ASN

---
## 5️⃣ ISPs e Backbones

### 📡 O papel do ISP

- Conectar usuários à Internet
- Resolver DNS por padrão
- Encaminhar tráfego
### 🌊 Cabos Submarinos

- Mais de 95% do tráfego global
- Conectam continentes por baixo do oceano
- [submarinecablemap.com](https://www.submarinecablemap.com)

> ISPs se conectam a pontos de troca de tráfego, ASN e cabos submarinos.

### ⚠️ Riscos

- Controle governamental ou corporativo
- Vigilância e censura
- Manipulação de tráfego
- Espionagem e sabotagem física de cabos

> 🧭 Exemplo: espionagem em cabos submarinos documentada [The Guardian](https://www.theguardian.com/world/2024/nov/22/wire-cutters-how-the-worlds-vital-undersea-data-cables-are-being-targeted)

---
## 6️⃣ Segurança de Rede

### 🛠️ Vetores de ataque

- BGP hijacking (sequestro de rotas)
- Interceptação em ISPs
- Bloqueio e censura
### 🛡️ Boas práticas

- VPNs para proteger tráfego do ISP
- Criptografia fim-a-fim
- Monitoramento de rotas BGP (RPKI)
## 📈 Roteiro de um pacote

1. Você digita um endereço
2. Resolução DNS (DNS Resolver → Root → TLD → Autoritativo)
3. IP obtido
4. TCP/UDP abre conexão
5. Pacote atravessa ISP, ASN, cabos
6. Resposta retorna

---
## 🎓 Conclusão

- Internet = infraestrutura distribuída com pontos críticos
- ISPs e ASN são vulneráveis
- Entender redes é fundamental para segurança
---
## 🔗 Referências

- [https://bgp.he.net/](https://bgp.he.net/)
- [https://ipinfo.io/](https://ipinfo.io/)
- [https://submarinecablemap.com/](https://submarinecablemap.com/)
- [https://www.cloudflare.com/learning/dns/](https://www.cloudflare.com/learning/dns/)
- [https://dnssec-analyzer.verisignlabs.com/](https://dnssec-analyzer.verisignlabs.com/)