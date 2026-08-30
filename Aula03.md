# Relatório Técnico: Estrutura de Diretórios (FHS) e Permissões Avançadas no Linux Server

## 1. Identificação
* **Título da Prática:** Análise da Estrutura FHS, Isolamento Departamental e Uso do Login Shell
* **Aluno:** Pedro Henrique Cavalcante Rocha
* **Matrícula:** 2023007730
* * **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Disciplina:** Laboratório de redes
* **Data de Realização:** 30 de Agosto de 2026

---

## 2. Objetivo
Esta prática teve como objetivo explorar a hierarquia padrão de diretórios do Linux (FHS), compreender o papel de pastas estratégicas do sistema como `/etc`, `/var`, `/srv` e `/tmp`, e aplicar uma estrutura de diretórios corporativos aninhados com isolamento rígido entre departamentos (`ti-dept`, `vendas-dept` e `diretoria-dept`), utilizando permissões octais (`770`/`660`) e validação via sessões de login limpas (`su -`).

---

## 3. Ambiente
* **Sistema Operacional Hospedeiro:** Windows 11
* **Hipervisor:** Oracle VM VirtualBox
* **Sistema Operacional Convidado:** Ubuntu Server
* **Usuário Administrador/Sudoer:** `vboxuser` (`vboxuser@pedrocha`)

---

## 4. Procedimento
A prática seguiu as seguintes etapas operacionais no terminal:

1. **Inspeção do Padrão FHS:** Navegação e análise da estrutura dos diretórios `/etc` (configurações locais) e `/var/log/auth.log` (logs de autenticação e histórico do `sudo`).
2. **Criação Recursiva de Diretórios:** Utilização da flag `-p` do comando `mkdir` para estruturar `/srv/ti-dept/projetos`, `/srv/vendas-dept/relatorios` e `/srv/diretoria-dept`.
3. **Gestão de Grupos Departamentais:** Criação dos grupos `ti-group`, `vendas-group` e `diretoria-group`, associando os usuários `fulano`, `cicrano` e `beltrano` aos seus respectivos setores.
4. **Aplicação de Posse e Isolamento Octal:** Alteração de donos e grupos (`chown`) e aplicação da permissão octal `770` (`drwxrwx---`) em `/srv/diretoria-dept` e `660` (`-rw-rw----`) no arquivo `orcamento_ti.txt`.
5. **Testes de Acesso:** Simulação de contexto de rede utilizando `su - <usuario>` para validar permissões de leitura, escrita e bloqueio executivo.

---

## 5. Testes e Evidências

### Desafio Prático: Diretório da Diretoria
* **Acesso Autorizado (`beltrano`):** Navegação até `/srv/diretoria-dept` e listagem do arquivo `orcamento_ti.txt` executadas com sucesso.
<img width="557" height="121" alt="Captura de tela 2026-08-29 231640" src="https://github.com/user-attachments/assets/abfd5b19-de54-4e08-8e7d-5ed45cb19d7c" />

<img width="493" height="121" alt="Captura de tela 2026-08-29 232415" src="https://github.com/user-attachments/assets/72008068-72e1-4b14-85a1-96c3b982e647" />


* **Acesso Negado (`fulano`):** Tentativa de acesso bloqueada com a mensagem de erro de sistema `-bash: cd: /srv/diretoria-dept: Permission denied`.
* <img width="448" height="100" alt="Captura de tela 2026-08-29 232459" src="https://github.com/user-attachments/assets/4840d397-4336-4e06-bb25-425f2d56eb6b" />


* **Validação de Permissões e Grupos:** Confirmação da máscara `drwxrwx---` no diretório e presença de `beltrano` em `diretoria-group`.
<img width="617" height="112" alt="Captura de tela 2026-08-29 232630" src="https://github.com/user-attachments/assets/6adc3591-e397-4b21-a977-7326ca3bc990" />


---

## 6. Problemas e Soluções

### Ocorrência Técnico-Operacional N°1: Contexto de Variáveis de Ambiente (`su` vs `su -`)
* **Problema:** Ao trocar de usuário apenas com o comando `su usuario`, o terminal mantinha o diretório de trabalho do usuário anterior, gerando mensagens confusas sobre acessos e permissões herdadas.
* **Causa Identificada:** O comando `su` altera apenas a identidade do usuário, preservando o ambiente e as variáveis de caminho do shell original.
* **Solução Aplicada:** Uso obrigatório do comando `su - usuario` (com o hífen) para forçar o carregamento de uma *Login Shell* completa e limpa, posicionando o usuário diretamente em sua `/home`.

---

## 7. Conclusão
A compreensão da árvore FHS e da aplicação estrita de permissões numéricas/octais permite garantir que servidores Linux mantenham a integridade de dados corporativos confidenciais. A combinação do comando `mkdir -p`, atribuição correta de grupos via `chown` e restrição octal `770` assegura que setores isolados operem de maneira independente, sem riscos de vazamento ou alteração não autorizada de informações sensíveis.
