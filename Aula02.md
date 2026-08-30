# Relatório Técnico: Administração de Usuários e Permissões no Linux

## 1. Identificação
* **Título da Prática:** Prática de Gerenciamento de Usuários, Grupos e Permissões em Servidor Linux
* **Aluno:** Pedro Henrique Cavalcante Rocha
* * **Matricula:** 2023007730
* * **Curso:** Bacharelado em Sistemas de Informação (BSI)
* * **Disciplina:** Laboratório de redes
 


---

## 2. Objetivo
Esta atividade prática teve como objetivo validar o gerenciamento de controle de acesso em ambiente Linux Server. Pretendeu-se criar e organizar usuários em grupos de trabalho, estruturar diretórios compartilhados em `/srv/` e aplicar a máscara octal de permissões (`770`) e de arquivos (`660`), garantindo que apenas membros autorizados de cada setor tenham acesso a conteúdos restritos.

---

## 3. Ambiente
* **Sistema Operacional Hospedeiro:** Windows 11
* **Hipervisor:** Oracle VM VirtualBox
* **Sistema Operacional Convidado:** Ubuntu Server
* **Usuário Administrador/Sudoer:** `vboxuser` (`vboxuser@pedrocha`)

---

## 4. Procedimento
A execução da prática foi dividida nas seguintes etapas no terminal:

1. **Criação de Usuários e Senhas:** 
   * Criação das contas `cicrano`, `beltrano` e `novato` utilizando o comando `sudo adduser`.
2. **Criação e Gestão de Grupos:**
   * Criação do grupo `devs` (`sudo groupadd devs`) e inclusão dos usuários `fulano`, `cicrano` e `beltrano` com `sudo usermod -aG devs`.
   * Criação do grupo `financeiro` (`sudo groupadd financeiro`) e inclusão exclusiva de `cicrano` e `beltrano`.
3. **Estruturação de Diretórios e Arquivos Compartilhados:**
   * Criação das pastas `/srv/projeto` e `/srv/financeiro` (`sudo mkdir -p`).
   * Alteração da propriedade dos diretórios para o usuário `vboxuser` e grupos correspondentes (`devs` e `financeiro`) via `sudo chown` e `sudo chgrp`.
4. **Aplicação de Permissões Octais:**
   * Aplicação da permissão `770` (`drwxrwx---`) em `/srv/projeto` e `/srv/financeiro` usando `sudo chmod 770`.
   * Ajuste das permissões do arquivo `config_redes.txt` para `660` (`-rw-rw----`) e alteração do seu grupo para `devs`.

---

## 5. Testes e Evidências

### Passo A e B: Grupo Devs e Projeto
* **Criação de Grupo e Adição de Usuários:** Validação no arquivo `/etc/group` confirmando `fulano`, `cicrano` e `beltrano` no grupo `devs`.
<img width="374" height="59" alt="Captura de tela 2026-08-29 222635" src="https://github.com/user-attachments/assets/75c20ea9-8217-401a-abe4-5264d7b1a66b" />


* **Teste de Edição de Arquivo por `fulano`:** Modificação e leitura do arquivo `config_redes.txt` concluídas com sucesso.
<img width="538" height="123" alt="Captura de tela 2026-08-29 224411" src="https://github.com/user-attachments/assets/8e013061-ec09-4d01-bbe3-c7b3059fcced" />



### Exercício Prático de Fixação: Setor Financeiro
* **Validação do Grupo `financeiro`:** Verificação via `grep "financeiro" /etc/group` garantindo que `fulano` foi removido e apenas `cicrano` e `beltrano` pertencem ao grupo (`financeiro:x:1006:cicrano,beltrano`).
<img width="433" height="54" alt="Captura de tela 2026-08-29 225632" src="https://github.com/user-attachments/assets/8f5ca43c-1f3c-40ad-adca-f53bee5ccd94" />


* **Validação do Diretório:** Execução do `ls -ld /srv/financeiro` retornando `drwxrwx--- 2 vboxuser financeiro`.
<img width="548" height="56" alt="Captura de tela 2026-08-29 225747" src="https://github.com/user-attachments/assets/e55da226-d8a3-481a-a5de-ac3dcbbe5ad0" />

* **Teste de Sucesso (`cicrano`):** Execução de `echo "Relatorio Financeiro" > /srv/financeiro/relatorio.txt` e `cat` sem erros.
<img width="673" height="110" alt="Captura de tela 2026-08-29 230244" src="https://github.com/user-attachments/assets/f8a411f9-f518-4d4e-ac24-2b264e39f096" />

* **Teste de Bloqueio (`fulano` e `novato`):** Tentativa de escrita retornando `-bash: ... Permission denied` para ambos os usuários.
<img width="673" height="108" alt="Captura de tela 2026-08-29 230609" src="https://github.com/user-attachments/assets/b335a178-4e1a-4204-b240-9c42d5d7f42c" />
<img width="653" height="114" alt="Captura de tela 2026-08-29 230512" src="https://github.com/user-attachments/assets/bcb857df-862d-4302-9e4f-64698e502030" />


---

## 6. Problemas e Soluções

### Ocorrência Técnico-Sintática N°1
* **Problema:** Retorno de erro `devs: command not found` ao tentar validar o grupo no terminal.
* **Causa Identificada:** Falta de espaço e uso incorreto de crases em vez de aspas no comando `grep`.
* **Solução Aplicada:** Correção da sintaxe no terminal utilizando aspas duplas e espaçamento correto: `grep "devs" /etc/group`.

### Ocorrência de Permissão N°2
* **Problema:** Retorno de `Permission denied` ao tentar editar o arquivo `/srv/projeto/config_redes.txt` com o usuário `fulano`.
* **Causa Identificada:** O arquivo foi criado mantendo o grupo original do criador (`vboxuser`) em vez do grupo do diretório (`devs`).
* **Solução Aplicada:** Alteração do grupo do arquivo para `devs` via `sudo chgrp devs /srv/projeto/config_redes.txt` e permissão `chmod 660`.

### Ocorrência de Associação N°3
* **Problema:** O usuário `fulano` foi vinculado por engano ao grupo `financeiro` no primeiro passo do exercício de fixação.
* **Causa Identificada:** Execução indevida do comando `sudo usermod -aG financeiro fulano`.
* **Solução Aplicada:** Remoção do usuário do grupo através do comando `sudo gpasswd -d fulano financeiro`.

---

## 7. Conclusão
A prática demonstrou como a correta associação entre usuários, grupos e a máscara de permissões POSIX (`chmod`) forma a base da segurança corporativa em servidores Linux. A definição precisa das permissões de Leitura, Escrita e Execução previne o acesso não autorizado a dados confidenciais de setores sensíveis (como o Financeiro), ao mesmo tempo em que garante a colaboração entre os membros legítimos de uma equipe.
