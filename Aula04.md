# Relatório Técnico: Manipulação, Edição e Automação de Arquivos no Linux

## 1. Identificação
* **Título da Prática:** Edição de Arquivos, Permissões de Execução e Automação de Usuários em Lote
* **Aluno:** Pedro Henrique Cavalcante Rocha
* **Matrícula:** 2023007730
* **Turma:** 2026.02
* **Data de Realização:** 29 de Agosto de 2026

---

## 2. Objetivo
Esta prática teve como objetivo dominar comandos de manipulação e edição de arquivos no terminal do Linux Server, além de desenvolver scripts em Shell (`.sh`) para automatizar o gerenciamento de contas de usuários em lote, reduzindo o tempo de provisionamento e evitando erros manuais de administração.

---

## 3. Ambiente
* **Sistema Operacional Hospedeiro:** Windows 11
* **Hipervisor:** Oracle VM VirtualBox
* **Sistema Operacional Convidado:** Ubuntu Server 26.04 LTS
* **Recursos da VM:** 512 MB RAM / 1 CPU / 32 GB HD
* **Usuário Sudoer:** `vboxuser` (`vboxuser@pedrocha`)

---

## 4. Procedimento
1. **Manipulação Inicial:** Criação, cópia, movimentação e remoção interativa/forçada de arquivos utilizando os utilitários `touch`, `nano`, `cp`, `mv` e `rm`.
2. **Criação da Base de Dados de Entrada:** Elaboração do arquivo `usuarios.txt` contendo a lista dos usuários `aluno01` até `aluno20`.
3. **Desenvolvimento do Script de Criação (`passo1_criar.sh`):** Utilização da estrutura de repetição `for` e do utilitário `useradd -m -s /bin/bash` para criar diretórios home e definir a shell padrão de cada conta.
4. **Desenvolvimento do Script de Senhas (`passo2_senhas.sh`):** Implementação da automação com `chpasswd` enviando a combinação `usuario:senha` via pipe (`|`).
5. **Concessão de Execução e Teste:** Aplicação da permissão `chmod +x` em ambos os scripts e validação via `getent passwd`.

---

## 5. Testes e Evidências

### Execução dos Scripts em Lote
* A criação das 20 contas e a atualização automatizada das senhas foram concluídas sem falhas de sintaxe.
<img width="475" height="127" alt="Captura de tela 2026-08-30 004243" src="https://github.com/user-attachments/assets/5a9b4c98-5a09-45e9-98df-aa812a5bc4cc" />
<img width="428" height="428" alt="Captura de tela 2026-08-30 005335" src="https://github.com/user-attachments/assets/ac87c762-cb4b-45c4-8591-878d8b298248" />
<img width="456" height="401" alt="Captura de tela 2026-08-30 005444" src="https://github.com/user-attachments/assets/210b394a-a3f8-4893-a2aa-6589f488c692" />
<img width="482" height="141" alt="Captura de tela 2026-08-30 005612" src="https://github.com/user-attachments/assets/5a60ac61-8c93-4055-88f1-caae62700d95" />


### Validação na Base de Dados do Sistema
* A verificação com `getent passwd | tail -n 20` confirmou a inclusão correta de todos os usuários com ID, home e shell definidos.
<img width="404" height="359" alt="Captura de tela 2026-08-30 010414" src="https://github.com/user-attachments/assets/db30ca80-7e54-459f-be42-95a760a33543" />


### Teste de Acesso (Login Shell)
* Alternância para o usuário `aluno01` via `su - aluno01` utilizando a senha `aluno01`. O login direcionou o usuário corretamente para seu diretório `/home/aluno01`.
<img width="573" height="228" alt="Captura de tela 2026-08-30 010553" src="https://github.com/user-attachments/assets/512414a6-c999-4647-bf71-d35678f4ea45" />

---

## 6. Problemas e Soluções

### Ocorrência N°1: Permissão Negada ao Tentar Executar o Script (`Permission denied`)
* **Problema:** Ao tentar rodar `./passo1_criar.sh`, o terminal retornou a mensagem de permissão negada.
* **Causa Identificada:** Arquivos de texto criados via `nano` não possuem a flag de execução (`x`) habilitada por padrão no Linux por motivos de segurança.
* **Solução Aplicada:** Atribuição da permissão de execução simbólica através do comando `chmod +x passo1_criar.sh passo2_senhas.sh`.

### Ocorrência N°2: Falha de Elevação de Privilégios no `useradd`
* **Problema:** O script gerou mensagens de erro informando falta de privilégios para criar contas no sistema.
* **Causa Identificada:** O comando `useradd` exige permissões de superusuário (`root`).
* **Solução Aplicada:** Inclusão do prefixo `sudo` na linha do comando dentro do laço `for` (`sudo useradd -m -s /bin/bash $usuario`).

---

## 7. Conclusão
A prática evidenciou a importância da automação via Shell Script na administração de sistemas corporativos. A substituição do cadastro manual de dezenas de usuários por rotinas automatizadas com `for`, `useradd` e `chpasswd` otimiza o tempo de configuração e garante padronização e escalabilidade na gestão de infraestruturas Linux Server.
