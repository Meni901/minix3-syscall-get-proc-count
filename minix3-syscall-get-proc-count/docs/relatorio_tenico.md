# Relatório Técnico: Situação e Motivos da Não Implementação da System Call `get_proc_count()`

**Autor:** Victor Ramos  
**Ambiente:** Windows 11, Oracle VirtualBox 7.2.22, VM `MV2` (4 CPUs, 2048 MB de RAM), MINIX 3  

---

## 1. Resumo da Situação

Neste trabalho, **eu não consegui implementar a system call `get_proc_count()` no MINIX 3**. Embora eu tenha criado a máquina virtual e instalado o sistema, os bloqueios para obter o código-fonte e configurar o ambiente impediram que eu modificasse o sistema operacional, compilasse o kernel/PM ou executasse um programa em C chamando a nova syscall de fato.

---

## 2. Motivos Pelos Quais Não Consegui Implementar

### 2.1. Diretório `/usr/src` inexistente e falha no `make build`
Quando tentei verificar o código-fonte e compilar o kernel original (`make build`), o terminal mostrou que `/usr/src` não existia (`No such file or directory`) e que o `make` não sabia como executar o alvo `build`. Sem `/usr/src`, não há base de código do MINIX 3 na VM para registrar ou compilar uma nova system call.

### 2.2. Download por rede (`git` e `pkgin`) não funcionou
Tentei obter o código-fonte pela rede dentro da máquina virtual usando `git` e `pkgin`, mas nenhuma dessas vias funcionou no meu ambiente.

### 2.3. Erro ao tentar montar a ISO no MINIX 3
Como eu tinha o arquivo `.iso` do MINIX 3, tentei criar `/mnt/iso` e montar `/dev/cd0a` usando `cd9660` para procurar os fontes localmente. O comando falhou com `mount: Can't mount /dev/cd0a on /mnt/iso: Invalid argument`. Além disso, no log do VirtualBox (`VBox.log`) da VM `MV2`, o drive óptico constava como vazio (`EmptyDrive = 1`), o que impediu acessar os arquivos da ISO por esse caminho.

### 2.4. Dificuldades gerais na configuração da VM e falta de execução prática
Com tantos erros seguidos na VM e sem conseguir extrair os fontes ou fazer o build funcionar, eu não cheguei a aplicar alterações na árvore real do sistema nem a rodar o binário de teste no MINIX 3. Os arquivos de código presentes neste repositório (`getproccount.c`, `get_proc_count.c` e `test_proc_count.c`) são apenas modelos/rascunhos gerados com apoio de IA para ilustrar a estrutura esperada, e não códigos testados em compilação na minha VM.

---

## 3. Como Seria o Fluxo da `get_proc_count()` na Teoria (Arquitetura Microkernel)

Mesmo sem conseguir implementar na prática, a lógica da chamada `get_proc_count()` na arquitetura microkernel do MINIX 3 envolveria:

1. **User Space (`test_proc_count.c`):** Um programa em C chama `get_proc_count()` esperando a quantidade de processos ativos.
2. **`libc` (`get_proc_count.c`):** A função da biblioteca monta uma mensagem IPC e chama `_syscall(PM_PROC_NR, PM_GETPROCCOUNT, &m)` para falar com o servidor do Process Manager.
3. **Microkernel:** Encaminha a mensagem de forma síncrona entre os processos/servidores por troca de mensagens (IPC).
4. **Process Manager (`getproccount.c`):** O PM receberia a chamada via `call_vec`, percorreria a tabela de processos `mproc` até `NR_PROCS` contando os processos com a flag `IN_USE` em `mp_flags` e devolveria essa contagem na mensagem de resposta.