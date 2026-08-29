# Relatório Técnico - Aula Prática 02: Administração de Usuários, Grupos e Permissões no Linux

## 1. Identificação
* **Nome Completo:** Pedro Henrique Cavalcante Rocha
* **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Disciplina:** Redes de Computadores / Sistemas Operacionais
* **Data:** 29/08/2026
* **Título da Prática:** Administração de Usuários, Grupos e Permissões no Ubuntu Server 26.04 LTS

---

## 2. Objetivo
Capacitar a administração de contas de usuários, grupos de trabalho e permissões no ambiente Ubuntu Server 26.04 LTS. A atividade focou no gerenciamento do ciclo de vida de usuários (`adduser`), criação e organização de grupos (`groupadd`, `usermod`), manipulação de propriedade de arquivos e diretórios (`chown`, `chgrp`) e definição de políticas de acesso via notação octal e simbólica (`chmod`). Por fim, validou-se o isolamento de segurança através da alternância de perfis de usuário no terminal.

---

## 3. Ambiente de Trabalho
* **Host (Máquina Física):** Windows 10/11
* **Hypervisor:** Oracle VM VirtualBox
* **Sistema Operacional Convidado:** Ubuntu Server 26.04 LTS
* **Máquina Virtual:** `pedrocha`
* **Usuário Administrador:** `administrador`
* **Estrutura de Usuários Criados:** `fulano`, `cicrano`, `beltrano`, `novato`
* **Grupos de Trabalho:** `devs`, `financeiro`

---

## 4. Procedimento Executado

1. **Criação das Contas de Usuários:**
   Utilização do comando `adduser` para criação de quatro usuários no sistema, garantindo a inicialização de seus respetivos diretórios em `/home` e atribuição de shells padrão (`/bin/bash`).
2. **Criação e Gestão de Grupos de Trabalho:**
   Criação dos grupos `devs` e `financeiro` através do comando `groupadd`. Em seguida, utilizou-se o utilitário `usermod -aG` para vincular os usuários aos seus respetivos grupos de atuação profissional.
3. **Provisionamento de Diretórios Compartilhados:**
   Criação das pastas `/srv/projeto` e `/srv/financeiro` no diretório de serviços do sistema para servir como repositórios colaborativos.
4. **Reatribuição de Propriedades (Dono e Grupo):**
   Alteração do dono dos diretórios para `administrador` (`chown`) e transferência do grupo associado para `devs` e `financeiro` (`chgrp`), retirando a propriedade padrão do usuário `root`.
5. **Aplicação de Políticas de Permissão (Notação Octal):**
   Aplicação da máscara de permissão `770` (`drwxrwx---`) nos diretórios compartilhados, garantindo controle total ao dono e ao grupo de trabalho, enquanto restringe completamente o acesso a usuários não autorizados (Outros).
6. **Resolução do Exercício Prático de Fixação:**
   * Criação do grupo `financeiro`.
   * Associação dos usuários `cicrano` e `beltrano` ao grupo `financeiro`.
   * Configuração de permissões estritas na pasta `/srv/financeiro` (`chmod 770`).
   * Teste de gravação e bloqueio de acesso entre perfis autorizados e não autorizados.

---

## 5. Testes e Evidências

### A. Validação de Criação de Usuários e Grupos
Inspecção das contas e checagem da associação do grupo `devs` nos arquivos de sistema `/etc/passwd` e `/etc/group`:

```bash
tail -n 4 /etc/passwd
grep "devs" /etc/group
