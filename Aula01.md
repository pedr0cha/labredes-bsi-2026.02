# Relatório Técnico - Aula Prática 01: Introdução à Virtualização e Instalação do Ubuntu Server 26.04

## 1. Identificação
* **Nome Completo:** Pedro Henrique Cavalcante Rocha
* **Matricula:** 2023007730
* * **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Disciplina:** Laboratório de redes
* **Data:** 29/08/2026
* **Título da Prática:** Instalação e Configuração do Ubuntu Server 26.04 LTS no Oracle VM VirtualBox

---

## 2. Objetivo
Compreender os conceitos fundamentais de virtualização (gerenciamento por Hypervisor Tipo 2 e isolamento de recursos) através da criação, configuração e implantação de uma máquina virtual no Oracle VM VirtualBox. A prática abrangeu o provisionamento do sistema operacional de rede Ubuntu Server 26.04 LTS, a validação da conectividade de rede via interface virtual e a verificação do sistema de arquivos e repositórios.

---

## 3. Ambiente de Trabalho
* **Host (Máquina Física):** Windows 10/11
* **Estrutura de Diretórios no Host:** `C:\2026\BSI\VM\PedroRocha\`
* **Hypervisor:** Oracle VM VirtualBox
* **Imagem ISO Utilizada:** `ubuntu-26.04-live-server-amd64.iso`
* **Configurações Finais da Máquina Virtual (`pedrocha`):**
  * **Memória RAM:** 2048 MB (2 GB)
  * **Processador:** 1 vCPU
  * **Disco Rígido Virtual:** 32 GB (Tipo VDI, Dinamicamente Alocado)
  * **Rede:** Adaptador em modo NAT (Interface `enp0s3`)
<img width="440" height="676" alt="Captura de tela 2026-08-29 192033" src="https://github.com/user-attachments/assets/ca838156-e96c-43c3-925b-b7fe5210a83f" />
---

## 4. Procedimento Executado
1. **Organização do Host:** Alocação da imagem ISO de instalação no diretório correspondente da máquina física e criação da estrutura de pastas para isolamento do ambiente do aluno.
2. **Provisionamento da VM:** Configuração dos parâmetros de hardware iniciais no VirtualBox, definindo a vCPU, o disco rígido virtual VDI e ajustando a memória RAM para suporte à compilação de pacotes.
3. **Instalação do Sistema Operacional:** Execução do assistente de instalação do Ubuntu Server 26.04 LTS a partir do armazenamento óptico virtual.
4. **Validação via Linha de Comando (CLI):** Acesso ao terminal do servidor recém-instalado para verificação de parâmetros de rede, checagem do particionamento de disco e atualização do catálogo de pacotes.

---

## 5. Testes e Validação

### A. Validação de Endereço de Rede (`ip addr`)
Execução do comando para checagem das interfaces de rede do servidor:

```bash
ip addr
```
<img width="878" height="815" alt="Captura de tela 2026-08-29 184337" src="https://github.com/user-attachments/assets/acb4e534-39ba-4657-980e-e9c696e0cd68" />


Evidência: A interface enp0s3 ativou corretamente o link e obteve o endereço IPv4 10.0.2.15/24 via DHCP por meio do modo de rede NAT do VirtualBox.

### B. Mapeamento do Sistema de Arquivos (df -h)
Execução do comando para análise do espaço de armazenamento alocado:
```bash
df -h
```
<img width="895" height="810" alt="Captura de tela 2026-08-29 184355" src="https://github.com/user-attachments/assets/deac7c73-2413-4b83-9de5-8874772b0744" />

Evidência: A partição principal /dev/sda2 foi mapeada com sucesso e montada diretamente na raiz /, dispondo de uma capacidade total de 33 GB (com utilização otimizada de 3.1 GB e cerca de 28 GB livres).

### C. Atualização dos Repositórios (sudo apt-get update)
Validação do acesso externo e do gerenciador de pacotes do sistema:
```bash
sudo apt-get update
```
<img width="852" height="812" alt="Captura de tela 2026-08-29 184449" src="https://github.com/user-attachments/assets/9b332388-3906-4647-ad82-8ab971cdeb57" />


Evidência: O utilitário apt realizou com sucesso a comunicação com os espelhos oficiais do Ubuntu (archive.ubuntu.com), efetuando a leitura das listas de pacotes de segurança e atualizações sem interrupções.


### 6. Problemas Encontrados e Soluções:

----------------------------------------------------------------------------------------------------------------------------------------------------------

### Ocorrência Técnica N1:

Erro de compilação/instalação no estágio final (install_fail / curtin)
<img width="1292" height="796" alt="Captura de tela 2026-08-29 182221" src="https://github.com/user-attachments/assets/b1f31ff9-4acd-47e3-bc40-329fced3ba43" />


### Causa Identificada N1:

Insuficiência de recursos de memória RAM (alocação inicial padrão de 512 MB) durante a extração de pacotes do kernel e utilitários de sistema.

### Solução Aplicada N1:

Redimensionamento da memória Base da VM para 2048 MB (2 GB) nas configurações avançadas do sistema no VirtualBox, seguido de uma reinstalação limpa.

------------------------------------------------------------------------------------------------------------------------------------------------------------

### Ocorrência Técnica N2:

Omissão das telas manuais de particionamento e layout de teclado

### Causa Identificada N2:

Acionamento do mecanismo padrão de instalação automatizada (Unattended Installation) do VirtualBox ao reconhecer a imagem ISO.

### Solução Aplicada N2:

Utilização do ambiente funcional provisionado de forma autônoma com a conta de serviço vboxuser, documentando o comportamento do assistente do hipervisor.

--------------------------------------------------------------------------------------------------------------------------------------------------------------

### 7. Conclusão

A aula prática proporcionou uma sólida introdução ao gerenciamento de infraestrutura virtualizada. Através do Oracle VM VirtualBox, foi possível analisar na prática o comportamento de um hypervisor tipo 2 na alocação de recursos de hardware virtual (CPU, RAM e armazenamento dinâmico VDI). A validação dos comandos via interface de linha de comando assegurou o pleno funcionamento dos serviços de rede e do subsistema de arquivos do Ubuntu Server 26.04 LTS, atestando a eficácia do ambiente para futuras implementações de redes e servidores.


