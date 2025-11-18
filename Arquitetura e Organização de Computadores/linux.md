# Introdução a Sistemas Operacionais (foco em Linux)

  
##  **O que é um Sistema Operacional**

Um **sistema operacional (SO ou OS)** é o intermediário entre o usuário e o hardware do computador.  Ele abstrai a complexidade física da máquina e oferece uma interface organizada para que programas possam rodar sem se preocupar com detalhes como “como escrever um bit no disco” ou “como enviar sinais elétricos para a placa de vídeo”.

**As funções centrais de um SO incluem:**

- **Gerenciar recursos**: memória, CPU, dispositivos de entrada e saída.
- **Prover abstrações**: arquivos, processos, **sockets**, etc.
- **Garantir segurança e isolamento**: impedir que um programa invada o espaço do outro.
- **Facilitar interação**: oferecendo camadas (**linha de comando**, **interfaces gráficas**, **APIs**).

Sem o sistema operacional, seria como ter que construir o próprio carro para dirigir, ou seja,  precisaríamos entender profundamente de cada peça, e também sermos ótimos mecânicos para fazê-lo funcionar e dirigi-lo.

---

## **O que é o Linux**

Muita gente fala “Linux” se referindo ao sistema operacional completo (como Ubuntu, Fedora, Arch).  Tecnicamente, **Linux é o nome do kernel**, o núcleo que faz a comunicação entre hardware e software.

