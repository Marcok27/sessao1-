# sessao1-

---

#  Auditoria de Segurança e Reconhecimento de Ativos em Sistemas Linux

Este repositório documenta a execução prática de uma rotina de auditoria e reconhecimento de infraestrutura em ambientes Linux (distribuição Ubuntu). O objetivo principal foi validar a integridade do sistema operacional, mapear permissões de arquivos críticos de autenticação, inspecionar logs globais e consolidar essas descobertas em um relatório automatizado.

---

##  1. Reconhecimento e Coleta de Metadados do Sistema

Antes de qualquer procedimento de segurança, é vital entender a superfície e a versão exata do kernel em execução, bem como o nível de privilégio do operador atual.

* **Comandos Utilizados:** `uname -r`, `uname -a`, `cat /etc/os-release`, `whoami`, `id`
* **O que foi feito (Ref: `sessao1_passo1_sistema.png.png`):**
* Identificação da versão exata do Kernel Linux (`6.8.0-124-generic`).
* Extração dos metadados da distribuição via `/etc/os-release`, confirmando o uso do **Ubuntu 24.04.4 LTS (Noble Numbat)**.
* Validação de privilégios com `whoami` e `id`, garantindo a execução sob o escopo do usuário administrador máximo (`root` com `uid=0`).



---

##  2. Inspeção de Arquivos Críticos de Configuração e Permissões (`/etc`)

A pasta `/etc` centraliza as configurações do sistema. Esta etapa focou em mapear os arquivos que gerenciam credenciais, chaves e o agendamento de tarefas do sistema.

* **Comandos Utilizados:** `ls -la`, `grep -E`, `stat`
* **O que foi feito (Ref: `sessao1_passo2_etc.png.png`):**
* Contagem total de entradas no diretório raiz de configuração (215 arquivos manipulados).
* Filtragem refinada usando expressões regulares estendidas (`grep -E`) para isolar ativos sensíveis como `passwd`, `shadow`, `sudoers`, `ssh`, `hosts` e diretórios `cron.*`.
* Análise detalhada de metadados de segurança via `stat /etc/shadow` para validar os timestamps de modificação, alteração e permissões nativas de leitura restrita do arquivo de hashes de senhas.



---

##  3. Mapeamento de Diretórios de Usuários (`/home`)

Para fins de controle de acessos, inspecionou-se a árvore do sistema responsável por armazenar os dados de usuários comuns.

* **Comandos Utilizados:** `ls -la /home/`
* **O que foi feito (Ref: `sessao1_passo2_home.png.png`):**
* Listagem completa de permissões do diretório `/home/`.
* Identificação do diretório de usuário isolado `ubuntu` (`drwxr-x---`), garantindo que apenas o dono e o grupo tenham privilégios de acesso às pastas pessoais.



---

##  4. Auditoria de Logs Globais do Sistema (`/var/log`)

Inspeção do diretório responsável pela persistência de logs de auditoria, autenticação, inicialização do sistema e pacotes instalados.

* **Comandos Utilizados:** `ls -lh /var/log/`
* **O que foi feito (Ref: `sessao1_passo2_varlog.png.png`):**
* Listagem detalhada em formato legível por humanos (`-lh`) do repositório de logs.
* Mapeamento de arquivos vitais de segurança, como `auth.log` (tentativas de login), `syslog` (eventos gerais do sistema) e logs de pacotes como `dpkg.log`.



---

## 📊 5. Geração Automatizada do Relatório de Auditoria Final

Consolidação de todos os dados coletados nos passos anteriores em um único script de output contínuo para documentação formal.

* **Comandos Utilizados:** `mkdir -p`, `echo`, `>`, `>>`, `cat`
* **O que foi feito (Ref: `auditoria.txt.jpg`):**
* Criação do diretório de relatórios `~/relatorio/sessao1`.
* Utilização de operadores de append (`>>`) para injetar o carimbo de data dinâmica `$(date)`, os dados estruturais do hardware/kernel obtidos por `uname`, os logs de permissões obtidos por `ls | grep` e o mapeamento final de `/var/log`.
* Leitura final e validação com `cat auditoria.txt`, gerando um artefato pronto para auditorias de conformidade (Compliance).



---

## Tecnologias e Ferramentas Aplicadas

| Categoria | Ferramentas de Linha de Comando |
| --- | --- |
| **Coleta de Informações** | `uname`, `whoami`, `id`, `cat /etc/os-release` |
| **Auditoria de Permissões** | `ls -la`, `stat` |
| **Filtragem Avançada** | `grep -E` (Regex para múltiplos padrões) |
| **Escrita de Relatórios** | Operadores de redirecionamento (`>`, `>>`) e `echo` |