O “sistema Linux” que usamos no dia a dia combina o kernel com utilitários, bibliotecas e ferramentas vindas principalmente do projeto **GNU**, caso queira saber mais sobre o projeto, vou omitir detalhes neste artigo mas você pode conferir [aqui](https://www.gnu.org/gnu/gnu-history.pt-br.html).
## **Userland vs Kernel Land**

**No Linux, o mundo se divide em dois:**
#### **Userland**

É o espaço onde vivem os programas que nós, usuários, usamos no dia a dia. Tudo aqui **conversa com o kernel via syscalls, mas não tem acesso direto ao hardware, seus principais elementos são:**.

- **Interface com o usuário**:
  - **Shell**: interface de linha de comando (bash, zsh).
  - **Interface gráfica**: sistemas de janelas (X11, Wayland), gerenciadores de desktop (GNOME, KDE).

- **Utilitários**: pequenos programas que realizam funções básicas — calculadora, editor de texto, controladores de rede, ferramentas de sistema.
- **Bibliotecas e APIs**: camadas que simplificam a vida do programador. Exemplo: a biblioteca C (`glibc`) implementa funções como `printf()`, que no fundo se conecta a **syscalls**.
- **Processos e programas**: qualquer software que roda no espaço do usuário — navegadores, servidores, editores de código.
- **Daemons e serviços**: programas que ficam rodando em segundo plano prestando serviços ao sistema (rede, agendamento, autenticação).

#### **KernelLand**

É a parte que roda com privilégios máximos, diretamente em contato com o hardware.  Aqui ficam os componentes que garantem que o sistema funcione de forma estável e segura.

- **Kernel**: o núcleo central, que gerencia memória, processos, dispositivos e oferece chamadas de sistema.
- **Drivers**: “tradutores” que permitem ao kernel falar com diferentes hardwares (placa de rede, disco, GPU).
- **Gerenciadores internos**:

  - **Scheduler**: decide qual processo roda na CPU em cada momento.
  - **Memory Manager**: organiza a memória, garantindo isolamento entre processos.
  - **I/O Manager**: cuida de entrada/saída, buffers e comunicação com dispositivos.

- **Sockets e IPC (Inter-Process Communication)**: mecanismos para que processos conversem entre si, seja via rede (sockets TCP/UDP) ou local (pipes, filas, memória compartilhada).

> Essa separação garante estabilidade: se um navegador ou outro software em userland travar, não derruba o kernel.

A comunicação entre esses dois mundos acontece via **syscalls** — **instruções** especiais que permitem ao programa pedir serviços ao kernel. Podemos visualizar as coisas assim:

```bash
+----------------------------+

|     Programas do Usuário   |
| (navegadores, editores,    |  ← User Land
|  utilitários, daemons)     |
+----------------------------+       ↑
|  Bibliotecas e APIs        |       |
|  (glibc, OpenSSL, etc.)    |       |
+----------------------------+       |
|  Syscalls (chamadas ao     |       |
|  kernel)                   |       |
+----------------------------+       ↓
|  Kernel (drivers, memória, |      
|  processos, I/O, rede)     | ← Kernel Land
|                            |
+----------------------------+
|  Hardware (CPU, RAM, Disco)|
+----------------------------+
```

---

## Kernel: o coração do sistema

  O **kernel** é o núcleo do sistema operacional. Ele roda no nível mais privilegiado do processador e controla diretamente os recursos da máquina.

**Tipos de kernel:**
   - **Monolítico**: todo o núcleo roda junto (Linux segue esse modelo, mas permite módulos carregáveis).
   - **Microkernel**: deixa apenas o essencial no núcleo, movendo serviços para o espaço de usuário.
   - **Híbrido**: mistura características (caso do Windows e do macOS).

**Funções principais do kernel Linux:**
- **Gerenciar processos**: criação, escalonamento, finalização.
- **Gerenciar memória**: alocação, proteção, troca (swap).    
- **Gerenciar dispositivos**: drivers, entrada e saída.    
- **Expor chamadas de sistema (syscalls)**: a interface oficial entre programas e o kernel.

---

## File System: tudo é um arquivo

Uma das ideias mais elegantes do Linux é: **“tudo é um arquivo”**.  
Isso significa que não apenas documentos e imagens são arquivos, mas também diretórios, dispositivos de hardware, sockets e pipes.

  **Diferença entre arquivo e diretório:**
- Um arquivo guarda dados (texto, imagem, binário etc.). Dispositivos e diretórios aparecem como arquivos especiais no Linux.
- Um diretório é apenas um arquivo especial que lista referências para outros arquivos.

O que diferencia uma foto de um arquivo de música, são a forma como organizamos os bytes e como eles são interpretados, para isso criamos o que chamamos de formato de arquivos, ou seja, são bytes codificados e representados de tal forma que podem gerar som ou imagem, geralmente eles são confundidos como extensões como `.jpg`, `.wav` ou até `.exe` mas extensões no Linux não são nada além de convenções, elas não alteram absolutamente nada no funcionamento real.

> **Nota:** Podemos ler o conteúdo de um arquivo no Linux, dando um comando `cat` para ler o conteúdo como `texto`, e usar um `pipe` | para que a saída do `cat` vá para outro comando, neste caso o comando `xxd`, para ler arquivos binários no terminal — a primeira vez é sempre surpreendente.

  

**Alguns exemplos:**
- `/home/guilherme/documento.txt` → arquivo comum.
- `/dev/sda` → arquivo especial que representa um disco.
- `/proc/cpuinfo` → arquivo virtual com informações da CPU.

Outros diretórios e arquivos importantes podemos conferir neste [artigo](https://www.geeksforgeeks.org/linux-unix/linux-directory-structure/). Alguns exemplos adicionais:

- `/dev/null` → arquivo especial que descarta tudo que é escrito nele.
- `/etc/passwd` → arquivo de configuração com informações de usuários.

---

## Processos e Daemons

Um **processo** é um programa em execução. Ele tem seu espaço de memória, estado e prioridade, todos controlados pelo kernel.

**Etapas importantes:**
- Criação: no Linux, geralmente por meio do `fork()` (cópia de um processo) e `exec()` (substituição pelo novo programa).
- Escalonamento: o kernel decide qual processo roda em cada momento, compartilhando a CPU.
- Estados: executando, aguardando, finalizado.

**Daemons** são processos especiais que rodam em segundo plano, prestando serviços.  

Exemplos:
- `sshd` → permite conexões remotas via SSH.
- `cron` → agenda tarefas.

Você pode ver os processos rodando com `ps` ou `top`, por exemplo.

> Sem daemons, grande parte da automação e dos serviços de rede simplesmente não funcionaria.

---

## Interrupções e Chamadas de Sistema

Os processadores modernos precisam lidar com eventos inesperados — e aí entram as **interrupções**.

- **Interrupções de hardware**: vêm de dispositivos (ex.: teclado envia sinal ao pressionar uma tecla).
- **Interrupções de software**: disparadas por programas ou pelo próprio kernel.

Quando isso acontece, o kernel “pausa” o que estava executando, trata o evento e depois retoma a atividade normal.

Já as **chamadas de sistema (syscalls)** são o jeito formal de pedir algo ao kernel.  

**Exemplos:**
- `open()` → abrir um arquivo.
- `read()` → ler dados.
- `write()` → escrever dados.
- `execve()` → executar um programa.

Essas syscalls formam a ponte entre userland e kernel land.